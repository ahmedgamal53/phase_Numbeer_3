# SimpleDelicacy — Django Recipe Finder

A fully server-rendered Django application converted from a vanilla JS / localStorage project.
No APIs, no localStorage, no frontend frameworks — pure Django + DTL.

---

## Quick Start

### 1. Prerequisites
- Python 3.10+
- pip

### 2. Create & activate a virtual environment
```bash
python -m venv venv

# Windows
venv\Scripts\activate

# macOS / Linux
source venv/bin/activate
```

### 3. Install dependencies
```bash
pip install -r requirements.txt
```

### 4. Apply database migrations
```bash
python manage.py makemigrations recipes
python manage.py migrate
```

### 5. (Optional) Create a Django superuser for /admin/ panel
```bash
python manage.py createsuperuser
```

### 6. Run the development server
```bash
python manage.py runserver
```

### 7. Open your browser
| URL | Page |
|-----|------|
| http://127.0.0.1:8000/ | Sign In |
| http://127.0.0.1:8000/signup/ | Sign Up |
| http://127.0.0.1:8000/home/ | Home (after login) |
| http://127.0.0.1:8000/recipes/ | All Recipes |
| http://127.0.0.1:8000/favorites/ | My Favorites |
| http://127.0.0.1:8000/dashboard/ | Admin Dashboard |
| http://127.0.0.1:8000/admin/ | Django Admin Panel |

---

## Project Structure
```
recipe_project/
├── manage.py
├── requirements.txt
├── db.sqlite3             ← auto-created on migrate
├── media/                 ← uploaded recipe images
├── static/
│   ├── css/               ← all stylesheets
│   └── js/
│       └── dashboard.js   ← UI-only JS (image preview + ingredient rows)
├── templates/
│   ├── base.html          ← navbar + flash messages (all pages extend this)
│   ├── home.html
│   ├── auth/
│   │   ├── signin.html
│   │   └── signup.html
│   ├── recipes/
│   │   ├── list.html
│   │   ├── detail.html
│   │   └── favorites.html
│   └── dashboard/
│       ├── index.html
│       └── recipe_form.html
├── recipe_project/
│   ├── settings.py
│   └── urls.py
└── recipes/
    ├── models.py       ← CustomUser, Recipe, Ingredient, Favorite
    ├── views.py        ← all page views (render/redirect only, no APIs)
    ├── forms.py        ← SignUpForm, SignInForm, RecipeForm
    ├── urls.py
    ├── decorators.py   ← @admin_required
    └── admin.py
```

---

## Architecture — Key Decisions

| Topic | Approach |
|-------|----------|
| Auth | `django.contrib.auth` — sessions, `@login_required` |
| Role guard | `@admin_required` decorator in `decorators.py` |
| Database | SQLite (dev) — swap to Postgres by editing `DATABASES` in settings |
| Images | `ImageField` → stored in `media/recipes/` |
| Favorites | `Favorite` model (user + recipe FK), toggled via POST form |
| Search | Django ORM `Q` objects — GET query params, no JS |
| No localStorage | 100% replaced by Django ORM + sessions |
| No APIs / AJAX | Every action is a standard HTML form POST or GET |
| JS scope | Only `dashboard.js` — image preview UI + ingredient DOM rows |

---
