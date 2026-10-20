# SecuExam

Secure exam paper distribution system built with Flask, SQLite, and a static HTML/CSS/JS frontend.

## Problem and approach

Exam papers need to reach authorized recipients at the right time, while leaving a record of who accessed them. SecuExam models that workflow with separate setter, receiver, and administrator roles. A setter uploads a paper; the server stores encrypted content and enforces a release time; a receiver downloads a watermarked copy; and an administrator reviews users and audit events.

```text
Setter upload -> Flask API -> encrypted storage + SQLite metadata
                                      |
                           release-time and role checks
                                      |
Receiver download <- watermarked PDF <- authorized request
                                      |
                               audit log / admin view
```

## What is included

- Role-based flows for `setter`, `receiver`, and `admin`
- AES-256 encryption with Shamir-style key splitting
- Server-side time-lock enforcement
- Dynamic PDF watermarking on receiver download
- Admin analytics, approval, audit logs, and paper visibility
- Selenium end-to-end test suite

## Project structure

- `server.py`: Flask backend and database logic
- `secuexam_app/`: frontend pages, CSS, JS, and encrypted upload storage
- `test_secuexam.py`: Selenium test suite
- `reset_demo_state.py`: cleanup/reset utility for generated local state
- `generate_final_report.py`: generator for the final SecuExam documentation PDF
- `SecuExam_Final_Project_Report.pdf`: optional output from the report generator (not committed)

## Local setup

From the cloned repository root, create an isolated Python environment and install the app dependencies:

```bash
python -m venv .venv
```

Activate it with `.venv\Scripts\Activate.ps1` on Windows PowerShell or `source .venv/bin/activate` on macOS/Linux, then run:

```bash
python -m pip install -r requirements.txt
```

For the browser test suite, also install Selenium with `python -m pip install selenium` and have Chrome or Safari WebDriver available. Selenium is not included in `requirements.txt`.

## Run the app

```bash
python server.py
```

Default URL: `http://127.0.0.1:5050`

The server currently binds to all network interfaces and seeds known demo passwords. Run it only in a trusted local environment; this setup is not suitable for a public deployment.

## Open It On Your Phone

SecuExam now includes installable PWA support, so it can behave like a mobile app on a phone.

1. Start the server:

```bash
python server.py
```

2. In another terminal, print the phone-friendly access URL:

```bash
python print_mobile_access.py
```

3. Open the shown LAN URL on the phone while the phone and laptop are on the same Wi-Fi.

4. Install it to the home screen:

- iPhone / iPad Safari: `Share` -> `Add to Home Screen`
- Android Chrome: `Install app` or `Add to Home screen`

Keep the Flask server running while using the phone app, and use this demo only on a trusted network.

## Demo accounts

These credentials are seeded for local demonstration and automated tests only. Do not expose the demo server to the internet with these accounts enabled.

- `admin@secuexam.in` / `admin123`
- `setter@vit.ac.in` / `setter123`
- `receiver@vit.ac.in` / `receiver123`

## Reset generated state

Use this if you want to clear the local DB, uploaded encrypted files, screenshots, and caches, then restore the default seeded users.

```bash
python reset_demo_state.py
```

Useful variants:

```bash
python reset_demo_state.py --dry-run
python reset_demo_state.py --keep-screenshots
```

Stop the running Flask server before performing a real reset.

## Run tests

```bash
python test_secuexam.py
```

Start the app in another terminal before running the suite. The repository reports a previous run of `34/34 tests passed`; this is not a current CI result.

## Demo and limitations

- The app is a local prototype. Its seeded demo accounts, SQLite storage, and current server configuration require hardening before public use.
- The browser tests require a separate Selenium install and an available WebDriver.
- There is no hosted demo or committed screenshot gallery. The Selenium suite saves screenshots locally in `test_screenshots/` when run.

## Final report

The final project documentation PDF is generated locally and is not committed to the repository.

To regenerate it:

```bash
python generate_final_report.py
```
