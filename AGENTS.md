# yakbd/.github: agent instructions

The YAKBD organisation's shared files: `profile/README.md` (the page at https://github.com/yakbd), `CONTRIBUTING.md`
(linked from every repository and fetched as the website page https://yakbd.github.io/contributing) and `icon.png`
(the logo; the website fetches it too). No build, no tests.

## Rules

- `profile/README.md` stays a short front door: one sentence, the platform table, the repository table, links to the
  website. Documentation goes on the website (repository `yakbd/yakbd.github.io`), never here.
- The platform table says only what is released: "today" for a released platform, "in the next release" while it
  is on a branch, "planned" otherwise, never a date.
- Links: release links `releases/latest`; never link or mention private repositories.
- `CONTRIBUTING.md` headings are linked from other repositories (for example `#reporting-a-layout-bug`):
  keep them, or update the links.

## Docs after every release (rule)

When a release of any YAKBD repository adds, removes or changes functionality (a new platform, a dropped format),
update the platform table here and the website in the same release: re-pin `sources.json` in
`yakbd/yakbd.github.io` (this repository too, when `CONTRIBUTING.md` or `icon.png` changed), update its guide pages
and landing-page platform tags (its `AGENTS.md` says how).

## Checks before pushing

```sh
git diff --check
```

Then check every changed link by opening it; the website's `npm run check:external` also covers `CONTRIBUTING.md`
after a re-pin. Licence MIT-0. No `CLAUDE.md`: this file is the single source.
