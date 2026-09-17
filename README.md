# Claude Community Amsterdam

A static landing page with a mailing list signup form. It's hosted on Cloudflare Pages, and sign-ups go straight to a [Mailcoach](https://www.mailcoach.app) email list. There's no server code and no build step.

Everything that gets published lives in `public/`. The files in the repository root (this README, `DESIGN.md`) are not deployed.

## How it works

The form in `public/index.html` sends a plain HTML POST to Mailcoach. Mailcoach adds the subscriber and redirects the browser to one of these pages:

| Page | When |
|---|---|
| `/confirm` | Double opt-in is on and the confirmation email has been sent |
| `/thanks` | Subscribed right away (the form only redirects here when double opt-in is off) |
| `/already-subscribed` | The email address is already on the list |

Cloudflare Pages serves `confirm.html` at `/confirm` and redirects `/confirm.html` there, so the form uses the short URLs.

The Mailcoach subscribe URL and the three return URLs are set in `public/index.html`.

## Mailcoach list settings

In your list settings, open the **Onboarding** tab:

- Turn on **Allow POST from an external form**.
- Keep **double opt-in** on. It's the main protection against bots, because anyone can POST to the subscribe URL directly.
- If the list settings have a landing URL for after subscribing, set it to `https://claudecommunity.amsterdam/thanks`. Subscribers who click the confirmation link then land on this site instead of Mailcoach's default page.

The form also has a hidden honeypot field. A small script blocks the submit when that field is filled in, which catches simple form-filling bots.

## Deploy on Cloudflare Pages

### Connected to Git (automatic deploys)

1. Push this repository to GitHub or GitLab.
2. In the Cloudflare dashboard, go to **Workers & Pages → Create → Pages → Connect to Git** and pick the repository.
3. Build settings:
   - Framework preset: **None**
   - Build command: leave empty
   - Build output directory: `public`

Every push to `main` deploys the site. Other branches and pull requests get preview URLs.

### Direct upload (no Git)

```sh
npx wrangler pages deploy public --project-name claude-community-amsterdam
```

A Direct Upload project can't be switched to Git deploys later. You'd have to create a new project.

### Custom domain

`claudecommunity.amsterdam` is a root domain, and Pages only accepts those when Cloudflare runs the domain's DNS:

1. Add the domain to Cloudflare (the free plan is enough) and change the nameservers at your registrar to the ones Cloudflare shows.
2. In the Pages project, open **Custom domains → Set up a domain** and enter `claudecommunity.amsterdam`. Cloudflare issues the HTTPS certificate automatically.

## Local preview

```sh
npx wrangler pages dev public
```

This serves the site the way Cloudflare does, including the short `/thanks`-style URLs. `python3 -m http.server 8000 -d public` also works, but there you need the `.html` URLs.

## Design

The page follows Gemeente Amsterdam's straightforward web style: white surfaces,
red branding, bold headings, blue actions, and rectangular form controls.
It uses the official open-source Amsterdam Design System form CSS with local
styling and the system font Arial. The design is fixed and has no generated
artwork, custom-font downloads, animation, or randomness.

See [DESIGN.md](DESIGN.md) for references and the component license.
