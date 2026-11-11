# asizor status

Source for <https://status.asizor.com>, built with [Upptime](https://upptime.js.org). GitHub Actions
checks each public endpoint about every 5 minutes (the GitHub cron floor; scheduled runs can lag
under load) and publishes the page with GitHub Pages.

## What is monitored

| Name    | URL                                   | Up when                                                                  |
| ------- | ------------------------------------- | ------------------------------------------------------------------------ |
| API     | `https://api.asizor.com/health/ready` | 200 within 10 s (the API can reach its database and has tax data loaded) |
| Website | `https://asizor.com`                  | 200 within 10 s                                                          |

Only these two are listed for now. The customer dashboard is added at launch. Internal and
staff-only hosts are never listed. This page is the customer-facing view; our internal uptime
alerting is separate and is what pages us. This page reports what the checks see; it makes no availability commitment.

## Incidents

- When a check fails, Upptime opens an issue labelled `status` and the site slug (`api` or
  `website`), assigned to @Lancelittle, and closes it when the check recovers.
- Post updates as comments on that issue; they appear on the page.
- Manual notice (a problem the checks cannot see): open a new issue with the label `status` and
  the slug, write the update in the body, close it when resolved.

## Planned maintenance

Open an issue labelled `maintenance`. Upptime reads the window from an HTML comment in the body
and closes the issue automatically at `end`:

```text
<!--
start: 2026-12-01T02:00:00Z
end: 2026-12-01T03:00:00Z
expectedDown: api
expectedDegraded: website
-->
Short description of the work.
```

Use the slugs (`api`, `website`), comma-separated for several.

## GH_PAT

Upptime needs a token that can push commits that trigger workflows, open issues and update its
own workflow files; the default `GITHUB_TOKEN` cannot. It is stored as the repository secret
`GH_PAT`.

- Fine-grained, resource owner Asizor, only this repository; repository permissions Actions,
  Contents, Issues and Workflows read and write; no organization permissions.
- Expires after one year; a calendar reminder is set two weeks before.
- **An expired token silently stops the checks and the page goes stale.**

Rotate:

1. Create a new token with the settings above.
2. `gh secret set GH_PAT -R Asizor/status` and paste the new token at the prompt.
3. Re-run the "Setup CI" workflow and confirm the "Uptime CI" run after it is green.
4. Revoke the old token.

---

**Upptime** (<https://upptime.js.org>) is the open-source uptime monitor and status page, powered entirely by GitHub Actions, Issues, and Pages, made with 💚 by [Anand Chowdhary](https://anandchowdhary.com). Code: [MIT](./LICENSE).
