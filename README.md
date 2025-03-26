# Zapier Webhook Trigger from GitHub Pages

This repository demonstrates how to trigger a Zapier webhook from a GitHub Pages–hosted HTML file. It uses form-encoded data to avoid CORS issues, making it easy to send information like Airtable Record IDs directly to Zapier.

## Overview
- **Goal**: Pass record-specific data (e.g., Airtable Record IDs) to a Zapier webhook.  
- **Key File**: [`triggerHook.html`](./triggerHook.html) (deployed via GitHub Pages).  
- **Why Form Data?**: Sending data as form-encoded bypasses browser preflight checks that occur with JSON headers, preventing CORS errors.

## Usage
1. **Enable GitHub Pages**  
   - In your repo’s **Settings → Pages**, choose the **main** branch (root folder if applicable).  
   - Copy the published URL (e.g., `https://YourUsername.github.io/zapier-webhook-tab/`).

2. **Construct Your Link**  
   - The link typically looks like:  
     ```
     https://YourUsername.github.io/zapier-webhook-tab/triggerHook.html
       ?url=https://hooks.zapier.com/hooks/catch/12345/abcde/
       &recordId=rec123XYZ
     ```
   - `url` is your Zapier webhook (for example, `https://hooks.zapier.com/hooks/catch/4536921/2ecfib1/`).  
   - `recordId` can be any identifier you need to send (e.g., from Airtable).

3. **Use in Airtable**  
   - Create a **Button** or **Formula** field to build the URL dynamically:  
     ```plaintext
     "https://YourUsername.github.io/zapier-webhook-tab/triggerHook.html?url="
     & ENCODE_URL_COMPONENT("https://hooks.zapier.com/hooks/catch/12345/abcde/")
     & "&recordId=" & RECORD_ID()
     ```
   - Clicking this button opens the GitHub Pages URL, which automatically sends form-encoded data to Zapier.

4. **Check Zapier**  
   - In your Zap, use **Catch Raw Hook** to receive the data.  
   - The raw body will look like `message=Triggered+from+GitHub+Page&recordId=rec123XYZ`.  
   - Use **Formatter** steps to split it, or see the professional tip below for a cleaner solution.

## Professional Tip: Code by Zapier Parsing
Instead of manually splitting text, add a **Code by Zapier** step (JavaScript) after your webhook trigger:

```js
const params = new URLSearchParams(inputData.rawBody);
return {
  message: params.get("message"),
  recordId: params.get("recordId")
};
```

This outputs clean fields (`message`, `recordId`) you can map in later Zap steps.

## Final Notes
```html
<p>If you want the tab to close automatically after sending data, uncomment the <code>window.close()</code> line in <code>triggerHook.html</code>.</p>
<p>This approach keeps your setup lightweight and avoids CORS hurdles by sending data via form encoding.</p>
<p><strong>Happy automating!</strong> Feel free to open an issue or submit a pull request if you have questions or improvements.</p>
```
