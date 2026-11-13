# Fork notes

This is Lorenzo's personal fork of [Anyesh/wardrowbe](https://github.com/Anyesh/wardrowbe). The `main` branch on Forgejo
(`https://git.lorenzoiovino.com/loke/wardrowbe`) is the source of truth for this fork (locally it is the `delemaco` branch).

## Versioning

Releases are named `<upstream version>-loke.<n>`. The current upstream base is `1.13.0`, so the first release is
`v1.13.0-loke.1`. When the upstream base changes, `n` restarts at 1.

## Pulling upstream

```
git fetch upstream
git merge upstream/main
```

Watch the Alembic migrations: this fork carries an extra merge migration (`d1e1a0c0de01`), and upstream may add
migrations of its own. After every merge check that there is a single head:

```
cd backend && alembic heads
```

If there is more than one, add a new merge migration before releasing.

## Releasing

1. Merge (or commit) into `main` on Forgejo and make sure the frontend checks pass (lint, `tsc --noEmit`, `vitest run`, `npm run build`).
2. Tag and push to Forgejo:
   ```
   git tag v1.13.0-loke.1
   git push forgejo HEAD:main v1.13.0-loke.1
   ```
3. `.forgejo/workflows/release.yml` builds `backend/` and `frontend/` (linux/amd64) and pushes
   `git.lorenzoiovino.com/loke/wardrowbe:backend-<version>` and `:frontend-<version>`, where `<version>` is the tag
   without the leading `v`. A manual run (`workflow_dispatch`) uses `dev-<short sha>` instead.

The upstream `.github/workflows` are left untouched but ignored by Forgejo, which uses `.forgejo/workflows/` exclusively.

## Deployment

Deployment lives in Lorenzo's `cloud` repo, under `ovh/configs/wardrowbe/`.
