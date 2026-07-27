# Contact form → Google Apps Script (setup & maintenance)

The website contact form sends enquiries through a small script running inside
the practice's own Google Workspace. Enquiries go straight to
`anna@thehughespractice.co.uk` — with **no third party in the middle** — and the
person who enquired gets an automatic "thank you" reply. It's free and covered
by the Google Workspace terms the practice already has.

Do everything below **signed in as `anna@thehughespractice.co.uk`**.

---

## If it's already set up (the usual case): updating the script

The form is already live. To change the script — for example to edit the
auto-reply wording — you **edit the existing deployment** so the web-app URL
stays the same (a brand-new deployment would change the URL and break the form).

1. Go to **https://script.google.com** and open **Website contact form**.
2. Replace the code with the version under **"The script"** below (edit the
   auto-reply wording to taste first).
3. **Deploy → Manage deployments → (pencil / Edit) → Version: New version →
   Deploy.** The URL does not change.
4. Send a test enquiry from the live site and check both inboxes (yours, and
   the address you used as the enquirer — check spam the first time).

---

## First-time setup (only if starting from scratch)

1. Go to **https://script.google.com** → **New project**.
2. Delete the placeholder code and paste in the whole script below.
3. Rename the project (top-left) to **Website contact form**.
4. **Deploy → New deployment** → gear icon → **Web app**.
5. Set:
   - **Execute as:** `Me (anna@thehughespractice.co.uk)`
   - **Who has access:** `Anyone` — this is what lets website visitors (who
     aren't signed in to Google) submit the form.
6. **Deploy**, then **Authorise access** and approve the permissions (it asks
   because the script sends email as Anna).
7. Copy the **Web app URL** (it ends in `/exec`) — it goes in the website form's
   `action`. (Current form already uses this.)

---

## The script

Two emails go out on each submission: (1) a notification to Anna, and (2) an
auto-reply to the person who enquired. Edit the text between the quotes in the
auto-reply to change tone, response time, etc.

```javascript
const DEST = 'anna@thehughespractice.co.uk';
const FROM_NAME = 'Anna Hughes · The Hughes Practice';

function doPost(e) {
  try {
    const p = (e && e.parameter) || {};
    if (p._honey) return ContentService.createTextOutput('ok'); // spam trap

    const name = String(p.name || '').slice(0, 200);
    const email = String(p.email || '').slice(0, 200);
    const phone = String(p.phone || '').slice(0, 100);
    const message = String(p.message || '').slice(0, 5000);
    if (!name && !email && !message) return ContentService.createTextOutput('ok');

    // 1) Notify Anna
    const notify = {
      to: DEST,
      subject: 'New enquiry from your website',
      body: 'Name: ' + name + '\nEmail: ' + email + '\nPhone: ' + (phone || '—') + '\n\n' + message + '\n'
    };
    if (email) notify.replyTo = email;
    MailApp.sendEmail(notify);

    // 2) Auto-reply to the person who enquired
    if (email && /^[^@\s]+@[^@\s]+\.[^@\s]+$/.test(email)) {
      let firstName = (name.split(' ')[0] || 'there').toLowerCase();
      firstName = firstName.charAt(0).toUpperCase() + firstName.slice(1); // "jane"/"JANE" -> "Jane"
      const greeting = 'Hi ' + firstName + ',';

      // Plain-text version (fallback for email clients that don't show HTML)
      const textBody =
        greeting + '\n\n' +
        "Thank you for reaching out. It can take a bit of courage to send that first message, and I'm really glad you did.\n\n" +
        "I've received your enquiry and I'll reply personally within two working days to arrange a time for your free, 20-minute introductory call.\n\n" +
        "There's no pressure or commitment. It's simply a chance to talk and see whether working together feels right.\n\n" +
        "If you're in crisis or need urgent support before I'm able to reply, please don't wait. You can call the Samaritans free, at any time, on 116 123.\n\n" +
        'Warm wishes,\nAnna\n\n\n' +
        'Anna Hughes\nThe Hughes Practice\nTherapy for real life\n\nanna@thehughespractice.co.uk';

      // HTML version (what most people will see — gives the italic tagline)
      const htmlBody =
        '<div style="font-family:Arial,Helvetica,sans-serif;font-size:15px;line-height:1.5;color:#222">' +
        '<p>' + greeting + '</p>' +
        "<p>Thank you for reaching out. It can take a bit of courage to send that first message, and I'm really glad you did.</p>" +
        "<p>I've received your enquiry and I'll reply personally within two working days to arrange a time for your free, 20-minute introductory call.</p>" +
        "<p>There's no pressure or commitment. It's simply a chance to talk and see whether working together feels right.</p>" +
        "<p>If you're in crisis or need urgent support before I'm able to reply, please don't wait. You can call the Samaritans free, at any time, on 116 123.</p>" +
        '<p>Warm wishes,<br>Anna</p>' +
        '<p>Anna Hughes<br>The Hughes Practice<br><em>Therapy for real life</em></p>' +
        '<p>anna@thehughespractice.co.uk</p>' +
        '</div>';

      MailApp.sendEmail({
        to: email,
        name: FROM_NAME,
        replyTo: DEST,
        subject: 'Thank you for your message',
        body: textBody,
        htmlBody: htmlBody
      });
    }
  } catch (err) {
    // Never show an error to the visitor; worst case the message is lost and
    // they use the email link instead.
  }
  return ContentService.createTextOutput('ok');
}
```

---

## Notes

- **Change the wording** any time by editing the auto-reply text and redeploying
  as a **New version** (see top section). The two obvious things to review:
  the **response time** ("within two working days") and whether to add anything
  about **fees or next steps** (currently kept short and warm on purpose).
- **Deliverability:** sending from the Workspace account is reliable, but the
  very first auto-reply to a new recipient can land in their spam once, then
  settles.
- **Volume:** Workspace allows roughly 1,500 emails/day this way — far more than
  needed.
- **Replies:** if the enquirer replies to the auto-response, it comes back to
  Anna's inbox (`replyTo` is set).
