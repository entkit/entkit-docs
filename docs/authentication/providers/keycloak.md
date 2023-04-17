---
sidebar_position: 1
---

# Keycloak

Keycloak is an open source Identity and Access Management tool with features such as Single-Sign-On (SSO), Identity Brokering and Social Login, User Federation, Client Adapters, an Admin Console, and an Account Management Console.

[Check Keycloak Site](https://www.keycloak.org/)

## How `entkit` integrating Keycloak

`entkit` generates both backend and frontend clients, complete with authorization resources, permissions, policies, and roles, streamlining user management. To enforce backend security, Entkit utilizes resource and scope verification.

## Keycloak usage example

```go title="entc.go" {3-27}
entkit, err := entkit.NewExtension(
    entkit.WithAuth(
        entkit.AuthWithKeycloak(
            entkit.KeycloakHost("http://localhost:8080"),
            entkit.KeycloakRealm("entkit-demo-3"),
            entkit.KeycloakMasterAdminCredentials("admin", "admin"),
            entkit.KeycloakGeneratedAdminCredentials("entadmin", "entadmin"),
            entkit.KeycloakFrontendClientConfig(gocloak.Client{
                ClientID: gocloak.StringP("frontend"),
                RootURL:  gocloak.StringP("https://demo.entkit.com"),
                RedirectURIs: &[]string{
                    "https://demo.entkit.com/*",
                    "http://localhost:3000/*",
                    "http://localhost/*",
                },
                Attributes: &map[string]string{
                    "post.logout.redirect.uris": "+",
                },
                WebOrigins: &[]string{
                    "+",
                },
            }),
            entkit.KeycloakBackendClientConfig(gocloak.Client{
                ClientID: gocloak.StringP("backend"),
                Secret:   gocloak.StringP("test-secret"),
            }),
        ),
    ),
)
```