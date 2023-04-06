---
sidebar_position: 4
---

# Auth Config

This annotation is designed to establish an authentication and authorization provider for GraphQL operations.

:::info

As of now, only the Keycloak provider is available for use.

:::


Available options [check here](/docs/authentication/introduction) 

## Example
```go title="entc.go"
entkit, err := entkit.NewExtension(
	...
    entkit.WithAuth(
        entkit.AuthWithKeycloak(
            entkit.NewKeycloak(
                ...
            ),
        ),
	),
	...
)
```