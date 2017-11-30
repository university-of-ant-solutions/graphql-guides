---
description: Naming Conventions
keywords: graphql, naming, conventions
title: Naming Conventions
---

## Naming Conventions

Different objects you encounter in a Graphcool service like types or relations follow separate naming conventions to help you distinguish them.

### Types

The type name determines the name of derived queries and mutations as well as the argument names for nested mutations. Type names can only contain **alphanumeric characters** and need to start with an uppercase letter. They can contain **maximally 64 characters**.

*It's recommended to choose type names in the singular form.*
*Type names are unique on a service level.*

##### Examples

* `Post`
* `PostCategory`

### Scalar and Relation Fields

The name of a scalar field is used in queries and in query arguments of mutations. Field names can only contain **alphanumeric characters** and need to start with a lowercase letter. They can contain **maximally 64 characters**.

The name of relation fields follows the same conventions and determines the argument names for relation mutations.

*It's recommended to only choose plural names for list fields*.
*Field names are unique on a type level.*

##### Examples

* `name`
* `email`
* `categoryTag`

### Enums

Enum values can only contain **alphanumeric characters and underscores** and need to start with an uppercase letter.
The name of an enum value can be used in query filters and mutations. They can contain **maximally 191 characters**.

*Enum names are unique on a service level.*
*Enum value names are unique on an enum level.*

##### Examples

* `A`
* `ROLE_TAG`
* `RoleTag`

### Mutations

* Create

Eg: `addProjectCard, addProjectColumn`

* Update

Eg: `updateProject, updateProjectCard`

* Delete

Eg: `deleteProject, deleteProjectCard`

* Mutation Input

Eg: `AddCommentInput, CreditCardPaymentInput`

* Mutation Result

Eg: `AddCommentPayload, CheckoutAttributesUpdatePayload`

### Reference

- https://www.graph.cool/docs/reference/database/data-modelling-eiroozae8u/#types

- https://github.com/graphcool/framework/blob/master/docs/04-Reference/03-Database/02-Data-Modelling.md