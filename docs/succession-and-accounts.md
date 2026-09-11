# Succession: what the division owns, and what it doesn't

The website's biggest continuity risk is not knowledge. It is that almost
everything below sits inside personal accounts. If the webmaster steps back —
or simply loses access — a document cannot recover any of it.

This file is the inventory and the order of work. Update it as each item moves.

## Progress

Started 2026-09-10.

- [x] Division Google organisation — `southwestbowls`
- [x] GitHub account `southwestbowls` created — **but it is a User account.**
      Create an Organisation from it and add the current webmaster as a second
      owner, so access is per-person rather than a shared password.
- [x] Vercel account created — needs to become a **Team** (Pro) for the same
      reason; Hobby accounts are single-login by design, and their 12-function
      limit has already failed a deploy on this project once.
- [ ] Repo transferred to the GitHub organisation
- [ ] `GITHUB_REPO_OWNER` set in Vercel, publishing token reissued from the org
- [ ] Vercel project moved to the division team — **after the shirt campaign
      closes 2026-09-12**, never renaming the project
- [ ] Sheets and Forms moved into the division Google org
- [ ] Domain registrar for swlawnbowls.org confirmed division-owned

Register the GitHub organisation and the Vercel team against a division
address in the Google org (e.g. webmaster@swlawnbowls.org), not a personal
mailbox — recovery mail, billing and 2FA resets all follow that address.

## Where things live today

| What | Lives in | Owned by | Division-owned? |
|---|---|---|---|
| Website code & history | GitHub `gallogiulia/swlawnbowls-website` | personal | **No** |
| Hosting & deploys | Vercel `giulia-gallos-projects` | personal | **No** |
| Public hostname | `swd-google-calendar.vercel.app` | Vercel project above | **No** |
| Domain `swlawnbowls.org` | registrar / Squarespace | to confirm | **To confirm** |
| Entry & scoring sheets | Google Sheets | to confirm | **To confirm** |
| The four member forms | Google Forms | to confirm | **To confirm** |
| Publisher login accounts | Vercel env var `USERS` | project above | follows Vercel |
| GitHub token the publisher commits with | Vercel env var `GITHUB_PAT` | personal token | **No** |
| Division PayPal | PayPal | division | assumed yes |

## Order of work

Sequence matters. Each step is safe on its own; done out of order they break
the publisher or the live site.

1. **Create the division identities.** A Google account for the division, a
   GitHub organisation, a Vercel team. Add the current webmaster as an admin.
   Nothing moves yet, so nothing can break.

2. **Transfer the GitHub repository to the organisation.** Then set
   `GITHUB_REPO_OWNER` in Vercel to the new organisation. The code reads that
   variable (see `api/results/commit.js`), so no code change is needed.
   GitHub redirects the old path for a while, which hides mistakes — set the
   variable anyway rather than relying on the redirect.

3. **Reissue the publishing token.** `GITHUB_PAT` is currently a personal
   token; it dies with the personal account. Issue one from the organisation
   and replace the Vercel variable.

4. **Move the Vercel project to the division team.** Do this in a quiet week.
   `swd-google-calendar.vercel.app` is hardcoded in 186 places, 83 of them
   inside Squarespace pages that cannot be edited programmatically, so the
   project name must survive the move. Confirm the hostname still resolves
   before considering it done, and never rename the project.

5. **Move the Google Sheets and Forms** into the division account. File IDs
   survive a change of owner, so published `pubhtml` links keep working — but
   check a published sheet afterwards rather than assuming.

6. **Confirm the domain registrar** for swlawnbowls.org is division-owned, and
   that at least two people can reach it.

## What a successor should need

The target is a webmaster who needs **a browser and a password** — nothing else.
`/results-publisher-mobile` already meets that bar for results: it signs in,
loads the current data, takes photos and commits to the site with no local
tooling. Extending the same approach to tournaments and news is what makes the
job handoverable. VS Code and an AI assistant stay the faster path for whoever
wants them, but they must never be the *only* path.

## Environment variables

Set in the Vercel project. Moving accounts means recreating these, so keep the
list current.

    GITHUB_REPO_OWNER   GitHub owner (defaults to gallogiulia)
    GITHUB_REPO_NAME    repository  (defaults to swlawnbowls-website)
    GITHUB_BRANCH       branch      (defaults to main)
    GITHUB_PAT          token the publisher commits with
    SESSION_SECRET      signs publisher login sessions
    USERS               publisher logins
    GCAL_API_KEY        Google Calendar feed
    PAYPAL_*            entry payments
