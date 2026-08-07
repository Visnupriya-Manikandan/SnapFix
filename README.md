# SnapFix

AI-powered civic issue reporting — photograph a problem, and AI classifies the issue type, scores its severity, and routes it to the right municipal department in seconds.

> **Note:** the current HTML file's on-page title/heading still reads "NammaFix" — a leftover from an earlier build. Update those two spots if you want the live demo to match the "SnapFix" branding used in the pitch deck.

## What it does

1. Citizen uploads one or more photos of a civic issue (pothole, garbage overflow, broken streetlight, water leakage, etc.)
2. Optionally captures GPS location (reverse-geocoded to a readable address) and adds free-text context
3. AI vision model analyzes each photo and returns:
   - **Category** — Pothole / Garbage Overflow / Broken Streetlight / Water Leakage / Other
   - **Severity** — Low / Medium / High
   - **Confidence score** (0–100%)
   - **Description** of what's visibly wrong
   - **Safety note** on urgency/risk
4. App auto-routes the issue to the matching department and generates a ticket ID
5. Multiple photos can be batch-processed in one session

## Department routing map

| Category | Routed to |
|---|---|
| Pothole | Roads & Highways Department |
| Garbage Overflow | Sanitation Department |
| Broken Streetlight | Electricity / Street Lighting Department |
| Water Leakage | Water Works Department |
| Other | General Municipal Grievance Cell |

## Tech stack

- **HTML5 / CSS3 / vanilla JavaScript** — no framework, no build step
- **Google Gemini API** (`gemini-3.5-flash-lite`, vision) — photo classification, called directly from the browser
- **Browser Geolocation API** — captures citizen's GPS coordinates
- **OpenStreetMap Nominatim** — reverse-geocodes coordinates into a readable address
- **FileReader API** — image upload/preview, converts photos to base64 for the API call
- **In-memory only** — no backend, no database; ticket IDs and results exist only for the current browser session (nothing persists on refresh)

## Running it

This is a **standalone HTML file** — no install, no server, no build process.

1. Open `snapfix.html` in any modern browser (Chrome, Edge, Firefox)
2. Get a Google AI Studio (Gemini) API key at [aistudio.google.com](https://aistudio.google.com) — free tier is enough for a demo
3. Paste the key into the "Google AI Studio (Gemini) API Key" field and click **Set key**
   - The key is held only in that browser tab's memory for the session — it is never saved to disk or sent anywhere except Google's API
4. (Optional) Click **Use current GPS location** and allow location permission
5. Upload one or more photos, optionally add context, click **Analyze photos**

**Requires internet access at runtime** — the Gemini API call and the reverse-geocoding lookup both need connectivity. Everything else (UI, file handling) works offline.

## Known limitations / next steps

- **No backend** — results aren't stored anywhere; refreshing the page clears everything. A real deployment needs a database and persistent ticket storage.
- **API key exposed client-side** — fine for a demo on your own device, not safe for production or public hosting. A production version needs a backend proxy that holds the key server-side.
- **No authentication** — anyone with the page and a key can submit reports; no citizen accounts or officer login yet.
- **Fixed category set** — only 4 issue types are classified today; expanding categories means updating both the AI prompt and the department routing map.
- **No duplicate detection** — the same issue reported by multiple citizens currently creates separate tickets.

## Roadmap

See the pitch deck's roadmap slide for the phased plan: pilot in one municipality → backend + officer dashboard → scale across Tamil Nadu → replicate to other states.
