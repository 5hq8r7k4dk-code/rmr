# RMR Project TODO

## Context
- `rmr/` = frontend (vanilla JS/HTML/CSS)
- `roomate_review/` = backend (Node.js, Vercel serverless, PostgreSQL)
- Goal: replace hardcoded frontend data with real DB data via API
- Note: teammate owns login, ratings, comments, and apartments endpoints — coordinate before touching those

## Known Issues to Fix First (Frontend)
- [x] apartment.html:51 — stray backtick syntax bug
- [ ] Rating scale mismatch: frontend 0–5 stars, backend stores 0–9 (conversion: star × 1.8)
- [x] Category name mismatches: "Social"→sociability, "Communicative"→communication

## apartment.html
- [x] Replace hardcoded `people` array with fetch(`/api/roomates`)
- [x] Built filtered roommates endpoint (`/api/roomates?userId=X`) in backend
- [ ] Update fetch URL to `/api/roomates?userId=X` — ready on backend, blocked on auth (need logged-in user's ID)
- [ ] Fix "You" detection — compare by user ID from session, not name string (blocked on auth)
- [ ] Wire `submitRating` to POST ratings to backend (blocked — teammate building `POST /api/ratings`)

## studentchat.html
- [ ] Replace hardcoded `tenants` array with fetch(`/api/roomates`)
- [ ] Map API response fields to tenant object shape:
  - `username` → `name`, ratings → compute star avg from 5 attribute columns (convert 0–9 to 0–5)
  - `budget`, `tags`, `unit` — NOT in DB yet (teammate adding columns)
- [ ] Compatibility/filter features blocked until `budget` + `tags` columns exist in DB
- [ ] Comments: no API endpoint yet (teammate building it); hardcoded reviews are placeholder
- [ ] Avatar colors: generate deterministically from user ID when real data loads

## login.html / signup.html
- [x] signup.html: add phone, bio, and profile picture fields to form
- [x] signup.html: fix field id mismatches (`name`→`username`, `password`→`pass`)
- [x] signup.html: swap profile picture URL input for a styled file picker
- [x] signup.html: style new fields (tel, url, textarea) to match existing inputs in style.css
- [x] signup.html: wire form to POST `/api/users` (endpoint exists)
- [ ] signup.html: backend needs to handle file upload for profile picture (store image, return URL to save in DB)
- [ ] login.html: blocked — no `/api/login` endpoint yet (teammate building)
- [ ] Auth strategy TBD (JWT likely) — needed before "You" detection works anywhere

## finding.html
- [ ] Hardcoded apartment list — blocked until teammate creates apartments table/endpoint

## Deployment
- [ ] Decide: deploy both to Vercel (recommended) OR add CORS headers to all API routes
- [ ] If separate deployments: add `Access-Control-Allow-Origin` to every API route

## Backend TODO (teammate's work — for reference)
- [ ] Add `unit`, `budget`, `tags` columns to `users` table
- [ ] Add `/api/login` endpoint + password hashing (currently plaintext)
- [ ] Add `/api/ratings` POST endpoint (for submitting from apartment.html)
- [ ] Add `/api/comments` GET/POST endpoints
- [x] Add filtered roommates endpoint: GET `/api/roomates?userId=X`
- [ ] Add file upload handling for profile pictures (return URL to store in `image_url`)
- [ ] Create `apartment` table + endpoints (for finding.html)
- [x] Update `POST /api/users` to also insert a blank row into `rmr.attributes` for new users
- [x] Update `POST /api/users` to accept and insert `bio` field
