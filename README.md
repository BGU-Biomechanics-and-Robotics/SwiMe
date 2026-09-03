# SwiMe — static site mirror

A static (HTML/CSS/JS) mirror of the live WordPress + Elementor site
`https://www.swimesys.duckdns.org/`, generated for hosting on GitHub Pages.

## What this is

The original site is a single-page Elementor design (hero, "How it Works",
"About", team, "Contact", FAQ, footer) with two WordPress default pages
(a placeholder "Hello world!" post and its category/author archives) that
carry no real content and were intentionally left out of this mirror.

This copy was produced by:
1. Downloading the rendered homepage HTML.
2. Downloading every CSS/JS/image/video/font asset it references (including
   ones only requested at runtime by Elementor's own JS module loader, and
   two background-video URLs that had a pre-existing missing-slash bug on
   the live site, fixed here).
3. Rewriting every reference to a relative, same-origin path so the page
   works from a GitHub Pages project URL (`https://USER.github.io/REPO/`)
   without assuming it's served from a domain root.
4. Stripping WordPress-only plumbing (REST/oEmbed discovery links, RSD,
   shortlink, generator tag) that can't function without a live backend.
5. Disabling the Elementor contact form's submit cleanly (see below) since
   there is no backend to receive it.

## What doesn't work here (by nature of being static)

- **Contact form** — visually intact, but submission is intercepted with a
  small inline script and shows a "this is a static copy" message instead
  of posting anywhere. No destination email was published anywhere on the
  original page to wire up a `mailto:` fallback to; if you want the form to
  actually deliver messages, point it at an email address or a form
  service (e.g. Formspree) and I can wire that in.
- **Login** (`/accounts/login/` on `swime.duckdns.org`) — external app link,
  kept as-is; unaffected by this being a static mirror.
- **WordPress search, comments, admin, REST API** — not present in a static
  export; their discovery `<link>` tags were removed rather than left
  pointing at nothing.

## Regenerating this mirror

The scripts used to produce this from the live site (`mirror.py`,
`rewrite_html.py`, `validate.py`) live outside this repo, alongside the
downloaded `home.html` seed, in the project this was generated from.
