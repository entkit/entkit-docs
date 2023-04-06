# Description Annotation
Use the `entkit.Description` annotation to assign a description to an entity, which will then be displayed in the related locations.

## Example
```go {6}
func (Gadgent) Annotations() []schema.Annotation {
	return []schema.Annotation{
		entgql.RelayConnection(),
		entgql.QueryField(),
		entgql.Mutations(entgql.MutationCreate(), entgql.MutationUpdate()),
		entkit.Description("Mobile phones, tablets, earphones, etc."),
		...
	}
}
```