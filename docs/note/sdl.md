# SDL

Schema Definition Language

## mutation

- creating new data
- updating existing data
- deleting data

```js
mutation {
  createPerson(name: "Bob", age: 36) {
    name
    age
  }
}
```
