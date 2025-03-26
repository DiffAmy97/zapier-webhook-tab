# Zapier Webhook Trigger via GitHub Pages

This repository provides an HTML page hosted on GitHub Pages to trigger a Zapier webhook via URL parameters. It's useful for passing data (like Airtable Record IDs) directly into Zapier without encountering CORS issues.

## Workflow:

1. **Deploy via GitHub Pages** from the main branch.
2. **Use Airtable** to construct a URL dynamically, passing parameters securely via query strings.
3. Data sent is form-encoded, avoiding browser CORS restrictions.
4. Use Zapier's "Catch Raw Hook" to receive the data.

Always pass webhook URLs dynamically to keep your automation secure.

For setup help or improvements, create an issue or PR.
