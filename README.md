# Knowledge Base

A personal technical knowledge base built with [MkDocs](https://www.mkdocs.org/)
and the [Material theme](https://squidfunk.github.io/mkdocs-material/),
deployed as a GitHub Page. Organized by topic, not by course — each folder
under `docs/` is a subject you keep adding to over time.

## Local setup

```bash
python3 -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt

mkdocs serve                     # preview at http://127.0.0.1:8000, live-reloads on save
```

## Deploy to GitHub Pages

```bash
mkdocs gh-deploy --force
```

This builds the site and pushes it to a `gh-pages` branch. In your repo:
**Settings → Pages → Source: Deploy from a branch → Branch: `gh-pages`.**
Re-run `mkdocs gh-deploy` any time you want to publish new notes.

Alternatively, use the included GitHub Actions workflow
(`.github/workflows/deploy.yml`) to auto-publish on every push to `main` —
no local build needed after the first push.

## Adding a new topic (new top-level folder, e.g. `kubernetes/`)

1. `mkdir docs/kubernetes && touch docs/kubernetes/index.md`
2. Add pages as more `.md` files in that folder.
3. Register them in `mkdocs.yml` under `nav:` — this is what controls the
   sidebar order and grouping; a file not listed in `nav` still builds but
   won't appear in the sidebar.

## Adding a new page inside an existing topic

1. Create the `.md` file inside the topic folder (e.g. `docs/java/streams.md`).
2. Add one line to that topic's block in `mkdocs.yml` → `nav:`.
3. Link to it from the topic's `index.md` so it's reachable by click, not
   just from the sidebar.

## Project structure

```
knowledge-base/
├── docs/
│   ├── index.md                 ← homepage
│   ├── software-architecture/
│   ├── java/
│   ├── spring/
│   ├── database/
│   ├── docker/
│   └── git/
├── mkdocs.yml                   ← nav, theme, plugins — the site's control panel
├── requirements.txt
└── README.md
```

## Writing tips for this setup

- Admonitions for callouts: `!!! note`, `!!! warning`, `!!! tip` (see any
  existing page for examples).
- Diagrams: fenced ` ```mermaid ` blocks render as real diagrams, no image
  export needed — see `software-architecture/microservices.md` for one.
- Code blocks with a language tag get syntax highlighting and a copy button
  automatically.
