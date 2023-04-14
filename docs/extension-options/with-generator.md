---
sidebar_position: 2
---

# Generators Config

This annotation aims to delineate supplementary UI generators

## Available adapters

Currently, there are only two available UI adapters to choose from.

1. `entkit.DefaultRefineAdapter` - generate UI based on your definitions
2. `entkit.DefaultServerAdapter` - generate server with CLI to run your apps on development and production modes
3. `entkit.DefaultTypescriptAdapter` - generate typescript type definitions

## Example

```go title="entc.go"
entkit, err := entkit.NewExtension(
	...
    entkit.WithGenerator("my-frontend", entkit.DefaultRefineAdapter),
    entkit.WithGenerator("my-second-frontend", entkit.DefaultRefineAdapter, entkit.TargetPath(filepath.Join("other-refine-project-root/project"))),
    entkit.WithGenerator("typescript-defs", entkit.DefaultTypescriptAdapter),
    entkit.WithGenerator("my-awesome-server", entkit.DefaultServerAdapter),
    ...
)
```

:::info

Check out the example repository to see how the extension is used, along with a live demo showcasing its capabilities.

https://github.com/entkit/entkit-demo

:::