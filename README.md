# AgenticSE @ AAAI-27

Website for the **AAAI-27 Workshop on Agentic Software Engineering**.
Intended URL: https://agenticse-aaai.github.io/

## Local development

Use Node.js 22 and npm.

```sh
npm ci
npm run dev
```

Alternatively, `./run_local.sh` installs dependencies if missing and starts Vite.

```sh
npm run build
npm run preview
```

The production build is generated in `dist/`, which is ignored by Git.

## Editing content

- `src/workshop.js`: topics, important dates, organizers, and the tentative schedule.
- `src/App.vue`: submission guidelines, program, and page layout.
- `src/assets/style.css`: responsive styles.
- `index.html`: page title and social/search metadata.
- `public/`: robots.txt, sitemap.xml, and llms.txt; keep these aligned with public content.

`aaai_proposal` is a local symlink to the accepted proposal and CFP. **Do not edit,
remove, or traverse it for cleanup.** It is ignored by Git and is not required to
install, build, or deploy the site. Proposal files must never be copied to `public/`.

Content is based on `main.tex` and `call_for_participation.md` in that reference.
Tentative speaker invitees are not presented as confirmed speakers.

## Remaining announcements

- Confirm the exact workshop day (February 22 or 23, 2027), room, and schedule.
- Add confirmed speakers, program committee, and accepted papers when available.

## GitHub Pages

In the repository's **Settings → Pages**, select **GitHub Actions** as the build
source. `.github/workflows/deploy.yml` builds pull requests and deploys pushes to
`main` (or manual runs on `main`). Only `dist/` is uploaded. No generated files
need to be committed. The root base path assumes this organization-site URL.

The initial setup does not push commits or change repository settings.

## Original design reference

`AgenticSE-CAIS.github.io` is a read-only local symlink. Do not modify its target.
The site retains its original stylesheet, Inter font, Bootstrap layout, section
order, and organizer cards. Neither reference symlink is committed or deployed.
Organizer photos are stored in `public/images/` and linked from `src/workshop.js`.
The program schedule, keynote speakers, and accepted papers sections and their
navigation entries are commented out in `src/App.vue` until ready to announce.
