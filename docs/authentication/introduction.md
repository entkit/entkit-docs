# Authentication

Entkit offers robust authentication for Entgo-generated GraphQL and OpenAPI APIs. By generating authorization resources, permissions, policies, and roles, Entkit creates a powerful authentication and authorization platform for your API using the most popular providers in the industry.

## Supporting providers
1. [Keycloak](/docs/authentication/providers/keycloak)
2. [Casbin](/docs/authentication/providers/casbin)

## Configuration

1. Initially, you need to configure your extension.
    ```go title="entc.go"
    entkit, err := entkit.NewExtension(
        entkit.WithAuth(
            entkit.AuthWithKeycloak( ... ) // Check your preferred provider configuration
        )
    )
    ```
2. Run `go generate` to generate required resources
3. Setup [Entkit Authentication middleware](/docs/authentication/middleware)
4. Configure your resolver resources and scopes enforcements
   ```go title="ent.resolvers.go"
   // Companies is the resolver for the companies field.
   func (r *queryResolver) Companies(ctx context.Context, after *ent.Cursor, first *int, before *ent.Cursor, last *int, orderBy *ent.CompanyOrder, where *ent.CompanyWhereInput, q *string) (*ent.CompanyConnection, error) {
       // Default Action Scopes
       // ActionReadScope     = "Read"
       // ActionCreateScope   = "Create"
       // ActionUpdateScope   = "Update"
       // ActionDeleteScope   = "Delete"
       // You can also provide your custom actions scopes too
       err := ent.EntkitAuthorizeByResource(ctx, "Company", entkit.ActionReadScope)
       if err != nil {
           return nil, err
       }
       return r.client.Company.Query().
           Paginate(ctx, after, first, before, last,
               ent.WithCompanyOrder(orderBy),
               ent.WithCompanyFilter(where.ApplySearchQuery(q).Filter),
           )
   }
   ```
   ```go title="mutations.resolvers.go"
   // CreateCompany is the resolver for the createCompany field.
   func (r *mutationResolver) CreateCompany(ctx context.Context, input ent.CreateCompanyInput) (*ent.Company, error) {
       err := ent.EntkitAuthorizeByResource(ctx, "Company", entkit.ActionCreateScope)
       if err != nil {
           return nil, err
       }
       return ent.FromContext(ctx).Company.
           Create().
           SetInput(input).
           Save(ctx)
   }
   ```
5. Run your API server

:::info

To test your configuration, authorize your requests using the Bearer token supplied by your preferred authentication provider.

:::

:::caution

Entkit generates a default user within your chosen authentication provider.

Default admin credentials:

username: `entadmin`

password: `entadmin`

:::