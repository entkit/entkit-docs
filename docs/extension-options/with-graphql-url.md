---
sidebar_position: 2
---

# GraphQL Config

Supply the endpoint for your GraphQL server.

## Example

```go title="entc.go"
entkit, err := entkit.NewExtension(
	...
    entkit.WithGraphqlURL("http://localhost/query"),
	...
)
```