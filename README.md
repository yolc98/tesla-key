# tesla.bromptonai.com

Static site for [GitHub Pages](https://docs.github.com/en/pages), served at the custom domain `https://tesla.bromptonai.com`.

It exists only so Tesla's Fleet API can verify the owner's domain for a personal, read-only app that reads their home Powerwall data. Tesla requires the app's public key at:

`https://tesla.bromptonai.com/.well-known/appspecific/com.tesla.3p.public-key.pem`

## What is in this repo

| Path | Purpose |
| --- | --- |
| `.well-known/appspecific/com.tesla.3p.public-key.pem` | Public key, published byte-for-byte at the path Tesla fetches |
| `index.html` | Short note that this domain is for that personal Fleet API app |
| `callback/index.html` | OAuth redirect page at `/callback/`. It displays `code`, `state`, and `error` (plus `error_description` when present) from the page URL so they can be copied. The page does not send those values anywhere. |
| `CNAME` | Custom domain `tesla.bromptonai.com` |
| `.nojekyll` | Turns off Jekyll so the dot-directory is published |

The private key is not in this repository and must stay out of it. Do not add secrets or tracking scripts.

## Why `.nojekyll` is required

GitHub Pages runs Jekyll on a branch deployment by default. Jekyll does not publish files or folders whose names start with a dot, so `.well-known` would be missing from the live site. An empty `.nojekyll` file in the repository root disables Jekyll. Pages then copies the branch as static files, including `.well-known`, without rewriting the public key. That is the right fix here because Tesla checks those exact bytes.

## Remaining manual steps

GitHub Pages is not fully turned on by this repository alone.

1. In the repo, open **Settings > Pages**.
2. Set **Source** to **Deploy from a branch**.
3. Choose branch **main** and folder **/ (root)**.
4. Set the custom domain to `tesla.bromptonai.com` and enable **Enforce HTTPS**.

In GoDaddy DNS for `bromptonai.com`, add a CNAME record with host `tesla` pointing to `yolc98.github.io`.

After DNS propagates and Pages finishes deploying `main`, these URLs should respond:

- `https://tesla.bromptonai.com/`
- `https://tesla.bromptonai.com/.well-known/appspecific/com.tesla.3p.public-key.pem`
- `https://tesla.bromptonai.com/callback/`
