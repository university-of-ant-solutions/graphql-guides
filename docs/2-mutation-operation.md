---
description: Mutation Operation
keywords: graphql, mutation, operation, introduction
title: Mutation Operation
---

## Mutation

To form a mutation, you must specify three things:
  1. Mutation name. The type of modification you want to perform.
  2. Input object. The data you want to send to the server, composed of input fields. Pass it as an argument to the mutation name.
  3. Payload object. The data you want to return from the server, composed of return fields. Pass it as the body of the mutation name.

Mutations are structured like this:

  ```
  mutation {
    mutationName(input: {MutationNameInput!}) {
      MutationNamePayload
  }
  ```

The input object in this example is `MutationNameInput`,  and the payload object is `MutationNamePayload`.

In the [mutations](https://developer.github.com/v4/reference/mutation/) reference, the listed input fields are what you pass as the input object. The listed return fields are what you pass as the payload object.

For a real-world example, see "[Example mutation](https://developer.github.com/v4/guides/forming-calls/#example-mutation)."

### Example mutation

Mutations often require information that you can only find out by performing a query first. This example shows two operations:
  1. A query to get an issue ID.
  2. A mutation to add an emoji reaction to the issue.

```
query FindIssueID {
  repository(owner:"octocat", name:"Hello-World") {
    issue(number:349) {
      id
    }
  }
}

mutation AddReactionToIssue {
  addReaction(input:{subjectId:"MDU6SXNzdWUyMzEzOTE1NTE=",content:HOORAY}) {
    reaction {
      content
    }
    subject {
      id
    }
  }
}
```

Let's walk through the example. The task sounds simple: add an emoji reaction to an issue.

So how do we know to begin with a query? We don't, yet.

Because we want to modify data on the server (attach an emoji to an issue), we begin by searching the schema for a helpful mutation. The reference docs show the [addReaction](https://developer.github.com/v4/reference/mutation/addreaction/) mutation, with this description: `Adds a reaction to a subject`. Perfect!

The docs for the mutation list three input fields:
  - clientMutationId (String)
  - subjectId (ID!)
  - content (ReactionContent!)

The `!`s indicate that `subjectId` and `content` are required fields. A required `content` makes sense: we want to add a reaction, so we'll need to specify which emoji to use.

But why is `subjectId` required? It's because the `subjectId` is the only way to identify which issue in which repository to react to.

This is why we start this example with a query: to get the `ID`.

Let's examine the query line by line:
  - `query FindIssueID {`

    Here we're performing a query, and we name it FindIssueID. Note that naming a query is optional; we give it a name here so that we can include it in same Explorer window as the mutation

  - `repository(owner:"octocat", name:"Hello-World") {`

    We specify the repository by querying the repository object and passing owner and name arguments.

  - `issue(number:349) {`

    We specify the issue to react to by querying the issue object and passing a number argument.

  - `id`

    This is where we retrieve the `id` of `https://github.com/octocat/Hello-World/issues/349` to pass as the `subjectId`.

When we run the query, we get the `id`: `MDU6SXNzdWUyMzEzOTE1NTE=`

With the ID known, we can proceed with the mutation:

  - `mutation AddReactionToIssue {`

    Here we're performing a mutation, and we name it AddReactionToIssue. As with queries, naming a mutation is optional; we give it a name here so we can include it in the same Explorer window as the query.

  - `addReaction(input:{subjectId:"MDU6SXNzdWUyMzEzOTE1NTE=",content:HOORAY}) {`

    Let's examine this line:

      - `addReaction` is the name of the mutation.
      - `input` is the required argument key. This will always be `input` for a mutation.
      - `{subjectId:"MDU6SXNzdWUyMzEzOTE1NTE=",content:HOORAY}` is the required argument value. This will always be an [input object](https://developer.github.com/v4/reference/input_object/) (hence the curly braces) composed of input fields (`subjectId` and `content` in this case) for a mutation.

    How do we know which value to use for the content? The [addReaction](https://developer.github.com/v4/reference/mutation/addreaction/) docs tell us the `content` field has the type [ReactionContent](https://developer.github.com/v4/reference/enum/reactioncontent/), which is an [enum](https://developer.github.com/v4/reference/enum/) because only certain emoji reactions are supported on GitHub issues. The docs list the allowed values (note some values differ from their corresponding emoji names):

      - THUMBS_UP (:thumbsup:)
      - THUMBS_DOWN (:thumbsdown:)
      - LAUGH (:smile:)
      - HOORAY (:tada:)
      - CONFUSED (:confused:)
      - HEART (:heart:)

  - The rest of the call is composed of the payload object. This is where we specify the data we want the server to return after we've performed the mutation. These lines come from the [addReaction](https://developer.github.com/v4/reference/mutation/addreaction/) docs, which three possible return fields:

    - clientMutationId (String)
    - reaction (Reaction!)
    - subject (Reactable!)
  
  In this example, we return the two required fields (`reaction` and `subject`), both of which have required subfields (respectively, `content` and `id`).

When we run the mutation, this is the response:

```
{
  "data": {
    "addReaction": {
      "reaction": {
        "content": "HOORAY"
      },
      "subject": {
        "id": "MDU6SXNzdWUyMTc5NTQ0OTc="
      }
    }
  }
}
```

That's it! Check out your [reaction to the issue](https://github.com/octocat/Hello-World/issues/349) by hovering over the :tada: to find your username.

One final note: when you pass multiple fields in an input object, the syntax can get unwieldy. Moving the fields into a [variable](https://developer.github.com/v4/guides/forming-calls/#working-with-variables) can help. Here's how you could rewrite the original mutation using a variable:

```
mutation($myVar:AddReactionInput!) {
  addReaction(input:$myVar) {
    reaction {
      content
    }
    subject {
      id
    }
  }
}
variables {
  "myVar": {
    "subjectId":"MDU6SXNzdWUyMTc5NTQ0OTc=",
    "content":"HOORAY"
  }
}
```

You may notice that the `content` field value in the earlier example (where it's used directly in the mutation) does not have quotes around `HOORAY`, but it does have quotes when used in the variable. There's a reason for this:

  - When you use `content` directly in the mutation, the schema expects the value to be of type [ReactionContent](https://developer.github.com/v4/reference/enum/reactioncontent/), which is an enum, not a string. Schema validation will throw an error if you add quotes around the enum value, as quotes are reserved for strings.
  - When you use `content` in a variable, the variables section must be valid JSON, so the quotes are required. Schema validation correctly interprets the `ReactionContent` type when the variable is passed into the mutation during execution.










