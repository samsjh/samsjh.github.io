# samsjh.github.io

This repository hosts the [Financial Planner](https://github.com/samsjh/financial-planner) web application on GitHub Pages.

## ⚠️ Important: Repository Visibility

**This repository must be PUBLIC for the site to be accessible at https://samsjh.github.io/**

GitHub Pages for user/organization sites (username.github.io) requires the repository to be public unless you have GitHub Pro, Team, or Enterprise.

### To make the repository public:
1. Go to [Repository Settings](https://github.com/samsjh/samsjh.github.io/settings)
2. Scroll down to the "Danger Zone" section
3. Click "Change repository visibility"
4. Select "Make public"
5. Confirm the change

Once the repository is public, the GitHub Pages site will be accessible at https://samsjh.github.io/

## How it works

This repository is configured to automatically build and deploy the Financial Planner application from the `samsjh/financial-planner` repository to GitHub Pages at `https://samsjh.github.io`.

### Deployment Process

1. The GitHub Actions workflow (`.github/workflows/deploy.yml`) is triggered on pushes to the `main` branch or can be manually triggered.
2. The workflow checks out the `samsjh/financial-planner` repository.
3. It builds the Next.js application with static export enabled.
4. The built static files are deployed to GitHub Pages.

### Manual Deployment

To manually trigger a deployment:
1. Go to the [Actions tab](https://github.com/samsjh/samsjh.github.io/actions)
2. Select the "Deploy Financial Planner to GitHub Pages" workflow
3. Click "Run workflow"

## About Financial Planner

Financial Planner is a Next.js application for financial planning. For more information about the application itself, visit the [financial-planner repository](https://github.com/samsjh/financial-planner).