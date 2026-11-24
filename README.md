# Salon Queue Management System — Trimline

**Live demo:** NOT DEPLOYED YET — replace this line with your actual working GitHub Pages URL immediately after deployment.

**Hosted on:** GitHub Pages (free static hosting) + optional Firebase Firestore (free Spark tier, subject to quotas)

**Staff demo PIN:** `2468` (UI-only demo PIN; not secure authentication)

## What it does

A small, responsive salon queue application with seven screens: Home, Join Queue, My Status, Reception, Barber, TV Display, and Owner Settings. Customers can select up to three services and a preferred barber or Any, receive a token, view an approximate wait, and cancel before being called. Reception can add walk-ins, call eligible customers and mark no-shows after ten minutes. Barbers can Start/Done and take breaks. Admin can toggle services, duty and online joining. English/Gujarati interface labels are provided for core controls.

## IMPORTANT: two operating modes

- **Local demo mode (works immediately):** leave `config.js` empty. Data is saved in localStorage and synchronizes between tabs in the **same browser profile**. It does NOT synchronize between different phones or computers. Suitable only for local exploration.
- **Live shared mode (recommended for assignment):** connect Firebase Firestore as described below. Data is stored in one Firestore document and updates all connected devices in real time. Uses Firebase anonymous authentication. The free tier has usage limits. The provided database rules are intentionally permissive for a classroom demo; do not enter real customer data or use this system in a real salon.

## Setup: enable live cross-device queue (about 15–25 minutes)

1. Go to https://console.firebase.google.com/ and create a Firebase project (no paid plan required for this basic demo; select Spark if offered).
2. In **Build → Authentication → Sign-in method**, enable **Anonymous** and save.
3. In **Build → Firestore Database**, create a database. Choose a nearby available region; you can use production mode and add the rules in the next step.
4. Open Firestore **Rules** and replace the rules with the exact contents of `firestore.rules`; click **Publish**. These rules are for a public classroom demo only.
5. In **Project settings → Your apps**, register a **Web app**. Copy the `firebaseConfig` values into `config.js` as `window.FIREBASE_CONFIG = { apiKey: '...', authDomain: '...', projectId: '...', appId: '...', messagingSenderId: '...' };`. The Firebase Web config is public; **never** paste service-account JSON or admin private keys here.
6. Run locally with `python -m http.server 8080`, then open http://localhost:8080. You should see **Live database connected** in the footer. If not, check Firestore Rules, Anonymous Auth, browser console and config.
7. Open the same deployed URL in two separate devices. Join on one and check Reception on the other. Staff PIN: `2468`.

**Privacy warning:** Anyone with the URL can access the shared classroom demo database after anonymous sign-in; the staff PIN is only a UI gate and can be bypassed. Use invented names and dummy phone numbers. Reset/delete the Firestore demo document after your presentation. This is not production-secure software.

## Deploy for FREE using GitHub Pages

1. Sign in to https://github.com and create a **new PUBLIC repository**, for example `salon-queue-management`.
2. Extract this ZIP. Upload **all files inside** the `salon-queue-basic` folder to the repository root (including `index.html`, `styles.css`, `app.js`, `config.js`, `firestore.rules`, `README.md`, `prompts.md`, `reflection.md`). Or use Git commands below.
3. In the GitHub repository, open **Settings → Pages → Build and deployment**.
4. Set **Source: Deploy from a branch**, **Branch: main**, folder **/(root)**; save.
5. Wait a few minutes. Your link should resemble `https://YOUR-USERNAME.github.io/salon-queue-management/`.
6. Open the live link on a phone and check that the app loads. If using Firebase, check **Live database connected**.
7. **Edit the very top of this README**: replace `NOT DEPLOYED YET` with the **actual tested live URL**. Do not leave a placeholder in the submitted repository.
8. Put the public GitHub repository link and the working live URL in your submission.

Alternative free hosting: Vercel → Add New Project → Import GitHub repository → Framework Preset Other → Deploy. No build command is needed.

## Git CLI alternative

```bash
git init
git add .
git commit -m "Build basic salon queue management demo"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/salon-queue-management.git
git push -u origin main
```

## Main routes

The app uses GitHub-Pages-compatible hash routes: `#/`, `#/join`, `#/status/TOKEN-ID`, `#/lookup`, `#/reception`, `#/walkin`, `#/barbers`, `#/barber/ravi`, `#/display`, `#/admin`.

## Queue rules

- Arrival-order queue; for a barber, the earliest waiting customer who chose that barber or Any is eligible.
- A barber has at most one Called/In-chair customer.
- One active spot per phone number. Walk-ins can omit a phone number.
- Waiting → Called → In chair → Done; Waiting → Cancelled; Called → No-show (after 10 minutes).
- Online joining closes 30 minutes before closing. Reception walk-ins remain available.
- Token sequence resets on a new local date. Historical entries are retained (single document demo only).
- Estimates are approximate, based on compatible waiting services and the current barber's remaining work.

## Known limitations

This is a basic classroom project, not a commercial system. The shared Firebase demo stores customer data in one document and is unsuitable for large queues; the public demo security rules allow authenticated visitors to change shared state. The demo PIN is not secure. The phone lookup is not verified by OTP. Service prices/durations are fixed seed values (admin can enable/disable but not edit). No SMS, WhatsApp, appointments, payments, or automatic reminders. Gujarati localization covers core labels, but not every informational sentence. Local mode is not cross-device realtime. There is no automated browser suite.

## Submission checklist (from the supplied assignment guide)

- [ ] Public personal GitHub repository with all files
- [ ] **Actual working live URL at the very top of README**
- [ ] Staff PIN below live URL
- [ ] `prompts.md` with your genuine prompts in chronological order (add any other prompts you used)
- [ ] Three-minute demo video (mobile view or phone screen)
- [ ] Half-page reflection describing one agent mistake, how you found it, and what you changed
- [ ] Demonstrate Join → Status → Reception Call → Barber Start/Done → TV updates
- [ ] Submit both GitHub URL and live URL before **24 November 2026, 23:59**

## Demo video plan (~3 minutes)

0:00–0:25 Home and mobile layout; 0:25–1:00 Join with two services; 1:00–1:25 show token/status; 1:25–1:55 Reception Call Next; 1:55–2:20 Barber Start/Done; 2:20–2:40 TV view; 2:40–3:00 Admin online queue toggle and summary.
