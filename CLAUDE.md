# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**resthooks** is the website behind [resthooks.org](http://resthooks.org) — a documentation/marketing site explaining the **REST Hooks** pattern (a lightweight subscription/webhook layer on top of a REST API). Originally a Zapier initiative (2013–2017). It is a small **Flask** app that renders Markdown "flatpages" and static content.

This is a **legacy Python 2.7** application. Treat it as a static-content Flask site: the code is a thin renderer around Markdown posts in `posts/`.

## Tech Stack

- **Language**: Python 2.7 (see `runtime.txt`)
- **Framework**: Flask + Flask-FlatPages (Markdown-backed pages)
- **Markdown/formatting**: misaka, houdini, Pygments (syntax highlighting)
- **Process model**: Foreman / `Procfile` (`web` process)
- **Config**: `config.py` (loaded via `app.config.from_pyfile`)

## Repository Structure

```
resthooks.py         # the Flask app — routes, FlatPages setup, Markdown/Pygments rendering
config.py            # Flask config (STATIC_URL, POSTS_PER_PAGE, FLATPAGES_EXTENSION, DEBUG, ...)
templates/           # Jinja templates: index.html, post.html, 404.html
posts/               # Markdown content rendered as flatpages
├── code.md, docs.md
├── docs/            # alternatives, performance, retries, security
└── profiles/        # "how <company> uses REST Hooks" case studies
static/              # css/, js/ (jquery, underscore), img/, favicon
Procfile             # web: process definition for Foreman/Heroku
requirements.txt     # pinned Python 2.7 dependencies
runtime.txt          # Python runtime pin
```

## Commands

```bash
git clone <repo> && cd resthooks

# Python 2.7 environment (legacy)
mkvirtualenv resthooks -p python2.7      # or any Python 2.7 virtualenv
pip install -r requirements.txt

# Run
foreman run web        # via Procfile
# or
python resthooks.py    # direct
```

## Working On This Repo

- **Content changes = Markdown edits in `posts/`.** New pages are new `.md` files (they become flatpages automatically via Flask-FlatPages); follow the front-matter/format of an existing post in the same folder.
- **Case studies** live under `posts/profiles/` (`how-<company>-uses-rest-hooks.md`) — mirror that naming.
- Presentation changes go in `templates/` (Jinja) and `static/`.
- This is Python 2.7 — keep code Py2-compatible unless you are deliberately porting the whole app; don't introduce Py3-only syntax piecemeal.
- Routing uses a custom `RegexConverter` (registered in `resthooks.py`) — reuse it for regex-matched routes.
