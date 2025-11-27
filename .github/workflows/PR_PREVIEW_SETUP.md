# PR Preview Workflow Setup

This repository includes a GitHub Actions workflow that automatically builds and deploys preview versions of your site for each pull request.

## How it Works

When a pull request is opened or updated:
1. The Jekyll site is built with a baseurl of `/pr-{number}/`
2. The built site is deployed to the `gh-pages-preview` branch in a subdirectory
3. A comment is added to the PR with the preview URL
4. When the PR is closed, the preview is automatically cleaned up

## Setup Instructions

To enable PR previews, you need to configure GitHub Pages to serve from the `gh-pages-preview` branch:

### Option 1: Enable GitHub Pages from gh-pages-preview branch (Recommended)

1. Go to your repository settings
2. Navigate to **Settings > Pages**
3. Under "Build and deployment", set:
   - **Source**: Deploy from a branch
   - **Branch**: `gh-pages-preview`
   - **Folder**: `/ (root)`
4. Save the settings

Your PR previews will be available at: `https://csherland.github.io/pr-{number}/`

**Note**: This only works if this is a project repository. For user/org sites (`username.github.io`), you'll need to use Option 2.

### Option 2: Use Netlify for PR Previews (For user sites)

If this is a user site (username.github.io), GitHub Pages only supports one deployment per repository. Instead, use Netlify:

1. Sign up for a free [Netlify](https://www.netlify.com/) account
2. Import this repository to Netlify
3. Go to **Site settings > Build & deploy > Deploy contexts**
4. Enable "Deploy Previews" for pull requests
5. Netlify will automatically comment on PRs with preview URLs

### Option 3: Manual Preview (No setup required)

The workflow uploads the built site as an artifact. You can download and view it locally:

1. Go to the PR's "Checks" tab
2. Click on "PR Preview" workflow run
3. Download the "site-preview" artifact
4. Extract and open `index.html` in your browser

## Customization

You can customize the workflow by editing `.github/workflows/pr-preview.yml`:

- Change the preview URL format
- Modify the comment message
- Add additional build steps or checks
- Configure different cleanup behavior

## Troubleshooting

**Preview URL returns 404:**
- Make sure GitHub Pages is configured for the `gh-pages-preview` branch
- Wait a few minutes for GitHub Pages to build and deploy

**Workflow fails:**
- Check the Actions tab for error details
- Ensure the `gh-pages-preview` branch exists (it will be created automatically on first run)
- Verify that GitHub Actions has write permissions in repository settings
