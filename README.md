# Bookshelf
A self-hosted, browser-based personal book library manager running on port 8080.

<img width="1311" height="532" alt="image" src="https://github.com/user-attachments/assets/d1d642aa-f29e-4387-9745-3daaab458a5a" />

## Stack

- **FastAPI** — backend and routing
- **Jinja2** — server-side HTML templates
- **SQLite** — local database (no external DB required)
- **Docker / Docker Compose** — containerised deployment

## Features

- Book list with full-text search (title and author)
- Filters: Reading State, Year Read, Location (and any additional dynamic filter you add)
- Add, edit, and delete books
- Import books from an Excel file (`.xlsx`)
- Stats strip at the top: total, read, not read, reading
- Export filtered list as CSV or Excel

## Book Fields

All fields are optional except `Title`.

| Field | Notes |
|---|---|
| Title | Required |
| Author | |
| Release Date | |
| Worth Reading | |
| Reading Status | Free text or dropdown — drives the Reading State pill |
| Year Read | |
| Location | Where the book is physically or digitally stored |
| ISBN | |
| Current Page | |
| Total Pages | |
| Rating | |
| Description | |
| Notes | |

## Reading State Logic

The app stores two separate fields:

- `status_raw` — the original text value (e.g. `"Read"`, `"Not Read"`, `"Reading"`)
- `reading_bucket` — the normalised value used for filtering and the coloured pill (`read`, `not_read`, `reading`, `other`)

`reading_bucket` is computed automatically from `status_raw` on every save. Recognised values:

| Type in Reading Status | Pill shown |
|---|---|
| `Read` | Read |
| `Reading` | Reading |
| `Not Read` / `To Read` | Not read |
| anything else | Other |

## Getting Started

```bash
docker compose up --build
```

Then open in your browser:

- http://localhost:8080
- or http://YOUR-SERVER-IP:8080 from another device on the LAN

## Importing from Excel

1. Place your `.xlsx` file in the `imports/` folder, or upload it directly via the UI.
2. Go to **Manage → Import Excel** in the app.
3. The importer maps the `Status` column from the spreadsheet to `status_raw` automatically.

**Duplicate handling:** a book is skipped if an entry with the same `title` + `author` pair already exists in the database.

## Adding a Dynamic Filter

To expose a new filter in the UI (e.g. Location, Rating):

1. **`main.py`** — add the parameter to `home()`, query distinct values, filter the queryset, and pass the list to the template.
2. **`index.html`** — add a `<select>` element in the filter toolbar using a `{% for %}` loop over the new list.
3. **`index.html`** — add the new parameter to the sort column links (`&param={{ param|urlencode }}`).

## Stopping the App

```bash
docker compose down
```

To also remove the database volume:

```bash
docker compose down -v
```
