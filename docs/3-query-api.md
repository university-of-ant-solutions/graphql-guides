---
description: Query API
keywords: graphql, query, operation, api
title: Query API
---

## Queries

A GraphQL query is used to fetch data from a GraphQL endpoint. This is an example query:

```

query {
  posts {
    id
    title
    published
  }
}

```

Result

```
{
  "data": {
    "posts": [
      {
        "id": "cixnen24p33lo0143bexvr52n",
        "title": "My biggest Adventure",
        "published": false
      },
      {
        "id": "cixnenqen38mb0134o0jp1svy",
        "title": "My latest Hobbies",
        "published": true
      },
      {
        "id": "cixneo7zp3cda0134h7t4klep",
        "title": "My great Vacation",
        "published": true
      }
    ]
  }
}
```

## Type queries

### Fetching a single node

For each model type in your service, you should provides an query to fetch one specific node of that type. To specify the node, all you need to provide is its id or another unique field.

Query a specific post by its id:

```
query {
  post(id: "cixnen24p33lo0143bexvr52n") {
    id
    title
    published
  }
}
```

Result

```
{
  "data": {
    "post": {
      "id": "cixnen24p33lo0143bexvr52n",
      "title": "My biggest Adventure",
      "published": false
    }
  }
}
```

Specifying the node by another unique field

You can also supply any unique field as an argument to the query to identify a node. For example, if you already declared the slug field of the Post type to be unique, you could select a post by specifying its slug:
Query a specific Post node by its slug:

```
query {
  post(slug: "my-biggest-adventure") {
    id
    slug
    title
    published
  }
}
```

Result

```
{
  "data": {
    "post": {
      "id": "cixnen24p33lo0143bexvr52n",
      "slug": "my-biggest-adventure",
      "title": "My biggest Adventure",
      "published": false
    }
  }
}
```

### Fetch multiple nodes

You should provides queries to fetch all nodes of a certain model type. For example, for the `post` type the top-level query `posts` will be created.

#### Pagination

Pagination allows you to request a certain amount of nodes at the same time. You can seek forwards or backwards through the nodes and supply an optional starting node:

- to seek forwards, use first; specify a starting node with after.

- to seek backwards, use last; specify a starting node with before.

You can also skip an arbitrary amount of nodes in whichever direction you are seeking by supplying the skip argument.

Consider a blog where only 3 Post nodes are shown at the front page. To query the first page:

```
query {
  allPosts(first: 3) {
    id
    title
  }
}
```

To query the first two Post node after the first Post node:

```
query {
  allPosts(
    first: 2
    after: "cixnen24p33lo0143bexvr52n"
  ) {
    id
    title
  }
}
```

We could reach the same result by combining first and skip:

```
query {
  allPosts(
    first: 2
    skip: 1
  ) {
    id
    title
  }
}
```

Query the last 2 posts:

```
query {
  allPosts(last: 2) {
    id
    title
  }
}
```

Note: You cannot combine first with before or last with after. If you query more nodes than exist, your response will simply contain all nodes that actually do exist in that direction.

#### Result of Pagination

It is a new type. It should be end with “Connection”. Like RepositoryConnection! .v.v. Data struct:

```
{
  edges[RepositoryEdge]: {
    cursor: String!
    node: Repository
  }
  nodes: [Data]
  pageInfo[PageInfo!]: {
    endCursor: String
    hasNextPage: Boolean!
    hasPreviousPage: Boolean!
    startCursor: String
  }
  totalCount: Int!
}
```

### Reference

- http://facebook.github.io/relay/docs/graphql-connections.html

- https://www.graph.cool/docs/reference/graphql-api/query-api-nia9nushae#overview
