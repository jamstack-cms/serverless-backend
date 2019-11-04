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
``

