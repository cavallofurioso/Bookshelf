# Bookshelf App
A server-side app for managing a personal book index, accessible via browser on port 8080.


<img width="1335" height="475" alt="image" src="https://github.com/user-attachments/assets/ad848c89-c206-436c-ac5f-a2b013135e2e" />


## Stack
- FastAPI
- Jinja2 templates
- SQLite
- Docker / Docker Compose

## Features
- Book list with search
- Filters by reading status, reading year, and raw status
- Add, edit, and delete entries
- Initial import from Excel
- Top-level statistics (total, read, unread/not read, currently reading)
- Light\Dark Mode

## Fields
All fields are optional except `title`.

## Reading status mapping
The app stores both:
- `status_raw`: the original value imported from Excel
- `reading_bucket`: a normalized value used for filters (`read`, `not_read`, `reading`, `other`)

## Run
```bash
docker compose up --build
```

Then open:
- http://localhost:8080
- or http://YOUR-MACHINE-IP:8080 from your LAN

## Excel import
Import Excel allow to load a xlsx with no formatting (just like **Bookshelf - Demo.xlsx**)

## Import deduplication
During import, a book is skipped if an entry with the same `title` + `author` pair already exists.
