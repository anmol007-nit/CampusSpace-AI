# Campus Space AI

**Right Room. Right Time. Zero Conflicts.**  
An AI-powered smart campus operating system prototype by **Amateur AI DAPS**.

Campus Space AI is a hackathon-ready React application for discovering sample campus rooms, checking availability, and booking spaces. Recommendations use transparent, deterministic rules: availability, capacity, required equipment, room type, and building preference. They are not machine learning.

## Run locally

1. Install Node.js 20.19+ or 22.12+.
2. From the project folder, install dependencies:

   ```sh
   npm install
   ```

3. Start the development server:

   ```sh
   npm run dev
   ```

4. Open the local URL printed by Vite. Create a production build with `npm run build`.

No API key or backend setup is needed for the demo.

Event attendance uses the FastAPI backend. Start it from the project root on port `8000` (for example, `uvicorn backend.app.main:app --host 0.0.0.0 --port 8000`). The frontend defaults to the current hostname on port `8000`; set `VITE_API_URL` if the API is hosted elsewhere. For QR links opened on a phone, the frontend and API host must both be reachable from that phone.

## What works

- **Overview:** live totals, today's sample reservations, available-now checks, and utilization calculated from booking durations.
- **Room search:** search by name, building, type, or equipment; filter by date, time, capacity, type, and must-have equipment; inspect room details and accessibility notes.
- **Campus AI:** describe a room request in natural language to get transparent recommendations from the built-in, offline rules engine. It checks room capacity, equipment, accessibility, and browser-local booking conflicts; it never creates or confirms a booking.
- **My timetable:** manage a personal repeating weekly schedule with add, edit, delete, weekday filters, overlap checks, and an offline AI helper that drafts entries from natural language. Timetable data is saved in this browser and does not require an account.
- **Event attendance:** faculty-role and authorized staff accounts can create events and display a QR check-in link. Students and faculty must sign in before check-in; the backend stores one attendance record per account and event, including the attendee's account name, role, and check-in time. Event organizers can view the attendance list.
- **Bookings:** create and cancel browser-local reservations. The form validates required fields, local campus time, positive whole-number attendance, room capacity, and equipment. Adjacent reservations are allowed; overlapping reservations for the same room are rejected.
- **Alternatives:** deterministic recommendations exclude occupied or unsuitable rooms before ranking good fits. The conflict screen explains matches and can transfer the requested time and requirements to the room search.
- **Bookings, calendar, analytics, and administration-preview screens:** render current application records; administrative data is read-only and clearly labeled as a preview.
- **Persistence:** demo bookings use this browser's local storage. If browser storage cannot be read, the app reports the problem and remains usable with in-memory sample records for that session.
- **Event persistence:** events and attendance use the FastAPI backend and SQLite database. Start the backend to use this feature. Scanning the QR opens the app's check-in link; attendees sign in and confirm check-in. The QR itself is not identity verification.
- **Responsive layout:** navigation, forms, room cards, tables, and agenda adapt to narrow screens.

All room records and the preloaded bookings are **demonstration data**. They are not verified real campus records. Local storage and browser-side conflict checks are not shared across browsers or devices and are not safe against simultaneous multi-user production bookings.

## Testing checklist

- [ ] Book an available room; confirm it appears on the bookings page and dashboard.
- [ ] Try an overlapping reservation for the same room; verify it is rejected and alternatives appear.
- [ ] Book the same room in the immediately adjacent slot; verify it is allowed.
- [ ] Exceed a room's capacity; verify the request is rejected.
- [ ] Require equipment that the selected room does not have; verify it is rejected and not recommended elsewhere.
- [ ] Create a conflict and verify another available room with enough seats and the required equipment is recommended.
- [ ] Ask Campus AI for a room with a date, time, attendee count, and equipment; verify its recommendations follow those requirements and its booking action opens a prefilled request.
- [ ] Ask Campus AI to draft a recurring timetable entry; review and save it, edit it, delete it, and verify it persists after refreshing this browser.
- [ ] Sign in as faculty, create an event, and open its QR check-in link.
- [ ] Register/sign in as a student, use the QR link, and confirm attendance; verify duplicate check-in is rejected and the organizer sees the attendee record.
- [ ] Refresh; verify demo bookings remain in this browser.
- [ ] Deny or clear local storage; verify the warning is understandable and the rest of the demo remains usable.
- [ ] Check the room search and booking form at a narrow viewport.
- [ ] Run `npm run build`.

## Not connected / not implemented

- **Backend:** The event check-in feature uses the existing FastAPI backend and local SQLite database. The broader room directory and browser demo bookings are not synced to it.
- **Identity and authorization:** Sign-in uses backend accounts, but this prototype does not verify student/faculty status against an institutional identity provider. Account roles supplied during public registration are demo claims and must not be trusted for production.
- **Not in this MVP:** Institutional identity verification, QR camera scanning inside the app, and live campus integrations.

Never use demo roles or client-side validation as production authorization or booking integrity controls. A production rollout needs authenticated server-side authorization, database row-level security, and a database-enforced overlap constraint.

## Demo walkthrough

> “Hi, we’re Amateur AI DAPS, and this is Campus Space AI: Right Room, Right Time, Zero Conflicts. Campus teams often coordinate room bookings across disconnected schedules, which leads to clashes and wasted time. Here I can search sample campus spaces by date, time, capacity, and equipment, inspect a room, and submit a booking. The app checks capacity and required equipment, prevents overlapping reservations while allowing back-to-back bookings, and saves this demo reservation in the browser. If a room is already booked, transparent rules filter out unavailable or unsuitable rooms and explain the best alternatives. The dashboard, agenda, and analytics update from the same records. This prototype runs without credentials; bookings are local to this browser. Connecting authenticated users and database-enforced booking integrity is the next step.”
