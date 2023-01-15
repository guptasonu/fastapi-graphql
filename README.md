# fastapi-graphql
## Developing an API with FastAPI and GraphQL

Dillinger is a cloud-enabled, mobile-ready, offline-storage compatible,
AngularJS-powered HTML5 Markdown editor.

- Explain why you may want to use GraphQL over REST
- Use Orator ORM to interact with a Postgres database
- Describe what Schemas, Mutations, and Queries are in GraphQL
- Integrate GraphQL into a FastAPI app with Graphene
- Test a GraphQL API with Graphene and pytest

## Features
- REST is the standard for building web APIs. With REST, you have multiple endpoints for each CRUD operation: GET, POST, PUT, DELETE. Data is gathered by accessing a number of endpoints.
- For example, if you wanted to get a particular user's profile info along with their posts and relevant comments, you would need to call four different endpoints:

> /users/<id> returns the initial user data
> /users/<id>/posts returns all posts for a given user
> /users/<post_id>/comments returns a list of comments per post
> /users/<id>/comments returns a list of comments per user
- This can result in request overfetching since you'll probably have to get much more data than you need.

Moreover, since one client may have much different needs than other clients, request overfetching and underfetching are common with REST.

GraphQL, meanwhile, is a query language for retrieving data from an API. Instead of having multiple endpoints, GraphQL is structured around a single endpoint whose return value is dependent on what the client wants instead of what the endpoint returns.

In GraphQL, you would structure a query like so to obtain a user's profile, posts, and comments:
```
query {
  User(userId: 2){
    name
    posts {
      title
      comments {
        body
      }
    }
    comments {
      body
    }
  }
}
```
# Project Setup
## Create a directory to hold your project called "fastapi-graphql":
```
$ mkdir fastapi-graphql
$ cd fastapi-graphql
```
## Create a virtual environment and activate it:
```
$ python3.9 -m venv env
$ source env/bin/activate

(env)$
```
## Create the following files in the "fastapi-graphql" directory:
```
main.py
db.py
requirements.txt
```
# Install the dependencies:
```
(env)$ pip install -r requirements.txt
```
# Models
## Next, let's create models for users, posts, and comments.
## Starting with users, create the model and the corresponding migration:
```
(env)$ orator make:model User -m

```
## If all goes well, Orator will automatically create a "models" and "migrations" directories in your project's root directory:

```
├── db.py
├── main.py
├── migrations
│   ├── 2020_10_26_191507_create_users_table.py
│   └── __init__.py
├── models
│   ├── __init__.py
│   └── user.py
└── requirements.txt
```
## Create the model and migration files for Post and Comments:

```
(env)$ orator make:model Post -m
(env)$ orator make:model Comments -m
```

## To apply the migration, run:
```
$ orator migrate -c db.py
```
# Graphene
## Start up your server:
```
(env)$ uvicorn main:app --reload
```
# Mutations
## Mutations are used in GraphQL to modify data. So, they are used for creating, updating, and deleting data.

```
mutation createUser {
  createUser(userDetails: {
    name: "John Doe",
    address: "Some address",
    phoneNumber: "12345678",
    sex: "male"
  })
  {
    id
    name
    posts {
      body
      comments {
        body
      }
    }
  }
}
```

# Queries
## To retrieve data, as a list or a single object, GraphQL provides us with a Query.
```
query getAllUsers {
  listUsers {
    id
    name
    posts {
      title
    }
  }
}
```
