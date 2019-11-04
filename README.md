## JAMstack CMS infrastructure as code

This is the back end infrastructure that powers the JAMstack CMS.

If you would like to build another CMS based on the JAMstack CMS, this is a great place to start. It includes the following:

1. GraphQL API with authorization rules.

2. Amazon S3 for storage of images, videos, and files.

3. Authentication service for user management and general authentication and authorization.

4. Lambda functions for image resizing and post-confirmation of users to add to groups.

### To deploy this backend with another app

1. Download the amplify folder into the root of the application you'd like to build

2. Run the following command:

```sh
amplify init
```

3. Update the schema to match your requirements.

4. To update any configuration, you can run the following commands:

```sh
amplify update storage

amplify update api

amplify update auth

amplify update function
```

### Updating / evolving the GraphQL schema

To update the schema, open the schema at *amplify/backend/api/jamstackcms*. Here is the base schema:

```graphql
  
type Post @model
  @auth (
      rules: [
          { allow: groups, groups: ["Admin"], operations: [create, update, delete] },
          { allow: private, operations: [read] },
          { allow: public, operations: [read] }
      ]
  ) {
  id: ID!
  title: String!
  description: String
  content: String!
  cover_image: String
  createdAt: String
  published: Boolean
  previewEnabled: Boolean
  categories: [String]
  author: User @connection
}

type Comment @model @auth(
  rules: [
    { allow: groups, groups: ["Admin"], operations: [create, update, delete] },
    { allow: owner, ownerField: "createdBy", operations: [create, update, delete] }
  ]
) {
  id: ID!
  message: String!
  createdBy: String
  createdAt: String
}

type Settings @model @auth(rules: [
    { allow: groups, groups: ["Admin"] },
    { allow: groups, groupsField: "adminGroups"},
    { allow: public, operations: [read] },
  ]) {
  id: ID!  
  categories: [String]
  adminGroups: [String]
  theme: String
  border: String
  borderWidth: Int
  description: String
}

type Preview @model
  @auth (
      rules: [
        { allow: groups, groups: ["Admin"] },
        { allow: private, operations: [read] }
      ]
  ) {
  id: ID!
  title: String!
  description: String
  content: String!
  cover_image: String
  createdAt: String
  categories: [String]
}

type Page @model(subscriptions: null)
  @auth (
      rules: [
        { allow: groups, groups: ["Admin"] },
        { allow: private, operations: [read] },
        { allow: public, operations: [read] }
      ]
  )
{
  id: ID!
  name: String!
  slug: String!
  content: String!
  components: String
  published: Boolean
}

type User @model
  @auth(rules: [
    { allow: owner },
    { allow: groups, groups: ["Admin"]},
    { allow: public, operations: [read] }
  ])
{
  id: ID!
  name: String
  username: String
  avatarUrl: String
}
```

Once you've made the updates, run the following command to test the new API:

```sh
amplify mock
```

Once you've tested the updates, run the following command to deploy the new API:

```sh
amplify push
```