DiggingDeal Seller Portal. Built files only; confidential logic and access controlled by Supabase RLS.

## Android APK / QR installer

The seller center loads the static `apps.html` page via the **앱 설치 · QR** shortcut.
This page reads `app-builds.json` (no secrets) and renders separate installation cards for:

- `apps.customer`: customer Android app (live viewing)
- `apps.seller`: seller Android app (broadcast streaming)

### Publishing an APK

1. Build each Android application as an installable **APK**, not an AAB, and test the
   APK on a real Android device. Confirm the sign-in flow and core app functions.
2. Upload/publish each APK to an HTTPS download address; Expo EAS internal
   distribution pages are supported if their install permissions allow the audience.
3. Update the corresponding `app-builds.json` entry with:
   `enabled: true`, `apkUrl`, `version`, `releasedAt` (YYYY-MM-DD),
   and optionally `sha256`. **Never publish a guessed/expired URL.**
4. Publish the website and test the install button and QR scan with a phone.
   The QR points to the *stable install page* with `?app=customer` or
   `?app=seller`, so its URL remains unchanged after a new APK release.
5. If an APK is revoked or broken, set `enabled: false`. This hides the
   download action and QR rather than sending people to a dead installer.

The starter manifest deliberately contains no APK URLs, because a validated
download address has not yet been provided. QR codes and install actions
become active only when an enabled HTTPS URL is present.

QR image rendering uses a third-party public QR endpoint for the public page
URL; no sign-in tokens or user data are included. A clickable installation
button remains available if the QR image service is unavailable.

**Deployment note:** This repository stores built site files only. If the
upstream seller web app is rebuilt/deployed independently, carry the
`index.html` shortcut and `apps.html` / `app-builds.json` files into
its build pipeline, otherwise a future deployment may overwrite the entry.
