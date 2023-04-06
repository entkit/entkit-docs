# Route Path Annotation
Utilize the `entkit.RoutePath` annotation to assign a route path to an entity, which will then modify the URL route associated with that particular entity.

## Example

* `/mobile-phone` -> `/phone`
* `/mobile-phone/create` -> `/phone/create`
* `/mobile-phone/edit` -> `/phone/edit`
* `/mobile-phone/show` -> `/phone/show`
* ...

```go {6}
func (MobilePhone) Annotations() []schema.Annotation {
	return []schema.Annotation{
		entgql.RelayConnection(),
		entgql.QueryField(),
		entgql.Mutations(entgql.MutationCreate(), entgql.MutationUpdate()),
		entkit.RoutePath("phone"),
		...
	}
}
```