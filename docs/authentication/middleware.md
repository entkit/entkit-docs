---
sidebar_position: 2
---

# Authentication Middleware

Authentication middleware is a go Http Handler add necessary data for authentication and authorization(permissions).

## Available middlewares

### Go standard HTTP library

Ref: `ent.EntkitAuthMiddleware`

#### Example
```go title="server.go" {37}
package main

import (
	"context"
	"log"
	"net/http"

	"todo"
	"todo/ent"
	"todo/ent/migrate"

	"entgo.io/ent/dialect"
	"github.com/99designs/gqlgen/graphql/handler"
	"github.com/99designs/gqlgen/graphql/playground"

	_ "github.com/mattn/go-sqlite3"
)

func main() {
	// Create ent.Client and run the schema migration.
	client, err := ent.Open(dialect.SQLite, "file:ent?mode=memory&cache=shared&_fk=1")
	if err != nil {
		log.Fatal("opening ent client", err)
	}
	if err := client.Schema.Create(
		context.Background(),
		migrate.WithGlobalUniqueID(true),
	); err != nil {
		log.Fatal("opening ent client", err)
	}

	// Configure the server and start listening on :8081.
	srv := handler.NewDefaultServer(todo.NewSchema(client))
	http.Handle("/",
		playground.Handler("Todo", "/query"),
	)
	http.Handle("/query", ent.EntkitAuthMiddleware(srv))
	log.Println("listening on :8081")
	if err := http.ListenAndServe(":8081", nil); err != nil {
		log.Fatal("http server terminated", err)
	}
}
```
### Gin middleware

Ref: `ent.EntkitGinAuthMiddleware`