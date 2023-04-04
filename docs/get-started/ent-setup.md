---
sidebar_position: 2
---

# Ent setup

## Modify your `entc.go` file

1. Add imports
    ```diff title="entc.go"
   package main
   
   -import (
   +import (
   + "github.com/entkit/entkit"
   + "path/filepath"
    ```
2. To provide default configs to `entgql`, let's create `entgql` extension with `entkit` wrapper function.
    ```diff title="entc.go"
    - entgqlEx, err := entgql.NewExtension(
    + ex, err := entkit.NewEntgqlExtension(
        // Tell Ent to generate a GraphQL schema for
        // the Ent schema in a file named ent.graphql.
        entgql.WithSchemaGenerator(),
        entgql.WithSchemaPath("ent.graphql"), 
        // other options...
    )
    ```
3. Configure `entkit` extension
    ```go title="entc.go"
    entkitEx, err := entkit.NewExtension(
        entkit.WithUIs(
            entkit.NewUI(filepath.Join("..", "typescript-project"), entkit.TypescriptAdapter), 
            entkit.NewUI(filepath.Join("..", "refine-project"), entkit.RefineAdapter), 
        ), 
        entkit.WithGraphqlURL("http://localhost/query"),
    )
   
   opts := []entc.Option{
        entc.Extensions(ex),
    }
    if err := entc.Generate("./ent/schema", &gen.Config{}, opts...); err != nil {
        log.Fatalf("running ent codegen: %v", err)
    }
    ```
4. Run go generate inside your `gen.go` base directory
    ```shell
    go generate
    ```


:::info

Find all extension configuration options here.

:::
