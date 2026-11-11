# FastAPI Pub

A FastAPI-powered blog and user platform with authentication, profile management, password reset flows, and HTML-based post browsing.

## Overview

FastAPI Pub is a full-stack style FastAPI application that combines:

- REST API endpoints for users and posts
- JWT-based authentication
- Password reset via email workflow
- User profile image upload support
- Jinja templates for browser-based pages and post listings
- SQLAlchemy async ORM with PostgreSQL-ready configuration

This project is organized as a small blog application with API routes under `/api/...` and HTML pages such as home, login, register, and account views.

## Features

- User registration and login
- JWT token authentication via OAuth2 password flow
- Profile updates and password changes
- Password reset request and reset confirmation
- Blog post creation, listing, update, and deletion
- Pagination for posts
- Profile picture upload and deletion
- Static asset serving for CSS and images
- Health check endpoint for service monitoring

## Tech Stack

- Python 3.12+
- FastAPI
- SQLAlchemy 2.0 + async engine
- Pydantic + Pydantic Settings
- Jinja2 templates
- PostgreSQL-ready environment configuration
- JWT via PyJWT
- Argon2 password hashing via `pwdlib`
- AWS S3-compatible profile image storage

## Project Structure

```text
.
├── alembic/                 # Database migration files
├── routers/                 # API routers for users and posts
├── static/                  # Static assets (CSS, JS, images)
├── templates/               # HTML templates for frontend pages
├── tests/                   # Test suite (if present in working branch)
├── auth.py                  # JWT and password utilities
├── config.py                # Application settings from environment
├── database.py              # SQLAlchemy async DB configuration
├── email_utils.py           # Email sending utilities
├── image_utils.py           # Image processing and upload helpers
├── main.py                  # FastAPI app and route registration
├── models.py                # SQLAlchemy models
├── schemas.py               # Pydantic request/response schemas
├── alembic.ini              # Alembic configuration
├── pyproject.toml           # Project dependencies and tooling
├── .gitignore               # Git ignore rules
├── NOTES.md                 # Development notes from the project history
├── README.md                # Project documentation
└── .env.example             # Recommended local environment file (if created in your setup)
```

## Configuration

The app reads configuration from a `.env` file using `pydantic-settings`.

Example:

```env
DATABASE_URL=postgresql+asyncpg://username:password@localhost:5432/fastapi_pub
SECRET_KEY=your-super-secret-key
ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=30
POST_PER_PAGE=10
RESET_TOKEN_EXPIRE_MINUTES=60
MAX_UPLOAD_SIZE_BYTES=5242880
MAIL_SERVER=smtp.example.com
MAIL_PORT=587
MAIL_USERNAME=your-email
MAIL_PASSWORD=your-password
MAIL_FROM="FastAPI Blog <info.fastapi@resend.dev>"
FRONTEND_URL=http://localhost:8000
S3_BUCKET_NAME=your-bucket
S3_BUCKET_REGION=us-east-1
S3_ACCESS_KEY_ID=your-access-key
S3_SECRET_ACCESS_KEY=your-secret-key
S3_ENDPOINT_URL=https://s3.example.com
```

Note: the code uses environment variables that are case-insensitive. You should set the values according to your local or deployment environment.

## Installation

1. Clone the repository:

```bash
git clone https://github.com/GauravP9/FastAPI-Pub.git
cd FastAPI-Pub
```

2. Create a virtual environment:

```bash
python -m venv .venv
source .venv/bin/activate
```

3. Install dependencies:

```bash
python -m pip install --upgrade pip
python -m pip install -e .
```

If your environment does not support editable installs, install the dependencies declared in `pyproject.toml` manually with pip.

4. Create a `.env` file and add your environment variables.

## Run the Application

Start the development server:

```bash
uvicorn main:app --reload
```

Then open:

- http://localhost:8000
- API docs: http://localhost:8000/docs
- ReDoc: http://localhost:8000/redoc

## Main Routes

### Web/UI routes

- `GET /` — homepage with recent posts
- `GET /posts` — posts page
- `GET /posts/{post_id}` — individual post page
- `GET /login` — login page
- `GET /register` — registration page
- `GET /account` — account page
- `GET /forgot-password` — password reset request page
- `GET /reset-password` — reset password page

### API routes

#### Users

- `POST /api/users` — create a user
- `POST /api/users/token` — login and obtain JWT token
- `GET /api/users/me` — current authenticated user
- `PATCH /api/users/me/password` — change current password
- `POST /api/users/forgot-password` — request password reset email
- `POST /api/users/reset-password` — complete reset flow
- `GET /api/users/{user_id}` — view user by id
- `GET /api/users/{user_id}/posts` — get posts by user
- `PATCH /api/users/{user_id}` — update user profile
- `DELETE /api/users/{user_id}` — delete user
- `PATCH /api/users/{user_id}/picture` — upload profile picture
- `DELETE /api/users/{user_id}/picture` — remove profile picture

#### Posts

- `GET /api/posts` — list posts with pagination
- `POST /api/posts` — create a post (authenticated)
- `GET /api/posts/{post_id}` — get one post
- `PUT /api/posts/{post_id}` — update post fully
- `PATCH /api/posts/{post_id}` — partial update
- `DELETE /api/posts/{post_id}` — delete post

## Health Check

The app exposes a simple health endpoint:

```bash
curl http://localhost:8000/health
```

Expected result:

```json
{"status":"healthy"}
```

## Notes

This repository contains both API and template-based frontend functionality. It appears to be designed as a learning project or starter blog app, with strong emphasis on:

- async SQLAlchemy patterns
- FastAPI dependency injection
- JWT authentication flows
- protected routes
- AWS S3 image storage integration
- password-reset security practices

## License

This project does not appear to include a license file in the repository root. Please confirm the licensing requirements before publishing or reusing it publicly.

## Contributing

Contributions are welcome. For larger changes, it is recommended to open an issue or discuss the planned update before making a wide-reaching modification.

## Security Reminder

- Never commit real secrets to the repository
- Keep `.env` values local and secure
- Rotate tokens and service credentials regularly
- Verify image and upload handling before production deployment

