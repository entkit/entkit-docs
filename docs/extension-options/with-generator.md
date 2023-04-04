---
sidebar_position: 2
---

# With Generator

This annotation aims to delineate supplementary UI generators

## Available adapters

Currently, there are only two available UI adapters to choose from.

1. `entkit.RefineAdapter` - generate powerful ui based on your definitions
2. `entkit.TypescriptAdapter` - generate typescript type definitions

## Example

```go title="entc.go"
entkit, err := entkit.NewExtension(
	...
    entkit.WithGenerator(filepath.Join("..", "my-frontend"), entkit.RefineAdapter),
    entkit.WithGenerator(filepath.Join("..", "type-definitions"), entkit.TypescriptAdapter),
    ...
)
```