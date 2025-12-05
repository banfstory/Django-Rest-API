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
| Update users               | PUT    | /api/users/:pk                  | Yes (Bearer)   | "username", "email"                   | None                           |
| Get all followers          | GET    | /api/users/followers            | No             | None                                  | "user", "forum"                |
| Get followers by ID        | GET    | /api/users/:pk/followers     | No             | None                                  | None                           |
| Create followers           | POST   | /api/users/followers            | Yes (Bearer)   | "forum"                               | None                           |
| Delete followers           | DELETE | /api/users/:pk/followers/    | Yes (Bearer)   | None                                  | None                           |
| Get all forums             | GET    | /api/forums                     | No             | None                                  | "q", "name", "owner", "page"   |
| Get forum by ID            | GET    | /api/forums/:pk                 | No             | None                                  | None                           |
| Create forum               | POST   | /api/forums                     | Yes (Bearer)   | "name", "about"                       | None                           |
| Update forum               | PUT    | /api/forums/:pk                 | Yes (Bearer)   | "name", "about"                       | None                           |
| Delete forum               | DELETE | /api/forums/:pk                 | Yes (Bearer)   | None                                  | None                           |
| Get all posts              | GET    | /api/posts                      | No             | None                                  | "forum", "user"                |
| Get posts by ID            | GET    | /api/posts/:pk                  | No             | None                                  | None                           |
| Create posts               | POST   | /api/posts                      | Yes (Bearer)   | "title", "content", "forum"           | None                           |
| Update posts               | PUT    | /api/posts/:pk                  | Yes (Bearer)   | "title", "content"                    | None                           |
| Delete posts               | DELETE | /api/posts/:pk                  | Yes (Bearer)   | None                                  | None                           |
| Get all comments           | GET    | /api/comments                   | No             | None                                  | "post", "user"                 |
| Get comments by ID         | GET    | /api/comments/:pk               | No             | None                                  | None                           |
| Create comments            | POST   | /api/comments                   | Yes (Bearer)   | "content", "post"                     | None                           |
| Update comments            | PUT    | /api/comments/:pk               | Yes (Bearer)   | "content"                             | None                           |
| Delete comments            | DELETE | /api/comments/:pk               | Yes (Bearer)   | None                                  | None                           |
| Get all replies            | GET    | /api/replys                     | No             | None                                  | "comment", "user"              |
| Get replies by ID          | GET    | /api/replys/:pk                 | No             | None                                  | None                           |
| Create replies             | POST   | /api/replys                     | Yes (Bearer)   | "content", "comment"                  | None                           |
| Update replies             | PUT    | /api/replys/:pk                 | Yes (Bearer)   | "content"                             | None                           |
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

DATA: content <br/>

ENDPOINT: http:<span></span>//127.0.0.1:8000/api/replys/<int:pk>/ <br/>
METHOD: DELETE <br/>
DESCRIPTION: Delete user's reply <br/>
HEADER: Authorization: Bearer *Your access token* <br/>

