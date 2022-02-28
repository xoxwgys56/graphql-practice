# SDL

Schema Definition Language

## mutation

- creating new data
- updating existing data
- deleting data

```gql
mutation {
  createPerson(name: "Bob", age: 36) {
    name
    age
  }
}
```
