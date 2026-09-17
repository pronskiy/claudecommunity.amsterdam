# Claude Community Amsterdam

A static landing page with a mailing list signup form. It's hosted on GitHub Pages, and sign-ups go straight to a [Mailcoach](https://www.mailcoach.app) email list. There's no server code and no build step.

## How it works

The form in `index.html` sends a plain HTML POST to Mailcoach. Mailcoach adds the subscriber and redirects the browser to one of these pages:

| Page | When |
|---|---|
| `confirm.html` | Double opt-in is on and the confirmation email has been sent |
| `thanks.html` | Subscribed right away (the form only redirects here when double opt-in is off) |
| `already-subscribed.html` | The email address is already on the list |

## Setup

### 1. Fill in the placeholders

| Placeholder | Where | Example |
|---|---|---|
| `{{MAILCOACH_DOMAIN}}` | `index.html` | `yourteam.mailcoach.app` |
| `{{LIST_UUID}}` | `index.html` | UUID of your Mailcoach email list |
| `{{SITE_DOMAIN}}` | `index.html` (3×), `CNAME` | `meetups.example.com` |

```sh
grep -rn '{{' --include='*.html' --include=CNAME .
```

### 2. Configure the Mailcoach list

In your list settings, open the **Onboarding** tab:

- Turn on **Allow POST from an external form**.
- Keep **double opt-in** on. It's the main protection against bots, because anyone can POST to the subscribe URL directly.
- If the list settings have a landing URL for after subscribing, set it to `https://{{SITE_DOMAIN}}/thanks.html`. Subscribers who click the confirmation link then land on this site instead of Mailcoach's default page.

The form also has a hidden honeypot field. A small script blocks the submit when that field is filled in, which catches simple form-filling bots.

### 3. Publish on GitHub Pages

1. Push this folder to a GitHub repository.
2. Go to **Settings → Pages** and pick **Deploy from a branch**, then choose `main` and `/ (root)`.
3. Point DNS at GitHub Pages:
   - Subdomain (`meetups.example.com`): a `CNAME` record pointing to `<your-username>.github.io`
   - Apex domain (`example.com`): `A` records for `185.199.108.153`, `185.199.109.153`, `185.199.110.153` and `185.199.111.153`
4. Once DNS resolves, turn on **Enforce HTTPS** in the Pages settings.

## Local preview

```sh
python3 -m http.server 8000
```

Then open http://localhost:8000. The form only works once the placeholders are filled in.

## Design

The page follows Gemeente Amsterdam's straightforward web style: white surfaces,
red branding, bold headings, blue actions, and rectangular form controls.
It uses the official open-source Amsterdam Design System form CSS with local
styling and the system font Arial. The design is fixed and has no generated
artwork, custom-font downloads, animation, or randomness.

See [DESIGN.md](DESIGN.md) for references and the component license.
