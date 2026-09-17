# Claude Community Amsterdam

The website for [claudecommunity.amsterdam](https://claudecommunity.amsterdam): a landing page with a mailing list signup form and links from past events.

It's a static site, hosted on Cloudflare, and sign-ups go straight to a [Mailcoach](https://www.mailcoach.app) email list. There's no server code and no build step.

Everything that gets published lives in `public/`. The files in the repository root are not deployed.

## How it works

The form in `public/index.html` sends a plain HTML POST to Mailcoach. Mailcoach adds the subscriber and redirects the browser to one of these pages:

| Page | When |
|---|---|
| `/confirm` | Double opt-in is on and the confirmation email has been sent |
| `/thanks` | Subscribed right away (the form only redirects here when double opt-in is off) |
| `/already-subscribed` | The email address is already on the list |

Cloudflare serves `confirm.html` at `/confirm` and redirects `/confirm.html` there, so the form uses the short URLs.

The Mailcoach subscribe URL and the three return URLs are set in `public/index.html`.

## Design

The page follows the [Amsterdam Design System](https://designsystem.amsterdam/). See [DESIGN.md](DESIGN.md) for details.

## Development

Local preview, deployment and Mailcoach settings are in [CLAUDE.md](CLAUDE.md).

## License

The site's own code is MIT licensed, see [LICENSE](LICENSE). Two things in this repository are not covered by it:

- `public/assets/vendor/amsterdam/` contains form CSS from the Amsterdam Design System, licensed under the EUPL-1.2. Its own licence sits next to it.
- The event photos in `public/assets/images/` belong to the photographers who took them.
