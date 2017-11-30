---
description: Mutation API
keywords: graphql, mutation, operation, api
title: Mutation API
---

## Mutation

A GraphQL mutation is used to modify data. This is an example mutation:

```
mutation {
  createPost(
    title: "My great Vacation"
    slug: "my-great-vacation"
    published: true
    text: "Read about my great vacation."
  ) {
    id
    slug
  }
}
```

## Type mutations

For every available model type in your data model, you should provides `create`, `update` and `delete` mutation.

For example, if your schema contains a Post type:

```
type Post @model {
  id: ID!
  title: String!
  description: String
}
```

the following type mutations will be available:

- the createPost mutation creates a new node.

- the updatePost mutation updates an existing node.

- the deletePost mutation deletes an existing node.

### Creating a node

Create a new Post node and query its id and slug:

```
mutation {
  createPost(
    title: "My great Vacation"
    slug: "my-great-vacation"
    published: true
    text: "Read about my great vacation."
  ) {
    id
    slug
  }
}
```

### Updating a node

Update the text and published fields for an existing Post node and query its id:

```
mutation {
  updatePost(
    id: "cixnen24p33lo0143bexvr52n"
    text: "This is the start of my biggest adventure!"
    published: true
  ) {
    id
  }
}
```

### Deleting a node

Delete an existing Post node and query its (then deleted) id and title:

```
mutation {
  deletePost(id: "cixneo7zp3cda0134h7t4klep") {
    id
    title
  }
}
```

### Reference

- https://facebook.github.io/relay/graphql/mutations.htm (Good)

- https://www.graph.cool/docs/reference/graphql-api/mutation-api-ol0yuoz6go#overview

- https://facebook.github.io/relay/docs/graphql-mutations.html

- https://dev-blog.apollodata.com/designing-graphql-mutations-e09de826ed97
