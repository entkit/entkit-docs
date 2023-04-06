# Label Annotation
Use the `entkit.Label` annotation to assign a label to an entity, which will then be displayed in the navigation menu, lists, breadcrumbs, and other related locations.

## Example
```go {6}
func (MobilePhone) Annotations() []schema.Annotation {
	return []schema.Annotation{
		entgql.RelayConnection(),
		entgql.QueryField(),
		entgql.Mutations(entgql.MutationCreate(), entgql.MutationUpdate()),
		entkit.Label("Mobile Phone"),
		...
	}
}
```