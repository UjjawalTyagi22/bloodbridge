# BloodBridge — Emergency Blood Donor Network

> A real-time web platform connecting blood donors with patients in urgent need.

---

## Overview

BloodBridge is a community-driven emergency blood donor network built as a single-page web application. It allows donors to register their blood type and availability, and lets hospitals or patients post urgent blood requests. The platform instantly matches compatible donors to requests using a blood type compatibility algorithm, sorted by GPS distance from the hospital.

All data is stored and synced in real-time using Firebase Firestore — meaning when a donor registers or a request is posted, every user sees the update instantly without refreshing the page.

---

## Live Demo

**[https://bloodbridge.vercel.app](https://bloodbridge.vercel.app)**

---

## Problem Statement

Every year, thousands of patients in India die due to unavailability of blood at the right time. Existing blood bank systems are slow, offline, or require phone calls through multiple intermediaries. BloodBridge eliminates the gap by creating a direct, real-time connection between willing donors and patients in need.

---

## Features

### Core Features
- **Real-time donor registry** — donors register once, visible to everyone instantly
- **Live blood request board** — requests appear in real-time across all open browsers
- **Smart matching algorithm** — finds compatible donors based on blood type compatibility rules
- **Distance sorting** — donors are sorted by GPS distance from the requesting hospital
- **Blood bank availability tracker** — live bar chart showing donor count per blood group
- **Mark as fulfilled** — requests can be closed once blood is arranged
- **Hospital autocomplete** — 40+ real Indian hospitals preloaded with city data

### Blood Type Compatibility
- Full 8-type compatibility matrix (O-, O+, A-, A+, B-, B+, AB-, AB+)
- Universal donor (O-) and universal recipient (AB+) highlighted
- Interactive compatibility reference cards

### UI/UX
- Responsive design — works on mobile and desktop
- Scroll-reveal animations
- Live indicator on the dashboard
- Toast notifications for all actions
- Emergency FAB button for urgent requests

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | HTML5, CSS3, Vanilla JavaScript |
| Database | Firebase Firestore (NoSQL, real-time) |
| Authentication | Firebase Auth (Phone OTP — optional) |
| Hosting | Vercel (CDN, auto-deploy) |
| Version Control | Git + GitHub |
| Fonts | Google Fonts — DM Sans |

---

## Architecture

```
User Browser
     │
     ├── index.html (Single Page Application)
     │     ├── Firebase SDK (ES Module, CDN)
     │     ├── Firestore real-time listeners (onSnapshot)
     │     └── Vanilla JS — rendering, matching, UI
     │
     └── Firebase Firestore (Cloud Database)
           ├── /donors        — registered donor documents
           └── /requests      — blood request documents
```

### Data Model

**Donor Document** (`/donors/{id}`)
```json
{
  "name": "Arjun Sharma",
  "blood": "O-",
  "age": 28,
  "city": "Dehradun",
  "phone": "+91 98765 43210",
  "available": true,
  "lat": 30.3165,
  "lng": 78.0322,
  "createdAt": "Timestamp"
}
```

**Request Document** (`/requests/{id}`)
```json
{
  "name": "Suresh Kumar",
  "blood": "B+",
  "units": 2,
  "hosp": "AIIMS Rishikesh",
  "city": "Rishikesh",
  "phone": "+91 92109 87654",
  "urg": "critical",
  "fulfilled": false,
  "lat": 30.1044,
  "lng": 78.2833,
  "createdAt": "Timestamp"
}
```

---

## Blood Compatibility Algorithm

When a request is posted for blood type `X`, the system finds all donors whose blood type can donate to `X`:

```javascript
const COMPAT = {
  'O-' : { receives: ['O-'] },
  'O+' : { receives: ['O-', 'O+'] },
  'A-' : { receives: ['O-', 'A-'] },
  'A+' : { receives: ['O-', 'O+', 'A-', 'A+'] },
  'B-' : { receives: ['O-', 'B-'] },
  'B+' : { receives: ['O-', 'O+', 'B-', 'B+'] },
  'AB-': { receives: ['O-', 'A-', 'B-', 'AB-'] },
  'AB+': { receives: ['O-', 'O+', 'A-', 'A+', 'B-', 'B+', 'AB-', 'AB+'] }
};
```

Matched donors are then sorted by Haversine distance from the hospital coordinates.

---

## Project Structure

```
Bloodbridge/
└── index.html        ← entire application (HTML + CSS + JS)
```

Single-file architecture — no build tools, no npm dependencies, no bundler required. The Firebase SDK is loaded from Google's CDN via ES Modules.


##  Developer

**Ujjawal Tyagi**
Computer Science & Engineering
GitHub: [github.com/UjjawalTyagi22](https://github.com/UjjawalTyagi22)

---

## License

This project is open source and available under the [MIT License](LICENSE).

---

> *"The blood you donate gives someone another chance at life."*




