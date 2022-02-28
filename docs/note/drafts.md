# drafts

which not sorted.

## endpoints

`graphql`은 single endpoint를 갖고 있습니다. 이는 multiple endpoint를 갖고 있는 경우(예를 들어, REST API)와 다른 점이 있습니다. 반환하는 데이터 형태가 다릅니다.
multiple endpoint를 갖고 있는 경우, 고정된 data strucuture를 반환하지만 `graphql`의 경우는 그렇지 않습니다.

만약 모든 유저가 작성한 Post의 **title**과 작성한 유저의 **name**만 가져오려면 아래와 같이 하면 됩니다:

```js
{
  allPersons {
    name
    posts {
      title
    }
  }
}
```

## mutation

`mutation`은 CRUD 중 create, update, delete를 위해 사용합니다. 아래는 예시입니다:

```js
// type
type Person {
  id: ID!
  name: String!
  age: Int!
}

// client request
mutation {
  createPerson(name: "Bob", age: 36) {
    name
    age
  }
}

// server response
"createPerson": {
  "name": "Bob",
  "age": 36,
}
```

여기서 우리는 Client가 id를 생성하지 않았다는 것을 알 수 있습니다. 원한다면, id를 포함해서 request 보낼 수 있습니다.

## subscription

realtime-update 필요한 경우 사용합니다.

```js
subscription {
  newPerson {
    name
    age
  }
}
```

## special root type

`Query`, `Mutation`, `Subscription`같은 특별한 type이 존재합니다. 만약 위에서 사용한 `allPerson` query를 사용하기 위해서는 server-side에 아래와 같은 정의가 필요합니다:

```js
type Query {
  allPersons(last: Int): [Person!]!
}
```

여기에서 `allPerson`은 API의 *root field*입니다.
`Mutation`도 마찬가지입니다:

```js
type Mutation {
  createPerson(name: String!, age: Int!): Person!
}
```

## dataIdFromObject

https://www.apollographql.com/blog/graphql/basics/the-concepts-of-graphql/
https://youtu.be/zWhVAN4Tg6M

query 중에 데이터가 수정되는 경우.

예를 들어, Arthur라는 작가의 coauthor를 찾는 경우 첫번째로 반환되는 값이 Adam일 수도 있다. 동시에 id=5인 author를 찾는 query의 반환값이 Adam일 수도 있다. (같은 작가를 반환) 이러한 형태로 우리는 같은 작가가 반환되기를 기대할 수 있는데
여기에서 만약 데이터가 중간에 수정된다면, consistency가 파괴된다.
이를 해결하기위해서 dataIsFromObject라는 방법을 사용.

GraphQL result data => unique id

```js
dataIdFromObject: (result) => {
  if (result.id && result.__typename) {
    return result.__typename + result.id;
  }
  return null;
};
```

apollo client가 이를 보장한다.

- Same path, same object
- Same ID, same object
