# VolTrack — Dev Log & Prompt History

A full record of every prompt used to build this project, why it was made, and what changed.

---

## 1. Initial concept
**Prompt:** Hackathon challenge — create a system for a fast fraud-resistant digital check-in for student volunteers that auto-calculates hours by event type, tracks partial attendance, and exports structured data for teacher verification.

**Why:** This was the core hackathon brief. We needed a full working system from scratch.

**What happened:** Built the first version of VolTrack as an inline chat widget with:
- 6 event types with base and max hour ranges
- Student ID input + event selector
- 7 fraud detection checks (duplicate check-ins, concurrent sessions, speed-runs, 4+ events/day, rapid re-entry, bad ID format, duration over max)
- Partial attendance detection (below 85% of base hours = partial)
- Live activity feed
- Attendance log with filters
- Summary stats tab
- 4 export formats: full CSV, student hour summary, flagged-only CSV, structured JSON

---

## 2. Make it a standalone app
**Prompt:** But we want it to be its own thing, maybe an app or website thing.

**Why:** The widget was embedded in the chat — we needed a real downloadable file that could be hosted or opened directly in a browser.

**What happened:** Rebuilt everything as a single `volunteer-checkin.html` file with:
- Full dark-themed UI using Syne + DM Sans + DM Mono fonts
- Sidebar navigation with 4 tabs
- No dependencies — opens in any browser, no install needed
- Deployable to GitHub Pages, Netlify, or any static host

---

## 3. Git Bash deployment
**Prompt:** Can it be done on things like Git Bash?

**Why:** Needed to know how to actually get it live for the hackathon demo.

**What happened:** Provided exact Git Bash commands to clone a repo, copy the file in as `index.html`, commit, push, and enable GitHub Pages — resulting in a live URL at `https://username.github.io/repo-name`.

---

## 4. Add school email login + class selector
**Prompt:** Yes, and an option to choose a class from the following [list of MetroEd programs]. Also make it so emails in the format `_._@my.metroed.net` are student emails.

**Why:** Students needed to identify themselves with their real school email and program so records are tied to actual people.

**What happened:**
- Replaced generic Student ID field with a school email input
- Added a dropdown with all 23 MetroEd CTE programs
- Email validation enforces `firstname.lastname@my.metroed.net` format
- Both fields required before submitting

---

## 5. Update event types
**Prompt:** Make the event types be: Campus tours, Shadow Days, School presentations, Public events.

**Why:** The original generic event types didn't match the actual volunteer activities at the school.

**What happened:** Replaced all 6 original event types with the 4 real ones, keeping icon/base/max hour structure.

---

## 6. Teacher login + In Progress tab + check-in/out times
**Prompt:** Teachers and students have different logins. Students can see In Progress and the attendance log but can't change anything. Teachers log in and see pending hours to verify and check off. Also make the check-in time locked and students can't change it.

**Why:** Needed a real two-role system — students submit, teachers verify. Fraud resistance required locking the timestamp.

**What happened:**
- Added teacher email format: `firstinitial+lastname@metroed.net` (e.g. `jsmith@metroed.net`) — no dot, no `@my`
- Students can view In Progress and their own log (read-only)
- Teachers see everyone's records and have Verify buttons
- Check-in time locked to current time — no manual editing
- "In Progress" split into two tables: pending check-ins and pending check-outs
- Each has a ✓ Verify button teachers click to approve
- Verified-by field stored and shown in the log
- Export updated to include `Volunteer name | Period | Hr in | Hr out | Total hrs` data sheet format

---

## 7. Full architecture rewrite
**Prompt:** Full redesign — separate student and teacher login screens, locked time, in-progress tab for both roles, data sheet export.

**Why:** The previous version had teacher auth bolted on top. Needed a clean rebuild with roles baked in from the start.

**What happened:** Complete rewrite of the app with:
- Login screen with Student / Teacher tabs
- Role-based nav — students see Check-in, In Progress (read-only), My Log; teachers see In Progress, Attendance Log, Summary, Export
- Student period (AM/PM) stored on login and used in all records
- `sessionStorage` used to persist data across page refreshes within a session
- Volunteer data sheet export pulls only verified records

---

## 8. AM/PM class period on login
**Prompt:** Make the login page also ask if the student is in the PM or AM class.

**Why:** Period is required for teacher reporting — the data sheet needs AM/PM per student.

**What happened:**
- Added two toggle buttons (☀️ AM class / 🌙 PM class) to the student login form
- Required field — can't sign in without selecting
- Stored on the user object and written to every record
- Period column in all tables and exports uses the student's self-reported period (not inferred from clock time)
- Sidebar shows `Student · AM class` or `Student · PM class`

---

## 9. Teacher event duration control
**Prompt:** For the staff section, make it so they can control how long an activity is, and that can be shown on the event page.

**Why:** Teachers need to set expected hours per event — defaults may not match actual scheduled activity lengths.

**What happened:**
- Added an "Event Settings" tab (gear icon) to the teacher sidebar
- Each event has two editable fields: Expected hrs (base) and Maximum hrs (cap)
- Settings saved to `sessionStorage` and persist across page reloads
- Student event cards immediately reflect teacher-set hours
- Fraud detection and hour calculation both use teacher-set max

---

## 10. Fix stretched font + add Other event
**Prompt:** I don't like how the font looks stretched. Fix that. Also add another event that just says "Other."

**Why:** Syne was a wide display font that felt out of place. "Other" was needed for miscellaneous volunteer activities.

**What happened:**
- Removed Syne font entirely, switched to DM Sans throughout (already used for body text) — consistent, clean, no stretching
- Added "Other" (📋, 1–4h default) as a 5th event type
- Event grid updated to 3 columns to fit 5 cards cleanly

---

## 11. Separate check-in and check-out into tabs
**Prompt:** Make it so after you check in on an event, you have another tab for the event and after the event in person is done, you click "Check out." Makes it easier to understand.

**Why:** The previous flow was confusing — submitting twice on the same form wasn't intuitive for students.

**What happened:**
- Split the check-in screen into two tabs: **Check in** and **Active Events**
- Check in tab: pick event type → locked time shown → hit "Check in"
- Active Events tab: shows a card for every event the student is currently checked in to, with event name, time since check-in, and a big amber **"Check out"** button
- Checkout time is also locked to the moment the button is tapped
- Active badge on the tab shows how many open check-ins exist
- If a student tries to check in to an event they're already in, they get redirected to the Active Events tab automatically

---

## File structure

```
your-repo/
├── index.html      ← the entire app (single file, no dependencies)
└── DEVLOG.md       ← this file
```

## How to deploy

```bash
git clone https://github.com/YOUR_USERNAME/YOUR_REPO.git
cd YOUR_REPO
cp ~/Downloads/volunteer-checkin.html index.html
git add .
git commit -m "your message here"
git push
```

Then go to **GitHub repo → Settings → Pages → Branch: main → Save**.

Live at: `https://YOUR_USERNAME.github.io/YOUR_REPO`

---

## 12. Per-user data separation
**Prompt:** It doesn't save for each user — when I log out and put a different email, it shows the previous email's data.

**Why:** All records were stored in one shared `vt_rec` key, so every user saw everyone else's data.

**What happened:**
- Records now saved under individual keys per email (`vt_rec_firstname.lastname@my.metroed.net`)
- Signing in loads only that email's records — different student = clean slate
- Teachers still see everyone by collecting all `vt_rec_*` keys on login
- Teacher verifications write back to the correct student's key
- Sign out clears records from memory so nothing leaks between sessions

---

## 13. Event options disappeared
**Prompt:** The event options are gone.

**Why:** The `getEvHours` function was accidentally deleted during the per-user storage rewrite, which caused `renderEvCards` to crash silently and show nothing.

**What happened:** Restored the missing `getEvHours` function which reads teacher-set hour overrides (or falls back to defaults) for each event type. Event cards immediately returned.

---

*Built with Claude (Anthropic) as part of a hackathon challenge.*
