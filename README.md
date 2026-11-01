# SplitMate

A fresh, original expense-sharing app concept for friends, flatmates and trips in India. The visual direction uses calm mint, forest green, warm coral and soft neutral surfaces. It is not a re-skin of Splitwise and does not include Splitwise source code or brand assets.

## What is in this first build

### Working in the local app
- Create and edit groups; add or update friends.
- Add, edit and delete expenses.
- Split equally, by exact amount, percentage, or shares.
- Save a preferred split method per group.
- Calculate balances from expenses and recorded repayments.
- Suggest simplified repayments within each group.
- Record cash, UPI, bank-transfer or other settle-ups; launch a recipient's UPI payment app when their UPI ID is saved (the payment is not verified or auto-recorded).
- Attach a receipt image to an expense (stored locally; no OCR yet).
- Create recurring expense templates and add the next occurrence manually.
- Categorise expenses, search expense history, and view category insights.
- Export expense CSV files, full JSON backups, or a single group JSON file; import a backup on another device.
- Work offline after the first load when hosted over HTTPS, and store device data locally.

### Not connected yet — required before a public multi-user launch
- **Real-time cloud sync, user accounts and invite links.** This first build is local-first. Sharing a group JSON file is a manual hand-off, not live collaboration.
- **₹10/month and ₹99/year subscriptions.** Prices are shown as the intended plan; payment checkout, server-side subscription verification and account-level Pro entitlements still need to be integrated.
- **Receipt OCR and automated itemisation.** The app can attach a receipt photo, but does not read it automatically yet.
- **Automatic recurring schedules and notifications.** A template can be advanced manually; it does not run in the background.
- **Currency conversion and multi-currency group accounting.** This India-first build currently uses INR so balances are not incorrectly mixed across currencies.
- **Bank/card transaction import, payment processor integrations, push notifications and additional languages.**

The current feature set is a strong local prototype, not yet full feature parity with Splitwise. Build the missing online services before inviting the public or collecting subscriptions.

## Run locally on a computer

1. Install Python 3.
2. From this folder run:

   ```bash
   python -m http.server 8000
   ```

3. Open `http://localhost:8000` in a browser.

Data is saved in that browser's local storage. Use **Settings & backup → Export full backup** regularly.

## Install on Android as a web app

1. Publish the `site/` folder using GitHub Pages (a workflow is included below).
2. Open the resulting HTTPS URL in Chrome on Android.
3. Open Chrome's menu and choose **Install app** or **Add to Home screen**.

This creates an installable web app (PWA), not an APK.

## Build an Android APK with GitHub Actions

An Android WebView shell project is included in `android/app`. It packages the same app files locally so the core screens work without a network connection.

1. Create a GitHub repository and upload/push this folder to the `main` branch.
2. Open the repository's **Actions** tab.
3. Run **Build SplitMate Android APK** (or let it run after a push to `main`).
4. Download the `splitmate-debug-apk` artifact from the completed workflow run.
5. Extract `app-debug.apk`, transfer it to the Android phone, and install it. Android may ask you to allow installation from that source.

The workflow runs a small smoke test for balance/debt logic, then builds a **debug APK for private testing**. It is not a Play Store release build and is signed with a debug key. Before a public release, configure a private release signing key, review Android privacy/security requirements, and build a signed release APK/AAB.

## Publish the web app on GitHub Pages

A second workflow, **Deploy SplitMate web app**, publishes the `site/` folder. In repository settings, enable GitHub Pages with **GitHub Actions** as the source if it is not already selected. The published URL will appear in the workflow deployment summary.

## Run the local smoke tests

With Node.js installed, run `node tests/smoke.test.cjs`. The test checks the demo group balance calculations, repayment simplification and initial dashboard render.

## Project layout

- `index.html` — main responsive app and local data logic.
- `manifest.webmanifest`, `sw.js`, `assets/` — installable web app metadata, offline shell and icons.
- `site/` — GitHub Pages copy of the web app.
- `android/app/` — Android WebView wrapper and local web assets.
- `.github/workflows/` — APK build and Pages deployment workflows.

## Important privacy note

The prototype stores profile details, expenses and attached receipt images on the local device/browser. It does not send them to a server. A JSON backup contains the information you export, so share it only with intended group members. Before adding cloud sync, use authenticated access controls, row-level permissions, encrypted transport, secure secret management and tested backup/deletion procedures.
