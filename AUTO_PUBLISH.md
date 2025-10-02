# Auto Publish Configuration

This repository is configured to automatically publish to npm and create GitHub releases when changes are pushed to the `main` branch.

## How It Works

The auto-publish workflow (`.github/workflows/auto-publish.yml`) is triggered when:
- Changes are pushed to the `main` branch
- The changes are in the `src/**` directory or `package.json` file

When triggered, the workflow will:
1. Build the project
2. Automatically bump the patch version (e.g., 10.0.0 → 10.0.1)
3. Check if the new version already exists on npm
4. If the version doesn't exist:
   - Commit the version bump to the main branch
   - Create a git tag (e.g., v10.0.1)
   - Publish the package to npm registry
   - Create a GitHub release

## Prerequisites

To enable this workflow, you need to configure the following:

### 1. NPM Token

The workflow needs an npm authentication token to publish packages.

1. Generate an npm token:
   - Log in to [npmjs.com](https://www.npmjs.com/)
   - Go to Account Settings → Access Tokens
   - Generate a new Automation token

2. Add the token to GitHub:
   - Go to your repository Settings → Secrets and variables → Actions
   - Click "New repository secret"
   - Name: `NPM_TOKEN`
   - Value: Your npm token
   - Click "Add secret"

### 2. GitHub Token

The `GITHUB_TOKEN` is automatically provided by GitHub Actions and doesn't require manual configuration. However, ensure that the workflow has the correct permissions:

- `contents: write` - To push commits and tags
- `packages: write` - To publish releases

These are already configured in the workflow file.

## Version Management

The workflow automatically manages versions:

- **Pre-release versions** (e.g., `10.0.0-alpha9`) are converted to stable versions (e.g., `10.0.0`)
- **Patch version** is automatically incremented (e.g., `10.0.0` → `10.0.1`)
- If you need to bump major or minor versions, update `package.json` manually before pushing

## Manual Publishing

If you need to publish manually without triggering the auto-publish workflow:

1. Update the version in `package.json`
2. Run `pnpm run build`
3. Run `npm publish`

Or use the existing release workflow by creating a GitHub release manually.

## Troubleshooting

### Workflow doesn't trigger

- Check that changes are in `src/**` or `package.json`
- Verify that you're pushing to the `main` branch
- Check the Actions tab in GitHub for any error messages

### Publish fails

- Verify that the `NPM_TOKEN` secret is correctly configured
- Check that you have publish permissions for the `ok-klinechart` package on npm
- Ensure the version doesn't already exist on npm

### Permission errors

- Ensure the workflow has `contents: write` permission
- Check that the GitHub token has sufficient permissions

## Disabling Auto-Publish

To disable auto-publish while keeping the workflow file:

1. Go to repository Settings → Actions → General
2. Under "Workflow permissions", adjust as needed
3. Or delete/rename the `.github/workflows/auto-publish.yml` file
