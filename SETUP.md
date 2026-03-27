# Aera Landing Page Setup

## 1. Background Video

Add your looping video here:

- `assets/background-loop.mp4`

The page is already wired to autoplay, mute, loop, and play inline.

## 2. Waitlist Storage

The easiest spreadsheet-backed setup is:

- Google Sheet
- Google Apps Script web app

That gives you a live spreadsheet you can open anytime, without needing a separate backend.

## 3. Google Sheet Steps

1. Create a Google Sheet with headers in row 1:
   - `email`
   - `source`
   - `submitted_at`
2. In the Sheet, open `Extensions > Apps Script`.
3. Replace the default code with this:

```javascript
function doPost(e) {
  const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  const data = JSON.parse(e.postData.contents);

  sheet.appendRow([
    data.email || "",
    data.source || "",
    data.submittedAt || new Date().toISOString()
  ]);

  return ContentService
    .createTextOutput(JSON.stringify({ ok: true }))
    .setMimeType(ContentService.MimeType.JSON);
}
```

4. Copy the contents of [Code.gs](/Users/kimdang/Documents/GitHub/hydration-earring-landing/google-apps-script/Code.gs) into Apps Script.
5. Click `Deploy > New deployment`.
5. Choose `Web app`.
6. Set:
   - Execute as: `Me`
   - Who has access: `Anyone`
7. Copy the deployed web app URL.

## 4. Connect the Form

Create a file named `config.js` next to `index.html`, based on [config.example.js](/Users/kimdang/Documents/GitHub/hydration-earring-landing/config.example.js), and set:

```javascript
window.AERA_CONFIG = {
  waitlistEndpoint: "PASTE_YOUR_GOOGLE_APPS_SCRIPT_URL_HERE"
};
```

## 5. Optional Fields

If you want, I can also add:

- first name
- referral source
- hidden UTM tracking
- double opt-in email service later
