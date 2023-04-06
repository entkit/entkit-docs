---
sidebar_position: 4
---

# Metadata Config

To provide metadata variables to the adapters:

## Example

```go title="entc.go"
entkit, err := entkit.NewExtension(
	...
    entkit.WithMeta("key", "value")
	...
)
```