---
sidebar_position: 2
---

# Setup guide


## 1. Add imports to your `entc.go` file
```diff title="entc.go"
package main

-import (
+import (
+ "github.com/entkit/entkit/v2"
+ "path/filepath"
```

## 2. Modify EntGQL init 
:::caution

To provide default configs to `entgql`, You must create `entgql` extension with `entkit` wrapper function.

:::

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

## 3. Configure `entkit` extension

```go title="entc.go"
entkitEx, err := entkit.NewExtension(
  // First UI app
  entkit.WithGenerator("refine-project", entkit.DefaultRefineAdapter),
  // Second UI app
  entkit.WithGenerator(
      "other-refine-project",
      entkit.DefaultRefineAdapter,
      entkit.TargetPath(filepath.Join("other-refine-project-root/project")),
  ),
  // The server app has a CLI interface that allows you to run your UIs with a single command.
  entkit.WithGenerator("my-awesome-server", entkit.DefaultServerAdapter),
  entkit.WithGraphqlURL("http://localhost/query"),
)

opts := []entc.Option{
  entc.Extensions(ex),
}
if err := entc.Generate("./ent/schema", &gen.Config{}, opts...); err != nil {
  log.Fatalf("running ent codegen: %v", err)
}
```

[Check available configurations](/docs/category/extension-options)   

## 4. Configure your entities, fields and actions

```go title="schema/company.go" {5,24-26,32-34,44-52}
package schema

import (
    ...
    "github.com/entkit/entkit/v2"
     ...
)

type Company struct {
    ent.Schema
}

func (Company) Fields() []ent.Field {
    return []ent.Field{
        field.UUID("id", uuid.UUID{}).
            Default(uuid.New).
            Annotations(
                entgql.OrderField("ID"),
            ),
        field.String("name").
            MaxLen(128).
            Annotations(
                 entgql.OrderField("NAME"), 
                 entkit.TitleField(), 
                 entkit.FilterOperator(gen.Contains),
                 entkit.View("MyCustomTitle"),
            ),
        field.String("description").
            MaxLen(512).
            Annotations(
                entgql.OrderField("DESCRIPTION"),
                entkit.RichTextField(),
                entkit.HideOnList(),
                entkit.FilterOperator(gen.Contains),
            ),
    }
}

func (Company) Annotations() []schema.Annotation {
    return []schema.Annotation{
        entgql.RelayConnection(),
        entgql.QueryField(),
        entgql.Mutations(entgql.MutationCreate(), entgql.MutationUpdate()),
        entkit.Icon("ShopOutlined"),
        entkit.IndexRoute(),
        entkit.RoutePath("com"),
        entkit.Actions(
            entkit.ListAction,
            entkit.ShowAction,
            entkit.DeleteAction,
            entkit.EditAction,
        ),
    }
}
```

[Check available configurations](/docs/category/entity-configuration)

## 5. Run go generate inside your `gen.go` base directory

```shell
go generate
```

## 6. Run server CLI

```shell title="Run helpp command"
go run ./my-awesome-server/*.go help
```

```shell title="Serve generated refine-project application"
go run ./my-awesome-server/*.go serve refine-project
```

:::info

Your application is now being served on port **[http://localhost:80](http://localhost:80)**

**If you need to modify it, you can do so by using arguments.**

:::

## More

:::info

Check out the example repository to see how the extension is used, along with a live demo showcasing its capabilities.

https://github.com/entkit/entkit-demo

:::

:::info

Find all extension configuration options here.

:::
