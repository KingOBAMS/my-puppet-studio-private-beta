# My Puppet Studio — Private Beta Portal

This folder is a static website package prepared from the supplied Puppet Studio v5 HTML application and PDF manual.

## Contents
- `index.html` — beta portal / landing page
- `studio.html` — supplied animation application, copied without editing
- `user-manual.pdf` — supplied two-page manual
- `robots.txt` and `_headers` — indexing and basic response-header configuration

## Important security limitation
This package does **not** contain a password gate. Protect the entire deployed hostname with Cloudflare Access before sharing it. Configure the Access application to cover **all paths on the hostname**, not just `/`, so direct requests to `/studio.html` and `/user-manual.pdf` are also gated.

A browser-based HTML app must send its HTML, CSS, JavaScript and embedded assets to each authorised tester's browser. An authorised tester can inspect or save those delivered resources. Access control limits who can reach the app; it does not provide source-code secrecy or DRM.

## Recommended deployment: Cloudflare Pages + Access

1. Create a **private** GitHub repository, e.g. `my-puppet-studio-private-beta`.
2. Upload the contents of this folder to the repository root (not the ZIP file itself).
3. In Cloudflare, open **Workers & Pages → Create application → Pages → Connect to Git** and select the private repository.
4. Use:
   - Production branch: `main`
   - Build command: `exit 0`
   - Build output directory: `/` (repository root)
5. Deploy and note the production `*.pages.dev` hostname.
6. In Cloudflare Zero Trust, create an Access application of type **Self-hosted** for that hostname. Apply an **Allow** policy using the exact email addresses of your 5–10 invited testers. Enable **One-time PIN** as the login method.
7. Ensure the Access application protects the **whole hostname / all paths**. Test the root page, `/studio.html`, and `/user-manual.pdf` in a logged-out/private browser.
8. Before inviting testers, check that Pages preview deployments are disabled or separately protected. Do not share preview URLs.

Cloudflare documentation:
- Pages Git integration: https://developers.cloudflare.com/pages/configuration/git-integration/
- Static HTML deployment: https://developers.cloudflare.com/pages/framework-guides/deploy-anything/
- Access policies: https://developers.cloudflare.com/cloudflare-one/access-controls/policies/
- One-time PIN: https://developers.cloudflare.com/cloudflare-one/integrations/identity-providers/one-time-pin/

## Beta operations
- Add only tester email addresses to the Access Allow policy.
- Remove an email from the policy to revoke future access.
- Keep the GitHub repository private and do not invite testers to it.
- Update `studio.html` and redeploy when releasing a new build.
- Ask testers to use current desktop Chrome or Edge. The supplied manual identifies these as the recommended browsers.
- The feedback form is linked to the supplied Google Form URL. Its own Google Forms sharing/response settings are separate from Cloudflare Access.

## Validation still required after deployment
This package has been assembled locally. The Cloudflare deployment, Access login, browser behaviour and form submission must be tested in the live environment before invitations are sent.
