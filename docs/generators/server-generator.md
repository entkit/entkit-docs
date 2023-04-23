---
sidebar_position: 2
---

# Server and CLI generator

This generator creates a server application complete with a Command Line Interface (CLI) to serve your API backend and generate UIs for both development and production environments.

The CLI can override all parameters of the generated applications specified in your entc.go file. Thus, the default parameters in `entc.go` can serve as your development configuration.

For production configurations, you can provide them directly as command arguments through the server CLI or by using environment variables.

## Setup and configuration

Add server generator on your extension configuration by `entkit.WithGenerator` option. 

```go title="entc.go" {9}
...
entkitEx, err := entkit.NewExtension(
    entkit.WithGenerator("refine-project", entkit.DefaultRefineAdapter),
    entkit.WithGenerator(
        "other-refine-project",
        entkit.DefaultRefineAdapter,
        entkit.TargetPath(filepath.Join("other-refine-project-root/project")),
    ),
    entkit.WithGenerator("my-server", entkit.DefaultServerAdapter),
)
...
```

## Usage

### Serve you apps

After regenerating the code (using go generate), you can run your server application as follows:

#### Run only generated application

```shell
go run ./my-server/*.go serve refine-project
```

#### Run only api server

Since the app-server is enabled by default, you first need to disable it and then enable only the GraphQL server.

```shell
go run ./my-server/*.go serve --app-server-enabled=false --graphql-server-enabled=true refine-project
```

#### Run everything related to application
GraphQL server, GraphQL playground, App server etc.

Parameter: `--UnionServer` `--union-server` `-u`

```shell
go run ./my-server/*.go serve -u refine-project
```

#### All available serve command arguments

To view all available configuration arguments, run the `help` command.

```shell
go run ./my-server/*.go serve help
```

:::note

All available arguments can be configured using environment (ENV) variables.

e.g.

* `--graphql-uri-path`: GRAPHQL_URI_PATH

* `--app-server-enabled`: APP_SERVER_ENABLED

etc.

:::