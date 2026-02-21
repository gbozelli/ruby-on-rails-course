# Rails Introduction

## What is Backend?

- Frontend focus on user-side of the app
- Backend focus on the app logic and data
- Interacts with databases (SQL)
- Acts to integrate server-side and client-side
- Multiple languages (Python, JavaScript, Ruby)

## What is a framework?

- Code libraries to help us in programming
- Frontend: React, Angular, Vue
- Backend: Django, Ruby on Rails, Express

## Setup Ruby on Rails

- Install Rails  
``` bash
gem install rails
```

- Install Yarn  
``` bash
npm install --global yarn
```

- Create a new Rails app  
``` bash
rails new my_first_rails_app
cd my_first_rails_app
```

- Generate scaffold  
``` bash
rails generate scaffold Car make:string model:string year:integer
```

- Run database migration  
``` bash
rails db:migrate
```

- Start the server  
``` bash
rails server
```

- Visit in your browser:

```
http://localhost:3000/cars
```

## PostgreSQL and Environment variables

- Variables acessed locally could contain sensitive informations
- We need to hide this informations
- Rails uses the environment to set up its own internal environment variable
- PostgreSQL auth via roles. Role is like a user that interacts with service

### PostgreSQL and Rails integration

## PostgreSQL and Rails Integration

- Install PostgreSQL  
``` bash
sudo apt install postgresql postgresql-contrib libpq-dev
```

- Create a PostgreSQL role  
``` bash
sudo -u postgres createuser -s bozelli
sudo -u postgres psql
\password bozelli
\q
```

- Set your database password as an environment variable  
``` bash
export DB_PASSWORD=your_password_here
```

- Configure `config/database.yml`  
``` yaml
default: &default
  adapter: postgresql
  encoding: unicode
  username: bozelli
  password: <%= ENV["DB_PASSWORD"] %>
  host: localhost
```

- Create the database  
``` bash
rails db:create
```

- Run migrations  
``` bash
rails db:migrate
```

## Web refresher

### HTTP

- Way of structuring the request-response between client and server
- URL (https://www.domain.com:1234/path/to/resource?a=b&x=y) have 4 components:
  - ``https`` is the protocol
  - ``www.domain.com`` is the host.
  - ``1234`` is the port
  - ``path/to/resource`` is the path
  - ``?a=b&x=y`` are the query
- Request:
  - Request line: this says what is being requested. It consists of a verb, a path, and the HTTP version
  - Headers: additional information about the message, the requester, and the communication format
- HTTP Request Verbs:
  - GET: fetch resources from the server. No request body required; data is usually sent via URL parameters.
  - POST: create a new resource on the server. Data is sent in the request body.
  - PUT: replace an existing resource. Typically sends the full updated representation.
  - PATCH: partially update an existing resource. Sends only modified fields.
  - DELETE: delete an existing resource from the server.

- HTTP Headers:
  - Cache-Control: defines caching policies (e.g., no-cache, max-age).
  - Connection: controls whether the TCP connection remains open (keep-alive, close).
  - Pragma: legacy HTTP/1.0 header used mainly for cache control.
  - Trailer: indicates headers that will be sent after the message body (used with chunked transfer).
  - Transfer-Encoding: specifies how the message body is encoded during transfer (e.g., chunked).
  - Via: indicates intermediate proxies or gateways the request passed through.
  - Upgrade: requests switching to another protocol (e.g., HTTP to WebSocket).

- HTTP Responses:
  - status line: includes HTTP version, status code, and status message (e.g., HTTP/1.1 200 OK).
  - headers: metadata about the response (content type, caching rules, etc.).
  - body: the actual returned data (HTML, JSON, XML, etc.).

- Status codes:
  - 1xx: informational responses (request received, continuing process).
  - 2xx: success (request successfully processed).
  - 3xx: redirection (further action required).
  - 4xx: client errors (problem with the request).
  - 5xx: server errors (server failed to fulfill a valid request).

### REST

- There are just 7  different types of things that you usually want to do to an individual resource via the web
- You can do them by mixing and matching the HTTP verbs we just covered
- A “resource” usually means a “thing” in your database
1. GET all the posts (aka “index” the posts)
2. GET just one specific post (aka “show” that post)
3. GET the page that lets you create a new post (aka view the “new” post page)
4. POST the data you just filled out for a new post back to the server so it can create that post (aka “create” the post)
5. GET the page that lets you edit an existing post (aka view the “edit” post page)
6. PUT (or PATCH) the data you just filled out for editing the post back to the server so it can actually perform the update (aka “update” the post)
7. DELETE one specific post by sending a delete request to the server (aka “destroy” the post)
- Rails is structured to follow these conventions

### MVC
- Model, View and Controller
- The functions of a web app can be broken into more parts
- Each part is a class
- When a request comes into your app:
  - The router figures out which controller to send it to
  - That controller asks the model for data
  - Then that controller passes off whatever data it needs to the views
  - Once the proper view has been pumped full of the data it needs, it gets sent back to the client

### APIs

- Interface for programmers make requests to another sites
- A person hit a site using browser. A program hit a site using APIs

### Cookies

- A way to website remember who you are between requests
- Cookies are little bits of data that your browser sends to the website every time you make a request to it

### Sessions

- A session stores data about a user between requests
- It allows the server to remember information across multiple interactions
- In Rails, session data is usually stored in cookies (encrypted by default)
- Sessions are temporary and expire after logout or timeout
- Commonly used to store user_id after login

### Authentication

- Authentication answers the question: who is the user?
- It verifies the identity of someone trying to access the system
- Usually implemented with email/username and password
- After successful login, the server stores the user’s id in the session
- Rails can implement authentication manually or using gems like Devise
- The server checks session data on each request to identify the current user

### Authorization

- Authorization answers the question: what is the user allowed to do?
- It defines permissions and access control
- Example: only logged-in users can create posts
- Example: only admins can delete other users
- Authorization logic runs after authentication
- In Rails, this is often implemented with controller filters or gems like Pundit or CanCanCan

## Routing 

- 