---
description: Query Operation
keywords: graphql, query, operation, introduction
title: Query Operation
---

## Queries
GraphQL queries return only the data you specify. To form a query, you must specify [fields within fields](https://developer.github.com/v4/guides/intro-to-graphql#field) (also known as nested subfields) until you return only [scalars](https://developer.github.com/v4/reference/scalar/).

Queries are structured like this:

```
query {
  JSON objects to return
}
```

For a real-world example, see "[Example query](https://developer.github.com/v4/guides/forming-calls/#example-query)."

### Example query

Let's walk through a more complex query and put this information in context.

The following query looks up the octocat/Hello-World repository, finds the 20 most recent closed issues, and returns each issue's title, URL, and first 5 labels:

```
query {
  repository(owner:"octocat", name:"Hello-World") {
    issues(last:20, states:CLOSED) {
      edges {
        node {
          title
          url
          labels(first:5) {
            edges {
              node {
                name
              }
            }
          }
        }
      }
    }
  }
}
```

Looking at the composition line by line:

* `query {`

Because we want to read data from the server, not modify it, `query` is the root operation. (If you don't specify an operation, `query` is also the default.)

* `repository(owner:"octocat", name:"Hello-World") {`

To begin the query, we want to find a [repository](https://developer.github.com/v4/reference/object/repository/) object. The schema validation indicates this object requires an `owner` and a `name` argument.

* `issues(last:20, states:CLOSED) {`

To account for all issues in the repository, we call the `issues` object. (We could query a single `issue` on a `repository`, but that would require us to know the number of the issue we want to return and provide it as an argument.)

Some details about the `issues` object:

  * The [docs](https://developer.github.com/v4/reference/object/repository/) tell us this object has the type `IssueConnection`.
  
  * Schema validation indicates this object requires a `last` or `first` number of results as an argument, so we provide `20`.

  * The [docs](https://developer.github.com/v4/reference/object/repository/) also tell us this object accepts a `states` argument, which is an [IssueState](https://developer.github.com/v4/reference/enum/issuestate/) enum that accepts `OPEN` or `CLOSED` values. To find only closed issues, we give the `states` key a value of `CLOSED`.

* `edges {`

We know `issues` is a connection because it has the `IssueConnection` type. To retrieve data about individual issues, we have to access the node via `edges`.

* `node {`

Here we retrieve the node at the end of the edge. The [IssueConnection docs](https://developer.github.com/v4/reference/object/issueconnection) indicate the node at the end of the `IssueConnection` type is an `Issue` object.

Now that we know we're retrieving an `Issue` object, we can look at the [docs](https://developer.github.com/v4/reference/object/issue) and specify the fields we want to return:

```
title
url
labels(first:5) {
  edges {
    node {
      name
    }
  }
}
```

Here we specify the `title`, `url`, and `labels` fields of the `Issue` object.

The `labels` field has the type [LabelConnection](https://developer.github.com/v4/reference/object/labelconnection/). As with the `issues` object, because `labels` is a connection, we must travel its edges to a connected node: the `label` object. At the node, we can specify the label object fields we want to return, in this case, `name`.

You may notice that running this query on the Octocat's public `Hello-World` repository won't return many labels. Try running it on one of your own repositories that does use labels, and you'll likely see a difference.


## Links

[about-query-and-mutation-operations](https://developer.github.com/v4/guides/forming-calls/#about-query-and-mutation-operations)