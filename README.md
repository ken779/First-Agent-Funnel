# First Agent Studio — public site

Flat HTML. No build, no dependencies, no framework.

That is deliberate. This site exists so the privacy policy and terms are **published on a
real domain**, which Google requires before it will verify the YouTube scopes the product
needs — and a site that deploys from a `git push` with no pipeline is one that cannot be
broken by a pipeline the week the review lands.

```
index.html     the funnel
privacy.html   named scopes, and the Limited Use disclosure Google checks for
terms.html
styles.css     one file, light and dark
logo.svg
```

## Deploying

Netlify, from the repository root. There is nothing to build, so the publish directory is
the root and the build command is empty.

1. **Netlify → Add new project → Import from Git**, this repository, branch `main`
2. **Domain management → Add a domain:** `firstagentstudio.com`, and set
   **`www.firstagentstudio.com` as the primary** — Netlify then redirects the apex to it,
   which is the configuration that actually gets the CDN
3. At Cloudflare, which holds this domain's DNS, two records:
   - `www` CNAME to `firstagentfunnel.netlify.app`
   - `@` CNAME to `apex-loadbalancer.netlify.com`, flattened by Cloudflare automatically
4. Both records **DNS only** — the grey cloud, not the orange one. Proxied through
   Cloudflare, Netlify cannot verify the domain at all, and the half-working state that
   follows presents as certificate errors rather than as a DNS problem.

HTTPS is issued by Netlify once the domain verifies. Google will not accept a policy URL
that does not load over HTTPS, so that certificate is on the critical path, not a nicety.

`app.` is expected to point at the application, which is a separate deployment. The sign-in
links here already assume that.

## Keeping it honest

The privacy policy describes what the application actually does, scope by scope. If the
scopes change, or Google user data starts being sent somewhere it is not sent today, this
file changes in the same commit — a policy that has drifted from the code is worse than no
policy, because it has been read and relied on.

## How this gets here

It lives in the application repository under `site/` and is published to this one as a git
subtree. One source of truth: the privacy policy describes what the app does, so the two
should never be able to disagree.

From the application repository:

```
git subtree push --prefix site site main
```

`site` is the remote pointing at this repository (`ken779/First-Agent-Funnel`). The first push takes a moment while it
rewrites the history of that directory; later ones are quick.
