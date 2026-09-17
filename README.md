# Claude Community Amsterdam

A static landing page with a mailing list signup form. It's hosted on Cloudflare Workers (static assets only), and sign-ups go straight to a [Mailcoach](https://www.mailcoach.app) email list. There's no server code and no build step.

Everything that gets published lives in `public/`. The files in the repository root (this README, `DESIGN.md`) are not deployed.

## How it works

The form in `public/index.html` sends a plain HTML POST to Mailcoach. Mailcoach adds the subscriber and redirects the browser to one of these pages:

| Page | When |
|---|---|
| `/confirm` | Double opt-in is on and the confirmation email has been sent |
| `/thanks` | Subscribed right away (the form only redirects here when double opt-in is off) |
| `/already-subscribed` | The email address is already on the list |

Cloudflare serves `confirm.html` at `/confirm` and redirects `/confirm.html` there, so the form uses the short URLs.

The Mailcoach subscribe URL and the three return URLs are set in `public/index.html`.

## Mailcoach list settings

In your list settings, open the **Onboarding** tab:

- Turn on **Allow POST from an external form**.
- Keep **double opt-in** on. It's the main protection against bots, because anyone can POST to the subscribe URL directly.
- If the list settings have a landing URL for after subscribing, set it to `https://claudecommunity.amsterdam/thanks`. Subscribers who click the confirmation link then land on this site instead of Mailcoach's default page.

The form also has a hidden honeypot field. A small script blocks the submit when that field is filled in, which catches simple form-filling bots.

## Deploy on Cloudflare

The Worker is configured in `wrangler.jsonc`: it serves the files in `public/`, has no server code, and is attached to `claudecommunity.amsterdam`.

### Automatic deploys from GitHub

The Worker `claudecommunity-amsterdam` is connected to this repository in the Cloudflare dashboard (**Workers & Pages → claudecommunity-amsterdam → Settings → Build**). Every push to `main` runs `npx wrangler deploy` and publishes the site. Other branches get preview URLs.

### Manual deploy

```sh
npx wrangler deploy
```

### Custom domain

The domain's DNS is managed by Cloudflare. `wrangler deploy` attaches `claudecommunity.amsterdam` to the Worker and creates the DNS record itself. That only works when the domain has no other `A`, `AAAA` or `CNAME` record, so delete any old ones in **DNS → Records** first.

## Local preview

```sh
npx wrangler dev
```

This serves the site the way Cloudflare does, including the short `/thanks`-style URLs. `python3 -m http.server 8000 -d public` also works, but there you need the `.html` URLs.

## Design

The page follows Gemeente Amsterdam's straightforward web style: white surfaces,
red branding, bold headings, blue actions, and rectangular form controls.
It uses the official open-source Amsterdam Design System form CSS with local
styling and the system font Arial. The design is fixed and has no generated
artwork, custom-font downloads, animation, or randomness.

See [DESIGN.md](DESIGN.md) for references and the component license.

## License

The site's own code is MIT licensed, see [LICENSE](LICENSE). Two things in this repository are not covered by it:

- `public/assets/vendor/amsterdam/` contains form CSS from the Amsterdam Design System, licensed under the EUPL-1.2. Its own licence sits next to it.
- The event photos in `public/assets/images/` belong to the photographers who took them.
