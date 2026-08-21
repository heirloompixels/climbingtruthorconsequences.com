# climbingtruthorconsequences.com

Zola site for Climbing Truth or Consequences — climbing in and around
Truth or Consequences, New Mexico. Scaffolded 2026-08-21.

Right now it is a single coming-soon landing page and nothing else: one
section (`content/_index.md`), whose front matter holds every string on
the page, and one stylesheet. There is no nav, no blog, no search index
and **no JavaScript** — when the real content arrives, add templates
beside `index.html` rather than reaching for a theme.

## Local development

```sh
zola serve
```

## Build

```sh
zola build
```

## Deployment

Like `torc.art`, `kyleparkercunningham.com` and `larrypogreba.com`, and
unlike `jeannieortiz.com`, this site does **not** deploy to Cloudflare.
Cloudflare is the registrar and the DNS, and nothing more — no proxy, no
Worker, no Pages project.

- Source repo: `git@github.com:heirloompixels/climbingtruthorconsequences.com.git`
- GitHub Actions builds every push to `main` (`shalzz/zola-deploy-action@v0.22.1`,
  pinned to the Zola this site is developed against) using the automatic
  per-run `GITHUB_TOKEN`
- Published branch: `gh-pages`, served by GitHub Pages
- `static/CNAME` holds the apex, so GitHub redirects www to it

### DNS

Configured in the TorC Cloudflare account (`0170b714b0ee025091fed8a088f7a65a`),
zone `1c5cf56c40613ad348ed16e2743667e7`, on 2026-08-21. The zone was
empty before this.

| Record | Name | Value | Proxy |
|---|---|---|---|
| A ×4 | `@` | 185.199.108–111.153 | DNS only |
| AAAA ×4 | `@` | `2606:50c0:8000–8003::153` | DNS only |
| CNAME | `www` | `heirloompixels.github.io` | DNS only |

**Every record is deliberately grey-clouded.** GitHub Pages provisions
its Let's Encrypt certificate over an HTTP challenge to these hosts; a
proxied record answers that challenge itself and the certificate never
issues. Do not turn on the orange cloud — not before the certificate
exists, and not after without moving to Full (strict).

The apex uses A and AAAA records rather than a flattened CNAME because
that is the arrangement GitHub documents and checks for.

**Provision the certificate deliberately.** `new.torc.art` has gone
without one since it launched, because nobody opened the repo's Pages
settings and waited for it. After the first build lands, set the custom
domain in Settings → Pages, wait for the certificate, then tick *Enforce
HTTPS*. `base_url` in `config.toml` already claims `https://`, and Zola
builds absolute URLs from it, so the site is not finished until that
tick is real.

## Project structure

- `content/_index.md`: every string on the landing page, in front matter
- `templates/`: `base.html` (document shell and metadata), `index.html`
  (the landing section)
- `static/`: `style.css` and `CNAME`, copied verbatim
- `config.toml`: Zola site configuration
