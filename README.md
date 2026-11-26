# Northstar Outdoors — Cookiebot / Usercentrics CMP POC

A self-contained, multi-view sample website for testing consent management locally. It uses only deterministic dummy integrations prefixed with `ns_`; it does not load or call real analytics, advertising, or personalization vendors.

## Run locally

Serve this folder with any static server (for example, VS Code Live Server, `python3 -m http.server 8080`, or `npx serve .`) and open the resulting URL. Hash routes provide the views: `#home`, `#journal`, `#account`, `#inventory`, and `#qa`.

## Configure Cookiebot

1. Create a Cookiebot Domain Group and copy its Domain Group ID.
2. In `index.html`, replace `YOUR_DOMAIN_GROUP_ID` in the commented Cookiebot script tag.
3. Uncomment the script and keep `data-blockingmode="auto"` for the POC.
4. Serve the site from a stable hostname and add that hostname in Cookiebot Manager.
5. Run a Cookiebot scan. Classify each `ns_*` cookie using `Cookie inventory` and confirm the scan report matches names, purpose, retention, and category.
6. If using Usercentrics instead, replace the commented loader with the Usercentrics script for your settings ID and map the four categories to Necessary, Preferences, Statistics, and Marketing.

The built-in banner is a local fallback so the site is testable before a real CMP ID is configured. Once Cookiebot is enabled, use its banner/preference center as the source of truth and remove or disable the fallback if desired.

## What the dummy integrations do

`app.js` simulates first-party and third-party-style behavior using same-origin cookies only. Necessary state is always written. Preferences controls `ns_lang` and `ns_ab_variant`; Statistics controls `ns_analytics_id` and `ns_perf_sample`; Marketing controls `ns_campaign_id` and `ns_retargeting_id`. Optional keys are deleted when a category is rejected or withdrawn. No network request is sent by these integrations.

## QA checklist

Use a fresh private window with DevTools → Application/Storage → Cookies and Local Storage plus Network open.

- Accept all: verify all optional cookie groups appear, reload, and confirm persistence.
- Reject optional: verify only `ns_session_id` and `ns_consent_state` remain; no optional event/integration activity is present.
- Granular consent: enable one category at a time and verify only its documented keys appear.
- Withdraw/change consent: use Privacy → Withdraw consent or Account → Change choices; verify optional keys are removed immediately.
- Reload/persistence: revisit every route and confirm the banner/choices behave consistently.
- Browser storage inspection: compare cookies and `northstar_cmp_consent_v1` with the inventory table.
- Cookiebot verification: configure the ID, scan the hostname, and reconcile Cookiebot’s detected list with the inventory before testing auto-blocking.
