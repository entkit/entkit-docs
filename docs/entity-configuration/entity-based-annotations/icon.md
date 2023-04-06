# Icon Annotation
Use the `entkit.Icon` annotation to assign an icon to an entity, which will then be displayed in the navigation menu, lists, breadcrumbs, and other related locations.

## Example
```go {6}
func (MyEntity) Annotations() []schema.Annotation {
	return []schema.Annotation{
		entgql.RelayConnection(),
		entgql.QueryField(),
		entgql.Mutations(entgql.MutationCreate(), entgql.MutationUpdate()),
		entkit.Icon("GlobalOutlined"),
		...
	}
}
```