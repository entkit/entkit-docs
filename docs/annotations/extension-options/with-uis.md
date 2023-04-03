---
sidebar_position: 2
---

# With UIs

This annotation aims to delineate supplementary UI generators

## Available adapters

Currently, there are only two available UI adapters to choose from.

1. `entkit.RefineAdapter` - generate powerful ui based on your definitions
2. `entkit.TypescriptAdapter` - generate typescript type definitions

## Example

```go title="entc.go"
entkit, err := entkit.NewExtension(
	...
    entkit.WithUIs(
        entkit.NewUI(filepath.Join("..", "refine-project"), entkit.RefineAdapter),
        entkit.NewUI(filepath.Join("..", "typescript-project"), entkit.TypescriptAdapter),
        ...
    ),
	...
)
```