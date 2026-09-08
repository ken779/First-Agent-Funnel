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
CNAME          the custom domain, read by GitHub Pages
```

## Deploying

GitHub Pages, from the repository root:

1. **Settings → Pages → Source:** Deploy from a branch, `main`, `/ (root)`
2. **Settings → Pages → Custom domain:** the domain, which writes `CNAME`
3. At the registrar, point the apex at GitHub Pages:
   - `A` records to `185.199.108.153`, `.109.153`, `.110.153`, `.111.153`
   - or `ALIAS`/`ANAME` to `<user>.github.io` if the registrar supports it
4. Tick **Enforce HTTPS** once the certificate is issued. Google will not accept a policy
   URL that does not load over HTTPS.

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

`site` is the remote pointing at this repository. The first push takes a moment while it
rewrites the history of that directory; later ones are quick.
