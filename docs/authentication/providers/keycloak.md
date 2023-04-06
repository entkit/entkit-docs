---
sidebar_position: 1
---

# Keycloak

Keycloak is an open source Identity and Access Management tool with features such as Single-Sign-On (SSO), Identity Brokering and Social Login, User Federation, Client Adapters, an Admin Console, and an Account Management Console.

[Check Keycloak Site](https://www.keycloak.org/)

## How `entkit` integrating Keycloak

`entkit` generates both backend and frontend clients, complete with authorization resources, permissions, policies, and roles, streamlining user management. To enforce backend security, Entkit utilizes resource and scope verification.

## Keycloak usage example

```go title="entc.go" {3-31}
entkit, err := entkit.NewExtension(
    entkit.WithAuth(
        entkit.AuthWithKeycloak(
            entkit.NewKeycloak(
                "http://localhost:8080", // Keycloak URL
                "entkit", // Realm name
                "admin", // Master admin username
                "admin", // Master admin password
                "entadmin", // Generated realm admin username
                "entadmin", // Generated realm admin password
                &gocloak.Client{
                    ClientID: gocloak.StringP("my-backend-client"),
                    Secret:   gocloak.StringP("my-backend-client-secret"),
                },
                &gocloak.Client{
                    ClientID: gocloak.StringP("my-frontend-client"),
                    RootURL:  gocloak.StringP("https://my-site.com"),
                    RedirectURIs: &[]string{
                        "https://my-site.com/*",
                        "http://localhost:3000/*",
						"http://localhost"
                    },
                    Attributes: &map[string]string{
                        "post.logout.redirect.uris": "+",
                    },
                    WebOrigins: &[]string{
                        "+",
                    },
                },
            ),
        ),
	),
)
```