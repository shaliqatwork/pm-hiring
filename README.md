# Meta Performance Marketing Lead – Recruitment Landing Page

A static, single-page recruitment site that introduces the role and embeds the
**existing** Google Form so candidates can apply without leaving the page.

There is no backend, database, framework or build step. Applications, resume
uploads, the response sheet and the Apps Script scoring workflow all stay in
Google exactly as they are today.

## Files

```
index.html      Page markup (hero, role highlights, application form, footer)
css/styles.css  All styling (design tokens at the top)
js/config.js    GOOGLE_FORM_URL – the only value you need to edit
js/main.js      Loads the form into the iframe, validates the URL, focus handling
netlify.toml    Netlify settings (publish root, no build command, security headers)
```

## Connect your Google Form

1. Open the form in Google Forms and click **Send**.
2. Choose the link tab (chain icon) and copy the link. It should look like
   `https://docs.google.com/forms/d/e/<long-id>/viewform`.
   - Do **not** use the editor URL (`https://docs.google.com/forms/d/<id>/edit`).
   - Do **not** use the shortened `forms.gle/...` link.
3. Open `js/config.js` and replace the placeholder on this line:

   ```js
   window.GOOGLE_FORM_URL = "PASTE_PUBLISHED_GOOGLE_FORM_URL_HERE";
   ```

4. Preview locally (below) and submit a test application, including a resume
   upload, to confirm it lands in the response sheet and is scored as usual.

Until a valid URL is set, the page shows an "Application form unavailable"
message instead of a broken frame. It also refuses editor links and non-Google
URLs. The page never shows its own "submitted" message; the confirmation
candidates see is Google Forms' own.

**Resume upload note:** Google Forms file-upload questions require the
candidate to sign in to a Google account. The page tells candidates this.
If a browser blocks the sign-in inside the frame, they can use the "Open it in
a new tab" link shown under the form.

## Local preview

Any static file server works. From this folder:

```bash
python3 -m http.server 8080
# then open http://localhost:8080
```

or

```bash
npx serve .
```

Opening `index.html` directly from disk also works for a quick look.

## Deploy to Netlify

**Drag and drop:** go to https://app.netlify.com/drop and drop this folder
(or the zip of it). Done.

**From Git:** push this folder to a repository, then in Netlify choose
*Add new site → Import an existing project*, pick the repository and keep the
defaults. `netlify.toml` already sets the publish directory to the root and
no build command, so Netlify's standard workflow deploys it as-is.

After changing `GOOGLE_FORM_URL`, redeploy (drag the folder again, or push).

## Customising

- **Colors / spacing:** edit the variables at the top of `css/styles.css`.
- **Footer contact:** replace `[recruiting contact]` in `index.html`, or delete
  that line.
- **Form height:** `.form-frame__iframe` in `css/styles.css` (2000px on mobile,
  1800px on wider screens). Increase it if your form scrolls inside the frame.
- **Content Security Policy:** `netlify.toml` only allows frames from
  `docs.google.com` / `accounts.google.com` and fonts from Google Fonts. If you
  add other third-party scripts (e.g. analytics), add their domains there.
