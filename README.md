# UberHamster Website

A multi-page marketing and booking website for a hamster boarding service.

The project is built with plain HTML/CSS/JavaScript and a lightweight Node.js server that:
- serves static assets and pages
- handles reservation form submissions
- validates reservation business rules
- stores submitted reservations in newline-delimited JSON (`reservations.ndjson`)

## Tech Stack

- Frontend: HTML, CSS, vanilla JavaScript (`app.js`)
- Backend: Node.js built-in modules (`http`, `fs`, `path`) in `server.js`
- Data storage: append-only local file (`reservations.ndjson`)

## Project Structure

```text
.
├── index.html            # Home page
├── reservations.html     # Reservation form page
├── gallery.html          # Gallery page
├── pricing.html          # Pricing page
├── faq.html              # FAQ page
├── styles.css            # Site styles
├── app.js                # Frontend interactions + form submission
├── server.js             # HTTP server + API endpoint
├── assets/               # SVG illustrations/gallery images
└── reservations.ndjson   # Submitted reservation records
```

## Run Locally

Requirements:
- Node.js 18+ (or any modern Node runtime with `fetch` support in browsers)

Start the server:

```bash
node server.js
```

Open:

[http://localhost:3000](http://localhost:3000)

You can change the port with:

```bash
PORT=4000 node server.js
```

## Pages and Routes

Static pages:
- `/` -> `index.html`
- `/reservations.html`
- `/gallery.html`
- `/pricing.html`
- `/faq.html`

API route:
- `POST /api/reservations`

## Reservation Form Behavior

Client-side (`app.js`) and server-side (`server.js`) validation enforce:
- required fields: `owner-name`, `email`, `hamster-name`, `check-in`, `check-out`
- valid email format
- `check-in` must be at least 2 days from today
- `check-out` must be after `check-in`

On successful submit:
- the server appends a record to `reservations.ndjson`
- the API returns `201` with a confirmation message

On validation error:
- the API returns `400` with an `error` and `errors` array

## Example API Request

```bash
curl -X POST http://localhost:3000/api/reservations \
  -H "Content-Type: application/json" \
  -d '{
    "owner-name": "Jordan Lee",
    "email": "jordan@example.com",
    "hamster-name": "Mochi",
    "hamster-type": "Syrian",
    "check-in": "2026-03-01",
    "check-out": "2026-03-04",
    "notes": "Prefers evening activity."
  }'
```

## Notes

- This is a simple file-based prototype and does not include authentication, rate limiting, or database storage.
- Reservations are persisted as local data in `reservations.ndjson`; handle that file carefully in shared environments.
