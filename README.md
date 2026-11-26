# افتقاد (Aftkad) - Sunday School Follow-Up Tracker

A web app for a church children's ministry (5th and 6th grade) that helps servants keep track of every child, so no one slips through the cracks.

**Live site:** https://aftkad-5598f.web.app

## What it does

- **Eftekad (pastoral follow-up):** tracks when each child was last visited or contacted, with a red alert when a child is overdue based on a configurable cycle (rolling days or calendar week).
- **Friday attendance:** take attendance per meeting, with attendance rates per child.
- **QR check-in:** generate a printable QR code for each child and scan codes at the door (uses qrcode.js and jsQR).
- **إخوة الرب section:** a separate list for children who are connected to the ministry through a sibling.
- **Roles:** separate sign-ins for the boys' servants, the girls' servants, and an admin view that sees everyone. Each role only sees and backs up its own group.
- **Backup and restore:** export everything to a CSV file (opens in Excel or Google Sheets) and restore from it.

## Tech stack

- Plain HTML, CSS, and vanilla JavaScript in a single page (no frameworks, no build step)
- Firebase Hosting
- Firebase Authentication (email and password, one account per role)
- Cloud Firestore (realtime sync across devices)
- Google Fonts (Cairo), qrcode.js and jsQR from cdnjs

## Setup

1. Create a Firebase project and enable **Authentication** (Email/Password) and **Cloud Firestore**.
2. In Authentication, create three users matching the emails in `index.html` (`boys@...`, `girls@...`, `admin@...`), each with its own password. You can change the emails in the `EMAILS` constant near the top of the script.
3. Copy your own Firebase web config into the `FIREBASE_CONFIG` constant in `index.html`. (The config in the repo is not a secret; it only identifies the project. Access is controlled by Authentication and the Firestore rules below.)
4. In the Firebase console, go to **Firestore Database → Rules**, paste the contents of `firestore.rules`, and publish. Access is role-based per servants account: boys' servants can access boys' records, girls' servants can access girls' records, and admin can access everything.
5. Deploy with `firebase deploy` (the Firebase CLI) or host the files anywhere as a static site.

## Privacy

The app stores personal data about real children (names, phone numbers, addresses, birthdays, confession father), so:

- Firestore access is role-based: boys' servants and girls' servants access their own group's records, while admin access is limited according to the rules. Never open the rules to the public.
- No personal data is kept in this repository; everything lives in Firestore.
- CSV backups contain personal data too, so treat the downloaded files as confidential.

## Passwords

There are no passwords in the code. All passwords live only in Firebase Authentication. If a password is forgotten, reset it from the Firebase console (Authentication → Users).
