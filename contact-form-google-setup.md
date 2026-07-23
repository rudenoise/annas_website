# Contact form → Google Apps Script (one-time setup)

This replaces formsubmit.co with a small script running inside the practice's
own Google Workspace, so enquiries go straight from the website into
`anna@thehughespractice.co.uk` with **no third party in the middle**. It's free
and covered by the Google Workspace terms the practice already has.

The setup takes about five minutes in a browser. Do it **signed in as
`anna@thehughespractice.co.uk`**.

## Steps

1. Go to **https://script.google.com** and click **New project**.
2. Delete the placeholder code in the editor and paste in the whole script
   from the block below.
3. Click the project name ("Untitled project") and rename it to
   `Website contact form`.
4. Click **Deploy → New deployment**.
5. Click the gear icon next to "Select type" and choose **Web app**.
6. Set:
   - **Description:** `contact form`
   - **Execute as:** `Me (anna@thehughespractice.co.uk)`
   - **Who has access:** `Anyone` — this is what lets website visitors
     (who aren't signed in to Google) submit the form. The script only
     sends email to Anna; it can't be used to read anything.
7. Click **Deploy**, then **Authorise access** and approve the permissions
   (it asks because the script sends email as Anna).
8. Copy the **Web app URL** it shows (it ends in `/exec`) and send it back
   in Claude Code — the website form will then be switched over to it.

## The script

```javascript
const DEST = 'anna@thehughespractice.co.uk';

function doPost(e) {
  try {
    const p = (e && e.parameter) || {};
    // Honeypot: hidden field on the site's form that real visitors never
    // fill in. A value here means a spam bot — silently drop it.
    if (p._honey) return ContentService.createTextOutput('ok');

    const name = String(p.name || '').slice(0, 200);
    const email = String(p.email || '').slice(0, 200);
    const phone = String(p.phone || '').slice(0, 100);
    const message = String(p.message || '').slice(0, 5000);
    if (!name && !email && !message) {
      return ContentService.createTextOutput('ok');
    }

    const mail = {
      to: DEST,
      subject: 'New enquiry from your website',
      body:
        'Name: ' + name + '\n' +
        'Email: ' + email + '\n' +
        'Phone: ' + (phone || '—') + '\n\n' +
        message + '\n',
    };
    // Reply straight to the enquirer from the inbox.
    if (email) mail.replyTo = email;
    MailApp.sendEmail(mail);
  } catch (err) {
    // Never show an error to the visitor; worst case the message is lost
    // and they use the email link instead.
  }
  return ContentService.createTextOutput('ok');
}
```

## Afterwards

- Submit a **test enquiry** from the live site and check it arrives in
  Anna's inbox (check spam the first time).
- The old formsubmit.co address can then be forgotten — no account existed,
  so there is nothing to close.
- If the script is ever changed, use **Deploy → Manage deployments → Edit →
  New version** — creating a brand-new deployment would change the URL and
  break the form.
