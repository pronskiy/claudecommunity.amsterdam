# Design

Reference: [Gemeente Amsterdam](https://www.amsterdam.nl/) and the
[Amsterdam Design System](https://designsystem.amsterdam/).

A simple community signup page with a red community wordmark, white background,
black headings, blue actions, and rectangular form controls. The reading order,
copy, Mailcoach endpoint, field names, honeypot, and return pages are preserved.

The design uses the system font Arial, which is the Amsterdam Design System's
fallback, and keeps the same light appearance as the reference website.

`public/assets/vendor/amsterdam/forms.css` contains the official label, text-input, and
button components from `@amsterdam/design-system-css` 4.4.0 (EUPL-1.2).
`styles.css` supplies local component tokens and page layout. Vendor provenance
and the complete license are included alongside the CSS. The proprietary
Amsterdam Sans font, municipal logo, and token package are not bundled.

The community wordmark continues to name Claude Community Amsterdam. It does
not identify this page as a municipal service.
