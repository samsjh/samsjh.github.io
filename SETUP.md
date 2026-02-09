# Setup Guide: Hosting Financial Planner on GitHub Pages

This guide explains how this repository is configured to host the Financial Planner application from the `samsjh/financial-planner` repository.

## ⚠️ Critical: Repository Visibility Requirement

**IMPORTANT: This repository MUST be PUBLIC for the GitHub Pages site to be accessible at https://samsjh.github.io/**

GitHub Pages for username.github.io repositories only works when the repository is public, unless you have GitHub Pro, Team, or Enterprise account.

### Steps to make the repository public:
1. Navigate to [Repository Settings](https://github.com/samsjh/samsjh.github.io/settings)
2. Scroll to the "Danger Zone" section at the bottom
3. Click "Change repository visibility"
4. Select "Make public"
5. Confirm by typing the repository name and clicking the confirmation button

**After making the repository public, the site will be accessible at https://samsjh.github.io/ within a few minutes.**

## Overview

This repository (`samsjh.github.io`) serves as a GitHub Pages deployment host for the Financial Planner application. When you push changes to the `main` branch of this repository, or when changes are made to the Financial Planner app, a GitHub Actions workflow automatically:

1. Fetches the latest code from `samsjh/financial-planner`
2. Builds the Next.js application as a static site
3. Deploys it to GitHub Pages at `https://samsjh.github.io`

## Configuration Details

### GitHub Actions Workflow

The deployment workflow is defined in `.github/workflows/deploy.yml`. It:

- Triggers on pushes to the `main` branch
- Can be manually triggered via the Actions tab
- Checks out the `samsjh/financial-planner` repository
- Builds the Next.js app with static export
- Deploys to GitHub Pages

### Requirements

For this setup to work, you need to:

1. **Enable GitHub Pages** in this repository's settings:
   - Go to Settings → Pages
   - Under "Source", select "GitHub Actions"

2. **Ensure the Financial Planner repo is accessible**:
   - The `samsjh/financial-planner` repository must be public OR
   - You need to configure access tokens if it's private

## Manual Deployment

To manually trigger a deployment:

1. Navigate to the [Actions tab](https://github.com/samsjh/samsjh.github.io/actions)
2. Select "Deploy Financial Planner to GitHub Pages" workflow
3. Click "Run workflow" button
4. Select the branch (usually `main`)
5. Click "Run workflow"

The workflow will start and you can monitor its progress in the Actions tab.

## Troubleshooting

### Workflow fails to checkout financial-planner

If the workflow fails with permission errors:
- Ensure the `samsjh/financial-planner` repository is public, or
- Set up a Personal Access Token with repo access and add it as a repository secret

### Build fails

If the build fails:
- Check the workflow logs in the Actions tab
- Ensure all dependencies in `financial-planner/package.json` are correct
- Verify the Next.js configuration supports static export

### Deployment fails

If deployment fails:
- Ensure GitHub Pages is enabled in repository settings
- Check that the Pages source is set to "GitHub Actions"
- Verify the workflow has the correct permissions (pages: write)

## Updating the Financial Planner

To update the deployed application:

1. Make changes to the `samsjh/financial-planner` repository
2. Once you're ready to deploy, run the workflow manually from this repository, or
3. Configure a workflow in the `financial-planner` repo to trigger this workflow

## Alternative: Automatic Deployment on Financial Planner Changes

If you want automatic deployments when `financial-planner` changes, you can:

1. Add a repository dispatch trigger to this workflow
2. Add a workflow to `financial-planner` that triggers this workflow on push

Example addition to this workflow:
```yaml
on:
  push:
    branches: ["main"]
  workflow_dispatch:
  repository_dispatch:
    types: [deploy-financial-planner]
```

And add this to `financial-planner/.github/workflows/`:
```yaml
name: Trigger Deployment
on:
  push:
    branches: ["main"]
jobs:
  trigger:
    runs-on: ubuntu-latest
    steps:
      - name: Trigger deployment
        run: |
          curl -X POST \
            -H "Authorization: token ${{ secrets.GH_PAT }}" \
            -H "Accept: application/vnd.github.v3+json" \
            https://api.github.com/repos/samsjh/samsjh.github.io/dispatches \
            -d '{"event_type":"deploy-financial-planner"}'
```

(Requires a Personal Access Token stored as `GH_PAT` secret in the financial-planner repo)
