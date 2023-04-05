---
sidebar_position: 4
---

# With Auth

This annotation is designed to establish an authentication and authorization provider for GraphQL operations.

:::info

As of now, only the Keycloak provider is available for use.

:::


Available options
1. [Keycloak](#keycloak-usage-example) - **supporting** 
2. Auth0 - **not yet**
3. ...

## Keycloak usage example

```go title="entc.go"
entkit, err := entkit.NewExtension(
	...
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
	...
)
```