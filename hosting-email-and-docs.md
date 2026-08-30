# Hosting, email and documents — setup guide

For **thehughespractice.co.uk**. Covers where the website lives, and how to
choose email and document tools that are appropriate for a BACP-registered
therapist handling sensitive client data under UK GDPR.

---

## The one idea that makes this simple

**GDPR follows the *data*, not the website.**

Once the contact form is fixed (see the warning below), the website itself holds
**no personal data** — it's a public brochure. So the website can be hosted
anywhere convenient. The care and attention belong on the two places where
client data actually lives:

| Surface | Holds client data? | How much to worry |
|---|---|---|
| The website (brochure) | No | Low — pick on convenience → **GitHub Pages** |
| Email (enquiries, admin) | **Yes** | **High** |
| Documents / notes | **Yes** (special category) | **Highest** |

A client's name plus "I'm reaching out because I'm struggling with…" is
**special-category health data** under UK GDPR (Article 9), which needs extra
safeguards. That's why email and documents get the scrutiny, and the website
doesn't.

---

## ⚠️ Contact form — partially fixed, one decision left

The form originally posted to a personal consumer Gmail. That's fixed: it now
posts to the professional mailbox:

```
action="https://formsubmit.co/anna@thehughespractice.co.uk"
```

Enquiries land in the practice's own Google Workspace inbox. The remaining
concern is that submissions still pass **through formsubmit.co**, a third-party
relay with no data-processing agreement. Options, in order of simplicity:

- **Simplest:** drop the form and rely on the `mailto:` links to
  `anna@thehughespractice.co.uk` already used elsewhere on the site — no third
  party in the middle.
- **Keep a form:** switch to a handler that signs a DPA (e.g. Formspark,
  Basin), and add a consent checkbox linking to the privacy notice.

Note: formsubmit.co requires one-time activation — the first submission sends a
confirmation email to the inbox, which must be clicked before messages flow.

---

## 1. Website hosting — GitHub Pages (set up, July 2026)

The site is hosted on **GitHub Pages**: free, fast, custom domain, free HTTPS.

### How it works

- The site lives in the GitHub repo **`rudenoise/annas_website`**.
- A workflow (`.github/workflows/deploy.yml`) publishes the repo's files as-is
  on every push to the default branch — changes are live within a minute or so.
  The `drafts/` folder is excluded, so pages kept there stay unpublished.
- Fallback address (always works): `https://rudenoise.github.io/annas_website/`

### Custom domain + HTTPS

The Pages custom domain is set to **`www.thehughespractice.co.uk`** (matching
the canonical URLs in the pages). The bare domain and the `github.io` address
both redirect to it. DNS is managed at **Fasthosts** (the domain registrar):

| Record | Host | Value |
|---|---|---|
| `A` (×4) | `@` (apex) | `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153` |
| `CNAME` | `www` | `rudenoise.github.io` |
| `MX` | `@` | `smtp.google.com` — **email, leave untouched** |

GitHub provisions a free **Let's Encrypt** certificate automatically once DNS
propagates; after that, "Enforce HTTPS" should be ticked in the repo's
**Settings → Pages**.

### Note on data residency

GitHub Pages is not UK-hosted, but that's fine: the website carries no personal
data. Keep client data off the website (no forms posting sensitive info) and the
host's location doesn't matter for GDPR.

---

## 2. Email — the important one

Whatever provider is chosen, three things are non-negotiable for a therapist:

- **A signed Data Processing Agreement (DPA)** — required under UK GDPR
  Article 28. This rules out **free/consumer Gmail**, which offers no DPA.
- **A valid basis for any data leaving the UK/EU** — either the provider is
  covered by the **UK–US Data Bridge** (in force since Oct 2023), or it stores
  data in the UK/EU so the question doesn't arise.
- **Multi-factor authentication (MFA)** on every account.

### The options

**Familiar route — Google Workspace (paid)** `~£5–6 / user / month` Offers a
DPA, is Data-Bridge certified, and on Business Standard and above you can pin
the **data region to Europe**. It's the paid Workspace, *not* free Gmail. Lowest
friction if the practice already lives in Gmail.

**UK data-residency route — Microsoft 365 Business** `~£5 / user / month`
Microsoft runs UK datacentres (London / Cardiff) and, for a UK tenant, stores
Exchange (email) and OneDrive/SharePoint (files) **at rest in the UK**. Offers a
DPA. Strongest "everything in the UK" story, and it doubles as the documents
tool (see section 3).

**Privacy-forward route — Proton Mail for Business** `~£6 / user / month` Swiss
(Switzerland has UK "adequacy"), end-to-end encrypted, DPA, custom domain. The
strongest privacy posture; slightly more friction because of the end-to-end
encryption.

**Budget EU route — mailbox.org or Posteo** `~€3 / month` German, EU-hosted,
GDPR-native, custom domain + DPA. Excellent data-residency for the price; less
polished than the big two.

**Avoid:** consumer Gmail (no DPA) for anything client-related, and Fastmail for
this use (US servers, weaker adequacy story).

### Recommendation

- Want it simple and in the UK: **Microsoft 365 Business** — covers email
  *and* documents in one UK-resident subscription.
- Prefer to stay in the Gmail world: **Google Workspace (paid), data region
  set to Europe**.
- Want the strongest privacy narrative: **Proton for Business**.

---

## 3. Documents / office suite

The document tool matters because session-related notes are the most sensitive
data of all. Two decisions:

### a) The general office suite (letters, admin, spreadsheets)

- **Microsoft 365** — Word/Excel/etc. with UK data residency for a UK tenant.
  If M365 is chosen for email, documents come in the same subscription. This is
  the tidiest single answer.
- **Google Workspace** — Docs/Sheets, data region pinned to Europe.
- **Self-hosted (advanced)** — Nextcloud + Collabora/OnlyOffice on a UK VPS
  (e.g. Mythic Beasts, Krystal) gives docs, files, calendar and contacts, all
  UK and fully controlled. More power, but you own the maintenance, backups and
  security. Only worth it if there's appetite to run infrastructure.

### b) Clinical records — keep them OUT of email and general docs

The single most important boundary in this whole guide:

> **Email is for admin and booking. Session notes and client records belong in
> dedicated, UK-hosted practice-management software** — not in email, not in a
> Google Sheet.

Software built for UK therapists handles this properly (encryption, access
control, retention, its own DPA), for example **WriteUpp** (UK-based, popular
with BACP therapists), Cliniko, or Halaxy. This keeps special-category clinical
data in a purpose-built, auditable place and off the general office tools.

---

## 4. Compliance checklist

A short list of the practice's obligations as data controller:

- [ ] **Register with the ICO** and pay the data-protection fee (~£40–60/year)
      — legally required for most controllers.
- [ ] **Sign the DPA** with the chosen email / document / practice-management
      providers.
- [ ] **Turn on MFA** for every account.
- [ ] **Fix the contact form** so client enquiries land only in a controlled,
      DPA-covered mailbox.
- [ ] **Update the privacy notice** (`privacy.html`) to name the actual
      processors used (host, email provider, any form handler,
      practice-management software) and the lawful basis for processing.
- [ ] **Consider a short DPIA** (data protection impact assessment) given the
      special-category data — BACP's *Good Practice in Action* resources have
      templates.
- [ ] **Never email session content** — use the practice-management portal, or
      password-protected/encrypted files if a document must be sent.

---

## Suggested setup, end to end

For the least fuss with a solid compliance position:

1. **Website** → GitHub Pages, custom domain, free HTTPS — ✅ done (§1).
2. **Email** → **Google Workspace** on `anna@thehughespractice.co.uk` — ✅ in
   place (the domain's mail records point at Google). Confirm the data region
   is set to Europe and MFA is on.
3. **Clinical records** → a UK practice-management tool (e.g. WriteUpp).
4. **Contact form** → now delivers to the practice mailbox; decide whether to
   keep formsubmit.co or go `mailto:`-only (see the warning section above).
5. Work through the compliance checklist.

---

*Data-residency terms and DPAs change over time, and this guide reflects the
position as understood at the time of writing. Before committing, confirm the
current UK data-residency commitment and DPA directly with each provider (their
"Trust Centre" / "Data residency" pages). This guide is practical guidance, not
legal advice — for a therapy practice it's worth a quick check against current
ICO and BACP guidance.*
