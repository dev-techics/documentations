---
sidebar_position: 4
---

# Deployment

To deploy the project in the production server automatically there is a github action workflow is written in the `.github/workflows/ci.yml`.

This document describes the GitHub Actions CI workflow defined in `.github/workflows/ci.yml`.

**Overview**
- The workflow runs on every push and pull request.
- It has two jobs: `build` and `deploy`.
- `deploy` only runs on the `main` branch and only after `build` succeeds.

**Triggers**
- `push` to any branch
- `pull_request` to any branch

**Jobs**
**Build job (`build`)**
- Runs on `ubuntu-latest`.
- Checks out the repository.
- Sets up Node.js `22` with npm cache.
- Installs dependencies with `npm ci`.
- Builds the project with `npm run build`.

**Deploy job (`deploy`)**
- Runs on `ubuntu-latest`.
- Depends on `build` (`needs: build`).
- Only executes when `github.ref == 'refs/heads/main'`.
- Checks out the repository.
- Sets up Node.js `22`.
- Installs dependencies with `npm ci`.
- Builds the project with `npm run build`.
- Deploys the built `dist/` output to a server over SCP.

**Deployment details**
- Uses `appleboy/scp-action@v0.1.7`.
- Uploads `dist/**` and strips the top-level folder (`strip_components: 1`).
- Target destination comes from a secret.

**Required secrets**
- `SERVER_HOST`: SSH host for the deployment target.
- `SERVER_USER`: SSH username.
- `SERVER_SSH_KEY`: Private key with access to the target server.
- `SERVER_PATH`: Remote path where the build output should be deployed.

**Notes and common tweaks**
- If you want to deploy from a different branch, update the `if:` condition in the `deploy` job.
- If you change the build output directory (e.g., from `dist/` to something else), update the `source` field in the deploy step.