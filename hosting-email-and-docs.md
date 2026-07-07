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
| The website (brochure) | No | Low — pick on convenience → **GitLab** |
| Email (enquiries, admin) | **Yes** | **High** |
| Documents / notes | **Yes** (special category) | **Highest** |

A client's name plus "I'm reaching out because I'm struggling with…" is
**special-category health data** under UK GDPR (Article 9), which needs extra
safeguards. That's why email and documents get the scrutiny, and the website
doesn't.

---

## ⚠️ Fix the contact form first

The current site (`index.html`) has a contact form that posts to:

```
action="https://formsubmit.co/meannahughes@gmail.com"
```

This sends a client's name and message through a third-party service
(formsubmit.co) into a **personal consumer Gmail**. Neither has a data agreement
with the practice, and a consumer Gmail account isn't permitted for professional
client data. This is the most urgent item, independent of hosting.

**Fix (simplest):** replace the form with a `mailto:` link to the professional
mailbox (`hello@thehughespractice.co.uk`) — the same links already used
elsewhere on the site. Data then goes straight into a mailbox the practice
controls, with no third party in the middle.

If a form is preferred over a plain email link, route it to the professional
mailbox via a handler that will sign a data-processing agreement (e.g.
Formspark, Basin) — never to a consumer inbox — and add a consent checkbox
linking to the privacy policy.

---

## 1. Website hosting — GitLab Pages

We're using **GitLab Pages**: free, fast, custom domain, free HTTPS, and it
matches the pattern already used for `www.hughesindustries.uk` — push to git,
GitLab publishes.

### How it works

1. Create a project on GitLab and push the site files to it.
2. Add a `.gitlab-ci.yml` at the repo root. For a plain static site the whole
   job is "publish these files as-is":

   ```yaml
   image: alpine:latest

   pages:
     stage: deploy
     script:
       - echo 'Nothing to build — static site'
     artifacts:
       paths:
         - public
     only:
       - main
   ```

   GitLab Pages serves whatever is in the **`public/`** folder. Either move the
   site files into a `public/` directory, or add a build step that copies them
   there. (The `hughesindustries.uk` project already uses this exact pattern.)

3. Every push to the `main` branch redeploys the site automatically.

### Custom domain + HTTPS

In the GitLab project: **Settings → Pages → New Domain**, enter
`thehughespractice.co.uk`. GitLab shows a verification code and the DNS records
to add. At the domain registrar / DNS host, set roughly:

| Record | Host | Value |
|---|---|---|
| `A` | `@` (apex) | GitLab Pages IP (shown in GitLab) |
| `CNAME` | `www` | `<namespace>.gitlab.io` |
| `TXT` | `_gitlab-pages-verification-code` | verification value from GitLab |

GitLab then provisions a free **Let's Encrypt** certificate automatically —
HTTPS turns on within an hour or so.

> The exact IP and record names are shown in the GitLab Pages settings for the
> project — always copy them from there, as they can change.

### Note on data residency

GitLab Pages is not UK-hosted, but that's fine: the website carries no personal
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

1. **Website** → GitLab Pages, custom domain, free HTTPS (this guide, §1).
2. **Email + documents** → **Microsoft 365 Business** (UK data residency, DPA,
   MFA) — one subscription covers both.
3. **Clinical records** → a UK practice-management tool (e.g. WriteUpp).
4. **Fix the contact form** → `mailto:` to `hello@thehughespractice.co.uk`.
5. Work through the compliance checklist.

---

*Data-residency terms and DPAs change over time, and this guide reflects the
position as understood at the time of writing. Before committing, confirm the
current UK data-residency commitment and DPA directly with each provider (their
"Trust Centre" / "Data residency" pages). This guide is practical guidance, not
legal advice — for a therapy practice it's worth a quick check against current
ICO and BACP guidance.*
