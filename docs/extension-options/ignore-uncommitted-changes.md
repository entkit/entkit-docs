---
sidebar_position: 3
---

# Commitment Config

To bypass the **uncommitted changes warning** error message during generation, you can apply this specific annotation.

## Example

```go title="entc.go"
entkit, err := entkit.NewExtension(
	...
    entkit.IgnoreUncommittedChanges(),
	...
)
```