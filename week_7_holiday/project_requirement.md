# Building a Todo Api

- You are tasked by a client to build an API for his very popular todo mobile app.

- Here are the requirements:
  - Users have to register with the following details:
    - First Name (string)
    - Last Name (string)
    - Email (string)
    - Password (string)

  - Users can create task and flag tasks as completed. The task model should have these attributes:
    - Description (string)
    - Complete (boolean)

- These are the API endpoints and params sent from the `mobile app` :

  - Login user:
    - POST /api/v1/sessions
    - params: { user: { email: "user@email.com", password: "password" }, format: "json" }
    - response: { message: "message", auth_token: token }

  - Fetch tasks:
    - GET /api/v1/tasks
    - no params sent
    - Authenticated Users sends an ['Authorization'] header to the server.
    - response: { tasks: tasks (attributes: id, description, complete) }

  - Create task:
    - POST /api/v1/tasks
    - params: { task: { description: "description " }, format: "json" }
    - Authenticated Users sends an ['Authorization'] header to the server.
    - response: { task: task (attributes: id, description, complete) } or { message: "message" } if `error`

  - Register user:
    - POST /api/v1/users
    - params: { user: { email: "user@email.com", password: "password", password_confirmation: "password_confirmation, first_name: "first name", last_name: "last name" }, format: "json" }
    - response: { message: "message" , auth_token: token } or { message: "message" } if `error`

  - Flag task as completed:
    - POST /api/v1/tasks/:task_id/complete
    - Authenticated Users sends an ['Authorization'] header to the server.
    - response: { task: task (attributes: id, description, complete), meta: { message: "message" } }

  - Delete task:
    - DELETE /api/v1/tasks/:id
    - Authenticated Users sends an ['Authorization'] header to the server.
    - response: { task: task (attributes: id, description, complete), meta: { message: "message" } }

  - Logout user:
    - Done from the client by deleting the user token, server does not need to do anything. (You don't need to do anything here.)
