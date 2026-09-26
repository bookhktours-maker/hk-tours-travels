# Connect your enquiry form to a Google Sheet

This turns your Google Sheet into a free enquiry dashboard. Every submission on your website becomes a new row automatically. Takes about 5 minutes, no coding needed — just copy and paste.

## 1. Create the sheet

1. Go to [sheets.google.com](https://sheets.google.com) and create a **blank spreadsheet**.
2. Rename it something like **HK Tours Enquiries**.
3. In row 1, add these headers (one per column, A to H):
   `Timestamp | Name | Phone | WhatsApp | Country | Dates | Group Size | Message`

## 2. Add the script that receives form data

1. In your sheet, click **Extensions → Apps Script**.
2. Delete anything in the code box and paste this in:

```javascript
function doPost(e) {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
  var data = e.parameter;
  sheet.appendRow([
    new Date(),
    data.name || '',
    data.phone || '',
    data.whatsapp || '',
    data.country || '',
    data.dates || '',
    data.groupsize || '',
    data.message || ''
  ]);
  return ContentService.createTextOutput(JSON.stringify({ status: 'success' }))
    .setMimeType(ContentService.MimeType.JSON);
}
```

3. Click the **Save** icon (or Ctrl+S). Name the project anything, e.g. "Enquiry Handler".

## 3. Publish it as a Web App

1. Click **Deploy → New deployment**.
2. Click the gear icon next to "Select type" → choose **Web app**.
3. Fill in:
   - **Execute as:** Me (your Google account)
   - **Who has access:** Anyone
4. Click **Deploy**.
5. Google will ask you to **authorize** — click through the permission screens (it'll show an "unverified app" warning since it's your own personal script; click **Advanced → Go to [project name] (unsafe)** — this is safe, it's just Google being cautious about scripts it hasn't reviewed).
6. After deploying, copy the **Web app URL** it gives you — it looks like:
   `https://script.google.com/macros/s/AKfycb.../exec`

## 4. Connect it to your website

1. Open `index.html`.
2. Find this line near the bottom:
   ```javascript
   const SCRIPT_URL = "PASTE-YOUR-APPS-SCRIPT-WEB-APP-URL-HERE";
   ```
3. Replace the placeholder text with the URL you copied, keeping the quotes:
   ```javascript
   const SCRIPT_URL = "https://script.google.com/macros/s/AKfycb.../exec";
   ```
4. Save the file and re-upload it to your GitHub repo (or edit it directly on GitHub: open the file → pencil icon → paste the change → Commit).

## 5. Test it

1. Open your live GitHub Pages site.
2. Fill in the enquiry form with test details and click **Send enquiry**.
3. Check your Google Sheet — a new row should appear within a few seconds.

That's it. Every enquiry from your site now lands as a row in your sheet, which you can open on your phone, sort by date, mark as "contacted" in an extra column, or filter by country — all for free, with no submission limit.

## If it stops working later

Whenever you edit the Apps Script code itself (not just the sheet data), you need to redeploy: **Deploy → Manage deployments → pencil icon → New version → Deploy**. The URL stays the same, so you won't need to update `index.html` again unless you create a brand new deployment.
