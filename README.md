# Django-REST-API

A **standalone Django REST API** for a forum application.  

The API handles all server-side operations — **user authentication**, **forums**, **posts**, **comments** and **replies** — and communicates via **HTTP requests**. It can be consumed by any client (frontend, mobile app, or other services).

---

## 🚀 Overview

- RESTful API built with **Django**  
- CRUD endpoints for **users**, **forums**, **posts**, **comments** and **replies**
- Runs inside a **virtual environment** for dependency isolation
- Requires **Python 3.10.0** for full compatibility with dependencies
- JWT-based authentication (access & refresh tokens)  
- Modular route and schema structure for maintainability  

---

## 🖥️ Running the Django API (Windows Instructions)

Follow these steps to set up and run the Django API locally:

### 0. Verify Python version
Make sure you are using Python 3.10.0 (otherwise some libraries may not work if a different python version is used):
```bash
python --version
# Should output: Python 3.10.0
```

### 1. Create a virtual environment
Create a new virtual environment inside the project folder:
```bash
python -m venv venv
```
This creates a folder named `venv` that keeps all project dependencies isolated.

### 2. Activate the virtual environment
```bash
venv\Scripts\activate
```
When activated, your terminal prompt will show `(venv)`.

### 3. Install required libraries
Install all dependencies listed in requirements.txt:
```
pip install -r requirements.txt
```
This ensures Django and JWT, and other required libraries are installed.

### 4. Navigate to the Django app directory
```
cd src
```

### 5. Run the Django server
```
python manage.py runserver 127.0.0.1:8000
```
The local server will start at (running at port 8000):
👉 http://127.0.0.1:8000

Note: You can choose any address or port number you want ('python manage.py runserver <ADDRESS>:<PORT>')

**Examples:**
- Getting users: http://127.0.0.1:8000/api/users
- Getting posts: http://127.0.0.1:8000/api/posts
- Getting comments: http://127.0.0.1:8000/api/comments

---

## 📝 Concepts
- Forum: A category or section where related discussions take place.
- Post: A message or topic created within a forum.
- Comment: A response to a post.
- Reply: A nested response to a comment, allowing threaded discussions.

---

## 🛠️ Endpoints Reference
| Description                | Method | Endpoint                        | Authenticated? | Payload                               | Query Parameter                |
|----------------------------|--------|---------------------------------|----------------|---------------------------------------|--------------------------------|
| Get all users              | GET    | /api/users                      | No             | None                                  | "q", "username", "page"        |
| Get users by ID            | GET    | /api/users/:pk                  | No             | None                                  | None                           |
| Create users               | POST   | /api/users                      | No             | "username", "email", "password"       | None                           |
| Update users               | PUT    | /api/users/:pk                  | Yes (Bearer)   | "username", "email", "password"       | None                           |
| Delete users               | DELETE | /api/users/:pk                  | Yes (Bearer)   | None                                  | None                           |
| Get all followers          | GET    | /api/users/followers            | No             | None                                  | "user", "forum"                |
| Get followers by ID        | GET    | /api/users/int:pk/followers     | No             | None                                  | None                           |
| Create followers           | POST   | /api/users/followers            | Yes (Bearer)   | "forum"                               | None                           |
| Delete followers           | DELETE | /api/users/int:pk/followers/    | Yes (Bearer)   | None                                  | None                           |
| Get all forums             | GET    | /api/forums                     | No             | None                                  | "q", "name", "owner", "page"   |
| Get forum by ID            | GET    | /api/forums/:pk                 | No             | None                                  | None                           |
| Create forum               | POST   | /api/forums                     | Yes (Bearer)   | "name", "about"                       | None                           |
| Update forum               | PUT    | /api/forums/:pk                 | Yes (Bearer)   | "name", "about"                       | None                           |
| Delete forum               | DELETE | /api/forums/:pk                 | Yes (Bearer)   | None                                  | None                           |
| Get all posts              | GET    | /api/posts                      | No             | None                                  | "forum", "user"                |
| Get posts by ID            | GET    | /api/posts/:pk                  | No             | None                                  | None                           |
| Create posts               | POST   | /api/posts                      | Yes (Bearer)   | "title", "content", "forumId"         | None                           |
| Update posts               | PUT    | /api/posts/:pk                  | Yes (Bearer)   | "title", "content", "forumId"         | None                           |
| Delete posts               | DELETE | /api/posts/:pk                  | Yes (Bearer)   | None                                  | None                           |
| Get all comments           | GET    | /api/comments                   | No             | None                                  | "post", "user"                 |
| Get comments by ID         | GET    | /api/comments/:pk               | No             | None                                  | None                           |
| Create comments            | POST   | /api/comments                   | Yes (Bearer)   | "content", "postId"                   | None                           |
| Update comments            | PUT    | /api/comments/:pk               | Yes (Bearer)   | "content", "postId"                   | None                           |
| Delete comments            | DELETE | /api/comments/:pk               | Yes (Bearer)   | None                                  | None                           |
| Get all replies            | GET    | /api/replys                     | No             | None                                  | "comment", "user"              |
| Get replies by ID          | GET    | /api/replys/:pk                 | No             | None                                  | None                           |
| Create replies             | POST   | /api/replys                     | Yes (Bearer)   | "content", "commentId"                | None                           |
| Update replies             | PUT    | /api/replys/:pk                 | Yes (Bearer)   | "content", "commentId"                | None                           |
| Delete replies             | DELETE | /api/replys/:pk                 | Yes (Bearer)   | None                                  | None                           |
| Create New Access Token    | POST   | /api/token                      | No             | "username", "password"                | None                           |
| Create New Access Token    | POST   | /api/token/refresh              | No             | "refresh"                             | None                           |

---

## 🧪 Using the API
### 1. Authentication
Protected endpoints require a **Bearer token** in the `Authorization` header.
Example:
```http
POST /api/forums
Authorization: Bearer <ACCESS_TOKEN>
```
- Replace `<ACCESS_TOKEN>` with the token received from `/api/token` POST request (access token).
- The `refresh` token can be used to get a new access token from `/api/refresh` POST request (in the payload, "token" represents the "refresh" token).
- Endpoints marked **Yes (Bearer)** in the table require this header.

### 2. Payload (Request Body)
For endpoints that accept data (POST/PATCH), send a **JSON body** with the fields listed in the table.
Example: Create a forum
```http
POST /api/forums
Authorization: Bearer <ACCESS_TOKEN>
Content-Type: application/json
```
Request Body:
```json
{
  "name": "Programming",
  "about": "All programming related discussions"
}
```
- Only include the fields required by the endpoint (see **Payload** column).

### 3. Query Parameters
Some GET endpoints support **query parameters** for filtering.
Example: Get a user who's username is admin
```http
GET /api/users?username=admin
```
- Fields listed under **Query Parameter** in the table can be included in the URL.
- Multiple query parameters can be combined with `&`, e.g., `/api/posts?user=123&forum=456`.

---

## 📜 License
This project is licensed under the [MIT License]([https://github.com/banfstory/Forum-Express-API?tab=MIT-1-ov-file](https://github.com/banfstory/Django-Rest-API/blob/main/LICENSE)).

---

------------------------------------------------------------------------------------------------------------------

To run the virtual environment do the following (instructions for windows OS only), start with going into command prompt:

1. Go to root directory of this folder
2. Enter 'python -m venv venv'
3. Enter 'venv\Scripts\activate' to activate the virtual
4. Enter 'pip install -r requirements.txt'
5. Go to src folder 
6. Enter 'python manage.py runserver 127.0.0.1:8000' to run the local server or choose whatever address or port number you want

RESTFUL API AUTHENTICATION
In order to access certain resources or perform certain actions, the user needs to authenticate themselves by providing their username and password where they will be given an access token and a refesh token. The access token provides authorization for the user to access to resources and perform actions that are otherwised restricted if no access token was provided. The refresh token will be required to refresh the access token that has expired (access token expires in 7 days but can be changed in the settings.py file located in the django_api folder). These tokens are represented as JSON web tokens (JWT) and these tokens are separated by period in between representing encoded header data and payload data with the signature to ensures that no one can create their own tokens and pretend to be someone else.

To authenticate a user and get the tokens use http:<span></span>//127.0.0.1:8000/api/token/ as a POST method with the header as 'Content-Type: application/json' and data as '{"username":"your username","password":"your password"}', for example in curl you can use 'curl -X POST -H 'Content-Type: application/json' -d '{"username":"admin","password":"pass"}' http:<span></span>//127.0.0.1:8000/api/token/' to authenticate. 

To refresh an expired token, you can use http:<span></span>//127.0.0.1:8000/api/token/refresh/ as a POST method with the header 'Content-Type: application/json' and data as '{"refresh":"your refresh token"}', for example in curl you can use 'curl -X POST -H 'Content-Type: application/json' -d '{"refresh":"your refresh token"}' http:<span></span>//127.0.0.1:8000/api/token/refresh/'

Certain endpoints are only accessible if you provide the access token. To provide an access token, provide the header with "Authorization: Bearer *Your access token*", for example in curl you can use 'curl -X PUT -H 'Content-Type: application/json' -H "Authorization: Bearer *Your access token*" -d '{"username":"admin", "email":"admin@gmail.<span></span>com"}' http:<span></span>//127.0.0.1:8000/api/users/1/'

NOTE: You can authenticate as admin with the username 'admin' and password 'pass'

<h2>RESTFUL API ENDPOINTS</h2>

The API endpoints are broken down into catrgories, these are USERS, FORUMS, POSTS, COMMENTS, REPLYS

Endpoints that return multiple results will only show 10 results per page. To navigate through each page, add the query param 'page' with the page number into URL endpoint </br>

When sending files with data, both the files and data must be sent as form data with header Content-Type: multipart/form-data instead of Content-Type: application/json, for example in curl you can use 'curl -X PUT -H 'Content-Type: multipart/form-data' -H "Authorization: Bearer *Your access token*" -F 'image=@*your file path*' -F 'username=admin' -F 'email=admin@gmail.<span></span>com' http:<span></span>//127.0.0.1:8000/api/users/1/'
 
<h3>USERS ENDPOINTS</h3>

ENDPOINT: http:<span></span>//127.0.0.1:8000/api/users/ <br/>
METHOD: GET <br/>
DESCRIPTION: Get details of all users accounts <br/>
QUERY PARAMS: q, username, page <br/>

ENDPOINT: http:<span></span>//127.0.0.1:8000/api/users/ <br/>
METHOD: POST <br/>
DESCRIPTION: Register a user account <br/>
HEADER: Content-Type: application/json <br/>
DATA: username, email, password <br/>

ENDPOINT: http:<span></span>//127.0.0.1:8000/api/users/<int:pk>/ <br/>
METHOD: GET <br/>
DESCRIPTION: Get a user account <br/>

ENDPOINT: http:<span></span>//127.0.0.1:8000/api/users/<int:pk>/ <br/>
METHOD: PUT <br/>
DESCRIPTION: Update user account details <br/>
HEADER: Content-Type: application/json (without image file), 'Content-Type: multipart/form-data' (with image file), Authorization: Bearer *Your access token* <br/>
FILE: image <br/>
DATA: username, email <br/>

ENDPOINT: http:<span></span>//127.0.0.1:8000/api/users/followers/ <br/>
METHOD: GET <br/>
DESCRIPTION: Get followers list of which users are following which forums <br/>
PARAM: user, forum <br/>

ENDPOINT: http:<span></span>//127.0.0.1:8000/api/users/followers/ <br/>
METHOD: POST <br/>
DESCRIPTION: User follows a forum <br/>
HEADER: Content-Type: application/json, Authorization: Bearer *Your access token* <br/>
DATA: forum <br/>

ENDPOINT: http:<span></span>//127.0.0.1:8000/api/users/<int:pk>/followers/ <br/>
METHOD: GET <br/>
DESCRIPTION: Get a instance of a user who is following a forum <br/>
DATA: forum <br/>

ENDPOINT: http:<span></span>//127.0.0.1:8000/api/users/<int:pk>/followers/ <br/>
METHOD: DELETE <br/>
DESCRIPTION: User unfollows a forum <br/>

ENDPOINT: http:<span></span>//127.0.0.1:8000/api/users/default-image/ <br/>
METHOD: PUT <br/>
DESCRIPTION: Set user image to default image <br/>
HEADER: Authorization: Bearer *Your access token* <br/>

<h3>FORUMS ENDPOINTS</h3>

ENDPOINT: http:<span></span>//127.0.0.1:8000/api/forums/ <br/>
METHOD: GET <br/>
DESCRIPTION: Get details of all forums <br/>
QUERY PARAMS: q, name, owner, page <br/>

ENDPOINT: http:<span></span>//127.0.0.1:8000/api/forums/ <br/>
METHOD: POST <br/>
DESCRIPTION: Create forum <br/>
HEADER: Content-Type: application/json <br/>
DATA: name, about <br/>

ENDPOINT: http:<span></span>//127.0.0.1:8000/api/forums/<int:pk>/ <br/>
METHOD: GET <br/>
DESCRIPTION: Get a user account <br/>

ENDPOINT: http:<span></span>//127.0.0.1:8000/api/forums/<int:pk>/ <br/>
METHOD: PUT <br/>
DESCRIPTION: Update user's forum details <br/>
HEADER: Content-Type: application/json (without image file), 'Content-Type: multipart/form-data' (with image file), Authorization: Bearer *Your access token* <br/>
FILE: image <br/>
DATA: name, about <br/>

ENDPOINT: http:<span></span>//127.0.0.1:8000/api/forums/<int:pk>/ <br/>
METHOD: DELETE <br/>
DESCRIPTION: Delete forum <br/>
HEADER: Authorization: Bearer *Your access token* <br/>

ENDPOINT: http:<span></span>//127.0.0.1:8000/api/forums/<int:pk>/default-image/ <br/>
METHOD: PUT <br/>
DESCRIPTION: Set forum image to default image <br/>
HEADER: Authorization: Bearer *Your access token* <br/>

<h3>POSTS ENDPOINT</h3>

ENDPOINT: http:<span></span>//127.0.0.1:8000/api/posts/ <br/>
METHOD: GET <br/>
DESCRIPTION: Get details of all posts <br/>
QUERY PARAMS: forum, user <br/>

ENDPOINT: http:<span></span>//127.0.0.1:8000/api/posts/ <br/>
METHOD: POST <br/>
DESCRIPTION: Create post for the forum <br/>
HEADER: Content-Type: application/json, Authorization: Bearer *Your access token* <br/>
DATA: title, content, forum <br/>

ENDPOINT: http:<span></span>//127.0.0.1:8000/api/posts/<int:pk>/ <br/>
METHOD: GET <br/>
DESCRIPTION: Get details of a post <br/>

ENDPOINT: http:<span></span>//127.0.0.1:8000/api/posts/<int:pk>/ <br/>
METHOD: PUT <br/>
DESCRIPTION: Update user's post details <br/>
HEADER: Content-Type: application/json, Authorization: Bearer *Your access token* <br/>
DATA: title, content <br/>

ENDPOINT: http:<span></span>//127.0.0.1:8000/api/posts/<int:pk>/ <br/>
METHOD: DELETE <br/>
DESCRIPTION: Delete user's post <br/>
HEADER: Authorization: Bearer *Your access token* <br/>

<h3>COMMENTS ENDPOINT</h3>

ENDPOINT: http:<span></span>//127.0.0.1:8000/api/comments/ <br/>
METHOD: GET <br/>
DESCRIPTION: Get details of all comments <br/>
QUERY PARAMS: post, user <br/>

ENDPOINT: http:<span></span>//127.0.0.1:8000/api/comments/ <br/>
METHOD: POST <br/>
DESCRIPTION: Create comment for the post <br/>
HEADER: Content-Type: application/json, Authorization: Bearer *Your access token* <br/>
DATA: content, post <br/>

ENDPOINT: http:<span></span>//127.0.0.1:8000/api/comments/<int:pk>/ <br/>
METHOD: GET <br/>
DESCRIPTION: Get details of a comment <br/>

ENDPOINT: http:<span></span>//127.0.0.1:8000/api/comments/<int:pk>/ <br/>
METHOD: PUT <br/>
DESCRIPTION: Update user's comment details <br/>
HEADER: Content-Type: application/json, Authorization: Bearer *Your access token* <br/>
DATA: content <br/>

ENDPOINT: http:<span></span>//127.0.0.1:8000/api/comments/<int:pk>/ <br/>
METHOD: DELETE <br/>
DESCRIPTION: Delete user's comment <br/>
HEADER: Authorization: Bearer *Your access token* <br/>

<h3>REPLYS ENDPOINT</h3>

ENDPOINT: http:<span></span>//127.0.0.1:8000/api/replys/ <br/>
METHOD: GET <br/>
DESCRIPTION: Get details of all replys <br/>
QUERY PARAMS: comment, user <br/>

ENDPOINT: http:<span></span>//127.0.0.1:8000/api/replys/ <br/>
METHOD: POST <br/>
DESCRIPTION: Create reply for the comment <br/>
HEADER: Content-Type: application/json, Authorization: Bearer *Your access token* <br/>
DATA: content, comment <br/>

ENDPOINT: http:<span></span>//127.0.0.1:8000/api/replys/<int:pk>/ <br/>
METHOD: GET <br/>
DESCRIPTION: Get details of a reply <br/>

ENDPOINT: http:<span></span>//127.0.0.1:8000/api/comments/<int:pk>/ <br/>
METHOD: PUT <br/>
DESCRIPTION: Update user's reply details <br/>
HEADER: Content-Type: application/json, Authorization: Bearer *Your access token* <br/>
DATA: content <br/>

ENDPOINT: http:<span></span>//127.0.0.1:8000/api/replys/<int:pk>/ <br/>
METHOD: DELETE <br/>
DESCRIPTION: Delete user's reply <br/>
HEADER: Authorization: Bearer *Your access token* <br/>

