# Inkwell API — Flask Backend

A REST API for the Inkwell blog platform, built with **Flask**, **SQLAlchemy**, and **JWT authentication**. Pairs with the [Inkwell front-end prototype](../).

## Tech Stack

- Python 3.10+
- Flask 3
- Flask-SQLAlchemy (SQLite for dev, swap to PostgreSQL for production)
- Flask-JWT-Extended (access + refresh tokens)
- Flask-CORS

## Features

- **Auth** — signup, login, refresh token, `/me`
- **Users** — view profile, edit own profile (name, bio, avatar, social links), follow/unfollow
- **Blogs** — create, edit, delete, publish/draft; search, filter by category/tag, sort (recent/popular/likes), pagination
- **Comments** — nested replies
- **Likes & bookmarks** — per-user, per-post
- **Categories & tags** — with live post counts
- **Dashboard** — aggregated stats for the logged-in writer (views, likes, comments, followers, drafts/published/scheduled counts)
- **Notifications** — simple in-app notification feed

## Project Structure

```
inkwell-backend/
├── app/
│   ├── __init__.py         # App factory, blueprint registration
│   ├── extensions.py        # db, jwt, cors singletons
│   ├── models.py             # User, Profile, Blog, Category, Tag, Comment, Like, Bookmark, Follower, Notification
│   └── routes/
│       ├── auth.py            # /api/auth/*
│       ├── users.py           # /api/users/*
│       ├── blogs.py           # /api/blogs/*
│       ├── categories.py      # /api/categories
│       ├── tags.py            # /api/tags
│       ├── comments.py        # /api/blogs/<id>/comments
│       ├── interactions.py    # likes, bookmarks, notifications
│       └── dashboard.py       # /api/dashboard/stats
├── instance/                  # SQLite DB lives here (git-ignored)
├── config.py                   # Config from environment variables
├── seed.py                      # Populates sample data
├── run.py                        # Dev entrypoint
├── requirements.txt
├── .env.example
└── README.md
```

## Setup

```bash
cd inkwell-backend

# 1. Create a virtual environment
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

# 2. Install dependencies
pip install -r requirements.txt

# 3. Configure environment variables
cp .env.example .env            # then edit SECRET_KEY / JWT_SECRET_KEY

# 4. Create the instance folder (holds the SQLite file)
mkdir -p instance

# 5. Seed the database with sample data
python seed.py

# 6. Run the dev server
python run.py
```

The API is now live at `http://localhost:5000`. Health check: `GET /api/health`.

**Sample login (from `seed.py`):** `lavanya@example.com` / `password123`

## API Reference

All request/response bodies are JSON. Authenticated routes require an `Authorization: Bearer <access_token>` header.

### Auth
| Method | Endpoint | Auth | Description |
|---|---|---|---|
| POST | `/api/auth/signup` | — | Create account → `{name, email, password}` |
| POST | `/api/auth/login` | — | Log in → `{email, password}` |
| POST | `/api/auth/refresh` | refresh token | Get a new access token |
| GET | `/api/auth/me` | ✅ | Current user's profile |
| POST | `/api/auth/logout` | ✅ | Logout (client discards token) |

### Users
| Method | Endpoint | Auth | Description |
|---|---|---|---|
| GET | `/api/users/<id>` | — | Public profile |
| PUT | `/api/users/me` | ✅ | Edit own name/bio/avatar/social links |
| GET | `/api/users/<id>/blogs` | — | A user's published posts |
| POST | `/api/users/<id>/follow` | ✅ | Follow a user |
| DELETE | `/api/users/<id>/follow` | ✅ | Unfollow |

### Blogs
| Method | Endpoint | Auth | Description |
|---|---|---|---|
| GET | `/api/blogs?search=&category=&tag=&sort=&page=&per_page=` | — | List/search/filter/paginate published posts |
| GET | `/api/blogs/<slug>` | — | Full post detail (increments view count) |
| POST | `/api/blogs` | ✅ | Create a post |
| PUT | `/api/blogs/<id>` | ✅ (author only) | Edit a post |
| DELETE | `/api/blogs/<id>` | ✅ (author only) | Delete a post |

### Comments / Likes / Bookmarks
| Method | Endpoint | Auth |
|---|---|---|
| GET / POST | `/api/blogs/<id>/comments` | GET: —, POST: ✅ |
| DELETE | `/api/comments/<id>` | ✅ (author only) |
| POST / DELETE | `/api/blogs/<id>/like` | ✅ |
| POST / DELETE | `/api/blogs/<id>/bookmark` | ✅ |
| GET | `/api/users/me/bookmarks` | ✅ |

### Categories, Tags, Dashboard, Notifications
| Method | Endpoint | Auth |
|---|---|---|
| GET | `/api/categories` | — |
| GET | `/api/tags?search=` | — |
| GET | `/api/dashboard/stats` | ✅ |
| GET | `/api/notifications` | ✅ |
| POST | `/api/notifications/<id>/read` | ✅ |

### Example: creating a post

```bash
curl -X POST http://localhost:5000/api/blogs \
  -H "Authorization: Bearer <your_access_token>" \
  -H "Content-Type: application/json" \
  -d '{
    "title": "My First Post",
    "content": "## Hello\n\nThis is my first Inkwell post.",
    "status": "published",
    "category": "Technology",
    "tags": ["python", "flask"]
  }'
```

## Switching to PostgreSQL

Set `DATABASE_URL` in `.env`:
```
DATABASE_URL=postgresql://user:password@localhost:5432/inkwell
```
Install the driver: `pip install psycopg2-binary`, then re-run `python seed.py` (or use a migration tool like Flask-Migrate for production schema changes instead of `db.drop_all()`/`db.create_all()`).

## Notes & Limitations

- JWTs are stateless — `logout` doesn't revoke a token server-side. For real revocation, add a token-blocklist table.
- `seed.py` drops and recreates all tables — don't run it against real data.
- Google OAuth and Cloudinary image uploads aren't implemented — both require external API credentials you'd register yourself (Google Cloud Console / Cloudinary dashboard) and wire in via `authlib`/`flask-dance` and the `cloudinary` SDK respectively.
- CORS defaults to `*` for local development — restrict `CORS_ORIGINS` in `.env` before deploying.

## Connecting the Front End

The existing `inkwell-blog.html` prototype uses in-memory mock data. To wire it to this API, replace the `POSTS` array and mock functions with `fetch()` calls to these endpoints, storing the JWT (e.g. in memory or an httpOnly cookie set by the backend — avoid `localStorage` for tokens in production) and sending it as a Bearer token on authenticated requests.
