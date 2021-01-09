# My Notes

Followed the [tutorial](https://aws.amazon.com/getting-started/hands-on/build-react-app-amplify-graphql/) to build a simple web application using AWS Amplify.

- [My Notes](#my-notes)
  - [Build a Full-Stack React Application](#build-a-full-stack-react-application)
    - [Deploy and Host a React App](#deploy-and-host-a-react-app)
      - [Create a new React application](#create-a-new-react-application)
      - [Initialize GitHub repository](#initialize-github-repository)
      - [Deploy the app with AWS Amplify](#deploy-the-app-with-aws-amplify)
    - [Initialize a Local App](#initialize-a-local-app)
      - [Install the Amplify CLI](#install-the-amplify-cli)
      - [Configure the Amplify CLI](#configure-the-amplify-cli)
      - [Initialze the Amplify app locally](#initialze-the-amplify-app-locally)
    - [Add Authentication](#add-authentication)
      - [Install the Amplify libraries](#install-the-amplify-libraries)
      - [Create the authentication service](#create-the-authentication-service)
      - [Deploy the authentication service](#deploy-the-authentication-service)
      - [Configure the React project with Amplify resources](#configure-the-react-project-with-amplify-resources)
      - [Add the authentication flow in App.js](#add-the-authentication-flow-in-appjs)
      - [Set up CI/CD of the front end and backend](#set-up-cicd-of-the-front-end-and-backend)
      - [Deploy the changes to the live environment](#deploy-the-changes-to-the-live-environment)
    - [Add a GraphQL API and Database](#add-a-graphql-api-and-database)
      - [Create a GraphQL API and database](#create-a-graphql-api-and-database)
      - [Deploy the API](#deploy-the-api)
      - [Write front-end code to interact with the API](#write-front-end-code-to-interact-with-the-api)
    - [Add the Ability to Store Images](#add-the-ability-to-store-images)
      - [Create the storage service](#create-the-storage-service)
      - [Update the GraphQL schema](#update-the-graphql-schema)
      - [Deploy storage service and API updates](#deploy-storage-service-and-api-updates)
      - [Update the React app](#update-the-react-app)
    - [Deleing the resources](#deleing-the-resources)
      - [Removing individual services](#removing-individual-services)
      - [Deleing the entire project](#deleing-the-entire-project)

## Build a Full-Stack React Application

- Hosting
- Authentication
- Database and Storage

### Deploy and Host a React App

Create a React app and deploy and host through AWS Amplify.

- Create a **React** application
- Initialze a **GitHub** repository
- Deploy the app with AWS Amplify
- Implement code changes and redeploy the app

#### Create a new React application

```shell
npx create react-app amplifyapp
cd amplifyapp
npm start
```

#### Initialize GitHub repository

- Create a new GitHub repo
- Initialze git and push the app

  ```shell
  git init
  git remote add origin git@github.com:username/reponame.git
  git add .
  git commit -m "Initial commit"
  git push origin main
  ```

#### Deploy the app with AWS Amplify

- In the AWS Amplify service console, select "Get Started" under **Delivery** (**Host web app**).
- Select **GitHub** as the repo service.
- Authenticate with GitHub and Choose the **main** brance that created earlier.
- Accept the default build settings.
- Review the final details and select Save and Deploy.
- AWS Amplify will now build the source code and deploy at a URL. Once the build completes, to see the web app up and running.

### Initialize a Local App

- Install and configure the **Amplify CLI**
- Initiailze the Amplify app

#### Install the Amplify CLI

```shell
npm install -g @aws-amplify/cli
```

#### Configure the Amplify CLI

```shell
amplify configure
```

- This will open up the AWS console. Once logged into the console, we can jump back to the command line.
- Specify the **AWS region**.
- Specify the **username** of the new IAM user.
  - This will open up the IAM dashboard.
  - Set **permissions**: Attach existing policies directly: check **AdministratorAccess**.
  - Keep the **Access key ID** and **Secret access key**, then back to command line and paste them.
- Left the "Profile Name" to default.

#### Initialze the Amplify app locally

- In the Amplify console, click on **Backend environments**.
- Copy the amplify init command: `amplify pull --appId <appId> --envName staging`
- Initialze the Amplify project locally with the command.

### Add Authentication

- Install **Amplify libraries**
- Create and deploy an **authentication** service
- Configure the React app to include authentication

#### Install the Amplify libraries

```shell
npm install aws-amplify @aws-amplify/ui-react
```

#### Create the authentication service

```shell
amplify add auth

? Default configuration
? Username
? No, I an done.
```

#### Deploy the authentication service

```shell
amplify push --y
```

#### Configure the React project with Amplify resources

Add code in **src/index.js**. (import and config)

#### Add the authentication flow in App.js

Add code in **src/App.js**. (AmplifySignOut elemnt)

#### Set up CI/CD of the front end and backend

- AWS Amplify console > App settings > **Build settings**. Modify it to add backend section.
- Update the front end branch to point to the backend environment. Under the branch name, choose **Edit**, and then select the **staging** backend.

#### Deploy the changes to the live environment

Commit and push to origin main. In case the build fails.

> Error: JSONValidationError: File project: data should NOT have additional properties: 'graphqltransformer'.

Open the **Build settings** of the app in Amplify console. Build settings > Build image settings > Edit > **Package**. Specify the **version** of Amplify CLI installed (e.g. 4.41.1).

> Error: do not have a role.

Open the IAM > Access management > Roles > Create role > Choose a use case > select Amplify. Go back to AWS Amplify console. Select the app > App settings > General > app details > Edit > Set the **Service role**.

### Add a GraphQL API and Database

- Create and deploy a **GraphQL API**
- Write front-end code to interact with the API

#### Create a GraphQL API and database

```shell
amplify add api

? GraphQL
? notesapp
? API Key
? demo
? 7
? No, I am done.
? No
? Yes
? Single object with fields
? Yes
```

Open the GraphQL schema in text editor: **amplify/backend/api/myapi/schema.graphql**. Update the file.

#### Deploy the API

```shell
amplify push --y
```

To view the GraphQL API.

```shell
amplify console api

> Choose GraphQL
```

#### Write front-end code to interact with the API

Add code in **src/App.js**.

- fetchNotes
- createNote
- deleteNote

### Add the Ability to Store Images

- Create a **storage service**
- Update a GraphQL schema
- Update your React app

#### Create the storage service

```shell
amplify add storage

? Content
? imagestorage
? <your-unique-bucket-name>
? Auth users only
? create, read, update, delete
? N
```

#### Update the GraphQL schema

Update **amplify/backend/api/notesapp/schema.graphql**.

#### Deploy storage service and API updates

```shell
amplify push --y
```

#### Update the React app

Add code to use storage.

### Deleing the resources

#### Removing individual services

```shell
amplify remove auth

? <your-service-name>
```

Then push.

```shell
amplify push
```

#### Deleing the entire project

To delete the project and the associated resources.

```shell
amplify delete
```
