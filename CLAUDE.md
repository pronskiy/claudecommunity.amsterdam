# Development notes

A static site: plain HTML and CSS in `public/`, no build step and no dependencies. Cloudflare serves those files as a Worker with static assets, configured in `wrangler.jsonc`. Files in the repository root are not published.

## Local preview

```sh
npx wrangler dev
```

This serves the site the way Cloudflare does, including the short `/thanks`-style URLs. `python3 -m http.server 8000 -d public` also works, but there you need the `.html` URLs.

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

## Mailcoach list settings

In the list settings, open the **Onboarding** tab:

- Turn on **Allow POST from an external form**.
- Keep **double opt-in** on. It's the main protection against bots, because anyone can POST to the subscribe URL directly.
- If the list settings have a landing URL for after subscribing, set it to `https://claudecommunity.amsterdam/thanks`. Subscribers who click the confirmation link then land on this site instead of Mailcoach's default page.

The form also has a hidden honeypot field. A small script blocks the submit when that field is filled in, which catches simple form-filling bots.
