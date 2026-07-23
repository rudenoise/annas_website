# Unpublished pages

These pages are kept here on purpose but are **not part of the live website**.
The deploy workflow (`.github/workflows/deploy.yml`) removes this `drafts/`
folder before publishing, so nothing in here is reachable on the live site.

- `dealing-with-loss-in-your-twenties.html`
- `therapy-for-ai-and-work-anxiety.html`

## To make one live again

1. Move the file back to the repository root (out of `drafts/`).
2. Re-add its links where you want them to appear (e.g. the "Articles"
   column in the footers of the other pages).

The pages use root-relative assets (`landing.css`, `logo.png`, etc.), so they
only display correctly once moved back to the root.
