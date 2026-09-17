# PhishTrace 🎣

A single-file, client-side phishing email analyzer. Paste raw email source in, get a weighted risk score and a breakdown of every red flag out — no server, no API calls, no data ever leaves the browser.

**[Live demo](https://phishing-analyzer-kappa.vercel.app/)**

---

## Why

Most phishing-awareness tools either explain phishing in the abstract or require you to trust a third-party service with the suspicious email itself. PhishTrace does neither — it's a static HTML file that runs the same heuristics a SOC analyst walks through manually, entirely in-browser, so it's safe to point at real (redacted) samples and safe to self-host or embed anywhere.

## Features

| Category | Checks |
|---|---|
| **Sender identity** | From vs. Reply-To domain mismatch, typosquatted/lookalike brand domains (Levenshtein distance) |
| **Authentication** | SPF / DKIM / DMARC failures in `Authentication-Results`, suspicious relay TLDs in `Received` |
| **Link inspection** | IP-literal URLs, punycode homographs, URL shorteners, excessive subdomains, brand-impersonating domains |
| **Language patterns** | Urgency/pressure phrasing, credential- and financial-data harvesting requests, generic greetings, alarmist subject lines |
| **Payload risk** | Mentions of high-risk attachment extensions (`.exe`, `.scr`, `.zip`, `.docm`, etc.) |

Each triggered check is scored and shown as an individual finding (severity + point value + explanation), rolled up into a 0–100 risk dial and a plain-language verdict.

## Usage

1. Open `phishing-analyzer.html` in any modern browser — no build step, no dependencies.
2. Copy an email's full source:
   - **Gmail** → ⋮ menu → *Show original*
   - **Outlook** → File → Properties → *Internet headers*, or *View → View Source*
   - **Apple Mail** → View → Message → *All Headers*
3. Paste the full text (headers **and** body) into the input panel.
4. Click **Analyze email**. Use **Load a sample** to see a scored example first.

## How scoring works

Each heuristic contributes points on detection (5–30 depending on severity/confidence), summed and capped at 100:

- **60–100** → High risk, likely phishing
- **30–59** → Medium risk, proceed with caution
- **1–29** → Low risk, minor flags only
- **0** → No indicators triggered

The score is a triage aid, not a verdict — a clean score doesn't guarantee legitimacy, and a high score doesn't guarantee malice. It's designed to surface *why* an email looks off so a human can make the final call faster.

## Tech

Single `.html` file — vanilla JavaScript, no frameworks, no external requests, no build tooling. All parsing (header extraction, URL analysis, edit-distance matching against known brand names) runs synchronously in the page.

## Roadmap ideas

- Header-name whitelist/blacklist configurable via UI
- Export findings as a JSON/PDF report
- Bulk `.eml` file upload and batch scoring
- Configurable brand list for org-specific impersonation targets

## Disclaimer

Educational and triage-support tool only. Not a replacement for a mail security gateway, SEG, or your organization's incident response process. When in doubt, report the email to your security team rather than relying solely on this score.

## License

MIT
