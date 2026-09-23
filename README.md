# Scout — on-site attendance

Two pieces:

| File | Where it goes | What it does |
| --- | --- | --- |
| `index.html` | GitHub, this repo | The public form attendees open. One file, no logos folder, nothing to edit per event. |
| `apps-script.gs` | Google Apps Script | Receives confirmations, writes them to a spreadsheet, and serves school logos out of your Drive. |

Everything else happens in the admin portal.

---

## 1. Put the script on Google (once)

1. Go to **script.google.com** → **New project**.
2. Select everything in the code box and delete it.
3. Paste in all of `apps-script.gs`.
4. Rename the project **Scout attendance** (click "Untitled project" at the top).
5. **Deploy** → **New deployment** → the gear icon → **Web app**.
6. Set:
   - **Execute as:** Me
   - **Who has access:** **Anyone**
7. **Deploy**. Google asks you to authorise it — **Review permissions** → pick your
   account → **Advanced** → **Go to Scout attendance (unsafe)** → **Allow**. That
   warning is normal for your own script.
8. Copy the **Web app URL**. It ends in `/exec`.

Keep that URL. That is the only thing you need from this step.

> Changing the script later: **Deploy → Manage deployments → pencil → Version:
> New version → Deploy.** The URL stays the same.

## 2. Tell the admin portal about it (once)

In the Scout Attendance portal: **Settings**

- **Public site URL** → `https://jeffkoeneman-gif.github.io/scout-events/`
- **Attendance service URL** → the `/exec` URL you just copied

**Save settings.**

Every link and QR code made from then on carries that address inside it. The
live site never needs editing again — not for a new event, not for a new school.

## 3. Put the page on GitHub (once)

Upload `index.html` to the root of this repo, then **Settings → Pages →
Deploy from a branch → `main` / `(root)` → Save**. A minute later the site is live.

Opening the bare address shows "This link isn't valid" — correct. The page only
renders when a link gives it an event.

---

## School logos

They live in your Drive folder **Scout School Logos**
(`drive.google.com/drive/folders/1uu-I1pw9bC0Hiu2B7xYlR8tuAEyxYIMj`). The script
reads them from there and hands the image to the public page. Nothing in Drive
is shared, and no logo lives on GitHub.

Two ways in, both fine:

- Drop a file in the folder, named after the school — `Purdue.png`.
- Upload it in the portal, **Schools** tab. It saves into the same folder.

A trailing "University" or "College" is ignored on either side, so
`Loyola Marymount.png` answers for "Loyola Marymount University". A school with
no file shows its name in type instead.

If you ever move the folder, put its new id in `LOGO_FOLDER_ID` at the top of
the script.

## Where confirmations go

Into a spreadsheet the script creates on its first confirmation, called
**Scout — On-Site Attendance**, in your Drive. One tab per event, plus an
"All confirmations" tab. The first six columns are the HubSpot meeting mapping,
in order.

To find it: open the `/exec` URL in a browser. It answers with the spreadsheet
address.

The very first confirmation is slow — Google is creating the spreadsheet while
someone waits on the button. To get that out of the way, open the script editor,
pick **setup** from the function dropdown and press **Run** once.

A person who confirms twice for the same event gets one row, not two. Same
email, same event code, so a retry after a slow connection is safe.

**After any edit to the script, redeploy:** Deploy → Manage deployments →
pencil → Version: **New version** → Deploy. Without that step the live site
still runs the old code.

## Editing the page

Plain HTML, CSS and JavaScript in one file. Edit it on GitHub directly
(Code → `index.html` → pencil) and Pages redeploys within a minute. The three
lists near the top of the `<script>` — `SPORTS`, `YEARS`, `MEETING_TYPES` — are
the form's dropdowns, if they ever need to change.
