---
sidebar_position: 4
---

# With Meta

To provide metadata variables to the adapters:

## Example

```go title="entc.go"
entkit, err := entkit.NewExtension(
	...
    entkit.WithMeta("key", "value")
	...
)
```