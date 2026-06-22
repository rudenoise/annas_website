# Getting your website online — a step-by-step guide

This is written for someone doing this for the first time. No coding needed.
Work through it in order. The whole thing usually takes an afternoon, and
the only real cost is the web address (around £10 a year).

---

## First, a note about your files

Your website is made of **13 files**. They all have to stay together in one
folder, and they all get uploaded together. Here is what each one is:

| File | What it is |
|------|------------|
| `anna-hughes-therapy.html` | The page itself (the words and layout) |
| `hero.svg` | The two-chairs illustration |
| `icon-heard.svg` | "Feel heard" ear icon |
| `icon-safe.svg` | "Safe space" house icon |
| `icon-confidence.svg` | "Confidence" figure icon |
| `icon-understand.svg` | "Understand yourself" mirror icon |
| `icon-calm.svg` | "Manage stress" lotus icon |
| `icon-change.svg` | "Life changes" mountains icon |
| `icon-relationships.svg` | "Relationships" circles icon |
| `icon-trust.svg` | "Trust yourself" shield icon |
| `favicon.svg` | The little leaf icon in the browser tab |
| `apple-touch-icon.png` | The icon if someone saves the site to their phone |
| `og-image.png` | The preview picture shown when the link is shared |

**Keep a backup copy of this whole folder before you start editing anything.**

---

## Step 1 — Put in your real details

Open `anna-hughes-therapy.html` in a plain text editor (TextEdit on Mac,
Notepad on Windows — both are free and already installed).

At the very top you'll see a short **HOW TO EDIT** note. The golden rule:
**change the words, never touch anything inside the angle brackets `< >`.**

Use Find (Ctrl+F / Cmd+F) to jump to each item marked **`EDIT (REQUIRED)`**
and replace the placeholder:

- **Your email address** — search for `hello@annahughestherapy.co.uk` and
  change every copy to your real address (it appears a few times).
- **Your fees** — search for `£XX` and put your real price.
- **About / bio** — rewrite the "I'm Anna" paragraph in your own words.
- **Your photo** — see the note below.
- **Testimonials** — either paste in real, consented client quotes, or delete
  that whole section (it's clearly marked).
- **BACP details** — confirm the membership wording, or change it if you're
  registered with a different body (e.g. NCPS, UKCP).
- **Instagram** — search for `your-handle` and put your real Instagram name.

Save the file when you're done (keep it as a `.html` file).

### Adding your photo
The About section currently has a coloured placeholder box. To use a real
photo you'll need a little help swapping an image block into the page — send
the photo over and I'll do that part for you, as it's the one edit that's
fiddly by hand.

---

## Step 2 — Get your web address (domain)

This is the address people type, e.g. `annahughestherapy.co.uk`. You rent it
yearly from a "domain registrar".

1. Go to a registrar — well-known ones include **Cloudflare Registrar**
   (sells at cost, cheapest), **Namecheap**, or **123-reg** (UK-based).
2. Search for the name you want. A `.co.uk` is usually **£8–12 per year**.
3. Buy it. You now own that name. (You don't need to buy any hosting or
   email add-ons they try to upsell — just the domain.)

Tip: if your ideal name is taken, try `.co.uk`, `.com`, or adding your area
(e.g. `annahughesbristol.co.uk`).

---

## Step 3 — Put the site online (hosting)

"Hosting" is the computer that shows your page to visitors. Because your site
is simple, you can host it **free**. The easiest beginner option is **Netlify
Drop** — you literally drag your folder onto a web page.

1. **Rename** `anna-hughes-therapy.html` to **`index.html`**. (Hosts
   automatically show the file called `index.html` as the homepage. Renaming
   it is safe — nothing else needs changing.)
2. Go to **https://app.netlify.com/drop** in your browser.
3. Drag your whole folder (all 13 files) onto the page.
4. Wait a few seconds. It gives you a live link like
   `random-name.netlify.app`. Your site is online.
5. Create a free account when prompted, so the site stays up and you can
   come back to it.

Other good free options if you'd rather: **Cloudflare Pages** (very fast) or
**GitHub Pages** (free, but needs a GitHub account and is a bit more technical).

---

## Step 4 — Connect your domain

Right now the site is at a `.netlify.app` address. To use your own:

1. In Netlify, open your site → **Domain settings** → **Add a custom domain**.
2. Type the domain you bought and follow the on-screen steps. It will tell you
   to add a couple of settings ("DNS records") at your registrar — copy them
   across exactly.
3. It can take a few hours (sometimes up to a day) for the address to start
   working. Netlify switches on the padlock (HTTPS) automatically.

---

## Step 5 — One last edit, then re-upload

Now that you know your real web address, update three placeholders in
`index.html`. Search for `https://www.annahughestherapy.co.uk/` and replace it
with your real address everywhere it appears (these make Google and link
previews work correctly).

Save, then re-upload: in Netlify, open your site and drag the updated folder
onto the **Deploys** area. It replaces the old version. **This is also how you
make any future change** — edit the file, drag the folder back on.

---

## Step 6 — Test it

- Open it on a **phone** and a **computer**.
- Click **every** link and button.
- Tap the **email button** and check it opens a new email to the right address.
- Check the **Instagram** link goes to your account.
- Paste your link into WhatsApp or iMessage to yourself — the preview picture
  and title should appear.

---

## Rough costs

| Item | Cost |
|------|------|
| Domain name | ~£10 / year |
| Hosting (Netlify free tier) | Free |
| **Total** | **~£10 / year** |

If you'd like a matching email address (e.g. `hello@yourdomain.co.uk`),
that's a separate paid add-on (often a few pounds a month) — optional, and we
can sort it later.

---

## Making changes after launch

1. Open `index.html`, change the words between the tags, save.
2. To change an illustration, replace its file (e.g. `hero.svg`) with a new
   one of the same name.
3. Drag the folder back onto Netlify. Done.

Always keep a backup of the folder before editing.

---

*Stuck on any step? Bring me the bit you're on and I'll talk you through it.*
