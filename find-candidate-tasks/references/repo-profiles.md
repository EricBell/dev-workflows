# Repo Profiles for LiveCompare Task Discovery

This file contains profiles of the three approved repositories for LiveCompare tasks, including good task areas, setup commands, and what to avoid.

---

## NearBeach
**Language**: Python (Django) + JavaScript (Vue)
**Git URL**: `https://github.com/nearbeach/NearBeach`
**Zip Download**: Available via project-rules.md link

### Good Task Areas
- Task and project views, lists, filtering and sorting
- Forms and validation
- Permissions
- Notes and attachments
- Search
- Django test suite

### Setup Commands (Sandbox)
```bash
cd /workspace/nearbeach
python3 -m pip install --user --break-system-packages -r requirements.txt
python3 manage.py migrate
DJANGO_SUPERUSER_USERNAME=admin DJANGO_SUPERUSER_EMAIL=admin@example.com DJANGO_SUPERUSER_PASSWORD=password python3 manage.py createsuperuser --noinput
python3 manage.py runserver 0.0.0.0:8000
```
Then open `http://localhost:8000` and log in as `admin` / `password`.

### What to Avoid
- Live collaboration features
- Complex deployment/infrastructure tasks
- Anything requiring external services not in the repo

---

## erd-editor
**Language**: JavaScript
**Git URL**: `https://github.com/dineug/erd-editor`
**Zip Download**: Available via project-rules.md link

### Good Task Areas
- Creating and editing tables, columns and relationships
- Canvas and selection
- Light and dark themes
- Import and export
- Undo/redo
- Keyboard shortcuts
- Speed on large diagrams
- Unit and browser tests

### Setup Commands (Sandbox)
```bash
cd /workspace/erd-editor
echo "manage-package-manager-versions=false" >> ~/.npmrc
pnpm install --frozen-lockfile
cd packages/app && pnpm dev
```
Then open `http://localhost:5173`.

To run browser tests:
```bash
pnpm --filter @dineug/erd-editor exec playwright install chromium
```

### What to Avoid
- Live collaboration
- Server-dependent features

---

## Excalidraw
**Language**: TypeScript (React)
**Git URL**: `https://github.com/excalidraw/excalidraw`
**Zip Download**: Available via project-rules.md link

### Good Task Areas
- Drawing tools
- Arrows and how they attach to shapes
- Text
- Resizing and rotating
- Undo/redo
- Export to PNG and SVG
- Opening older files
- Keyboard shortcuts
- Accessibility
- Speed on large drawings
- ~2,600 unit tests

### Setup Commands (Sandbox)
```bash
cd /workspace/excalidraw
npx --yes yarn@1.22.22 install
BROWSER=none npx --yes yarn@1.22.22 start
```
Then open `http://localhost:3001`. First run takes a few minutes.

To run unit tests:
```bash
cd /workspace/excalidraw && npx --yes yarn@1.22.22 test:app --watch=false
```

### What to Avoid
- Live collaboration
- Share links
- AI features
- Library publishing
- These depend on Excalidraw's servers, which aren't in the repo

---

## Using These Profiles

When the user specifies a git URL, match it to the appropriate profile above. Use the good task areas as starting points for exploration, and the setup commands for the "How to judge completion" section of the Task Brief.