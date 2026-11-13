# ActID

**A digital identity wallet for Romanian citizens, built on the EU Digital Identity (EUDI) Wallet standards.**

Built in 48 hours at **Cluj Hackathon 2026** (theme: *Digital Romania*) by a team of four.

Romanian version (the team's original README): [README.ro.md](README.ro.md)

<br>

<p align="center"><img src="images/dashboard.png" width="760" alt="ActID dashboard"></p>

## What it does

ActID is a mobile-first web app where citizens keep their official documents in one place: ID card, passport, driving licence, criminal record certificate and road vignette. They can then share only the information a situation needs.

- **Selective disclosure.** You choose exactly which details to reveal. For example, you can prove you're over 18 without showing your personal ID number (CNP). Documents are issued as **SD-JWT Verifiable Credentials** signed with ES256.
- **Verification by a government clerk.** A clerk scans the citizen's QR code and sees only the details the citizen disclosed, with a "Verified by the Romanian State" badge from a trusted-issuer registry.
- **Expiry alerts.** You get notified 30, 7 and 1 day before a document expires, and you can request a renewal from the app.
- **Family management.** Parents manage their children's documents, and guardian access ends automatically when the child turns 18.
- **Encryption at rest.** Sensitive fields are encrypted with AES-256-GCM, using per-user keys derived with HKDF-SHA256.
- **Tamper-evident audit log.** Every action is chained with SHA-256 hashes, and the chain can be checked for integrity from the app.
- **ID scanning.** The app reads the MRZ code on ID cards and runs a liveness and face check in the browser.

## Screenshots

| 1. Citizen picks what to share | 2. App generates a QR code | 3. Clerk sees only those details |
|---|---|---|
| <img src="images/presentation.png" width="260" alt="Choosing which attributes to share"> | <img src="images/presentation-qr.png" width="260" alt="Generated presentation QR code"> | <img src="images/functionar.png" width="260" alt="Clerk portal showing verified attributes"> |

## My contributions

I worked on the frontend logic and parts of the backend:

- **Family and guardian management**, full stack: parent and child profiles, guardian links, children's documents, lookup by student ID, and guardian access that expires automatically at age 18 (FastAPI endpoints, SQLAlchemy models and the React page).
- **Wallet Security page**: shows the encryption status, the trusted issuers, the issuer key fingerprint and the history of shared presentations.
- **Document renewal flow**: the `POST /documents/renewal-request` endpoint and its UI, usable both by the owner and by a family member.
- **QR scan page** and token previews for the sharing flow.
- **API client and error handling**: the Axios interceptor, a shared error helper, the notification store and confirmation dialogs used across the app.

| Family page | Wallet Security page |
|---|---|
| <img src="images/family.png" width="380" alt="Family page"> | <img src="images/security.png" width="380" alt="Wallet Security page"> |

## Tech stack

| Layer | Technologies |
|---|---|
| Frontend | React 18, TypeScript, Vite, Tailwind CSS, Zustand, React Router, face-api.js |
| Backend | Python, FastAPI, SQLAlchemy, SQLite |
| Identity & crypto | SD-JWT VC (IETF draft), ES256 / P-256, AES-256-GCM, HKDF-SHA256, JWT auth with 2FA |
| ID scanning | PassportEye (MRZ), OpenCV, face_recognition |
| Deployment | Docker Compose, nginx |

## Running it locally

**Requirements:** Python 3.10+, Node.js 18+.

```bash
git clone https://github.com/Ayiato/Cluj-Hackathon-2026.git
cd Cluj-Hackathon-2026/actid
./start.sh
```

Open `http://localhost:5173`. Or run it with Docker: `docker compose up --build`, then open `http://localhost`.

The interactive API docs are at `http://localhost:8000/docs`.

**Demo accounts** (password `Parola@123`, 2FA code `123456`):

| Account | Role |
|---|---|
| `ion.popescu@gmail.com` | Citizen whose ID card expires in 30 days |
| `alex.ionescu@gmail.com` | Citizen living abroad who manages a parent's documents |
| `functionar@spclep.ro` | Government clerk who verifies QR presentations |

All data in the app is fictional demo data.

## Team

- Andrei Rusneac
- Alissia Platon
- Radu Puie
- Teodora Pleșa
