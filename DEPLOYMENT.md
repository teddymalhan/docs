# Deploying RetroWatch Documentation to Mintlify

This guide walks you through deploying your documentation to Mintlify's hosting platform.

## Deployment Options

### Option 1: Mintlify Hosting (Recommended)

Mintlify provides free hosting with automatic deployments from GitHub. This is the easiest and most integrated solution.

### Option 2: Self-Hosting

You can also deploy to Vercel, Netlify, or any static hosting provider.

---

## Option 1: Deploy to Mintlify

### Step 1: Create a Mintlify Account

1. Go to [https://mintlify.com](https://mintlify.com)
2. Click **Sign Up** or **Get Started**
3. Sign in with your GitHub account

### Step 2: Create a New Project

1. From the Mintlify dashboard, click **New Project** or **Create Project**
2. Choose **Connect GitHub Repository**
3. You'll be redirected to install the Mintlify GitHub App

### Step 3: Install the Mintlify GitHub App

1. On the GitHub App installation page:
   - Select the organization or user account where your repository is located
   - Choose **Only select repositories**
   - Select your `full-stack-spring` repository
   - Click **Install & Authorize**

<Note>
You must have admin permissions on the repository. If you don't, the repository owner will receive an approval request.
</Note>

### Step 4: Configure Your Project

Once the GitHub App is installed:

1. Back in the Mintlify dashboard, select your repository
2. Configure the following settings:
   - **Repository**: `your-username/full-stack-spring`
   - **Branch**: `main` (or your default branch)
   - **Documentation Directory**: `docs/`
   - **Subdomain**: Choose your subdomain (e.g., `retrowatch.mintlify.app`)

3. Click **Create Project** or **Deploy**

### Step 5: Verify Deployment

1. Mintlify will automatically build and deploy your documentation
2. Watch the deployment logs in the dashboard
3. Once complete, your docs will be live at: `https://your-subdomain.mintlify.app`

### Step 6: Set Up Custom Domain (Optional)

To use a custom domain like `docs.retrowatch.malhan.ca`:

1. Go to **Settings** > **Domain** in your Mintlify dashboard
2. Enter your custom domain
3. Add the following DNS records to your domain provider:

```
Type: CNAME
Name: docs (or your subdomain)
Value: cname.mintlify.com
```

4. Click **Verify Domain** in Mintlify
5. SSL certificates are automatically provisioned

---

## Automatic Deployments

Once set up, Mintlify automatically deploys when you push changes to GitHub:

```bash
# Make changes to your docs
cd docs
vim introduction.mdx

# Commit and push
git add .
git commit -m "Update documentation"
git push origin main

# Mintlify automatically detects and deploys!
```

You can monitor deployments in:
- The Mintlify dashboard
- Your GitHub repository's commit history
- Deployment notifications (if configured)

---

## Option 2: Deploy with GitHub Actions (Advanced)

For more control, you can trigger deployments via the Mintlify API:

### Step 1: Get API Credentials

1. Go to your Mintlify dashboard
2. Navigate to **Settings** > **API**
3. Copy your **Project ID** and **API Key**

### Step 2: Add GitHub Secrets

1. Go to your GitHub repository
2. Navigate to **Settings** > **Secrets and variables** > **Actions**
3. Add two secrets:
   - `MINTLIFY_PROJECT_ID`: Your project ID
   - `MINTLIFY_API_KEY`: Your API key

### Step 3: Create GitHub Actions Workflow

Create `.github/workflows/deploy-docs.yml`:

```yaml
name: Deploy Documentation

on:
  push:
    branches:
      - main
    paths:
      - 'docs/**'
      - 'backend/src/main/java/com/richwavelet/backend/config/OpenApiConfig.java'

jobs:
  update-docs:
    runs-on: ubuntu-latest
    steps:
      - name: Trigger Mintlify Deployment
        run: |
          curl -X POST https://api.mintlify.com/v1/project/update/${{ secrets.MINTLIFY_PROJECT_ID }} \
            -H "Authorization: Bearer ${{ secrets.MINTLIFY_API_KEY }}" \
            -H "Content-Type: application/json"
```

---

## Updating OpenAPI Spec

Your OpenAPI spec is in `docs/openapi.json`. To update it:

### Manual Update

```bash
# From production
curl https://retrowatch.malhan.ca/api-docs > docs/openapi.json

# Or from local
curl http://localhost:8080/api-docs > docs/openapi.json

# Commit and push
git add docs/openapi.json
git commit -m "Update OpenAPI spec"
git push
```

### Automated Update (Add to CI/CD)

You could add this to your backend deployment pipeline:

```yaml
# In your backend CI/CD
- name: Update OpenAPI Spec
  run: |
    curl https://retrowatch.malhan.ca/api-docs > docs/openapi.json
    git add docs/openapi.json
    git commit -m "chore: update OpenAPI spec [skip ci]" || exit 0
    git push
```

---

## Option 3: Self-Hosting Alternatives

### Deploy to Vercel

```bash
# Install Vercel CLI
pnpm i -g vercel

# Deploy from docs directory
cd docs
vercel

# Follow the prompts
```

### Deploy to Netlify

```bash
# Install Netlify CLI
pnpm i -g netlify-cli

# Build and deploy
cd docs
netlify deploy --prod
```

<Warning>
Self-hosting requires manually building the Mintlify site. The GitHub integration and automatic deployments only work with Mintlify hosting.
</Warning>

---

## Troubleshooting

### Node Version Issues

If you get "node version 25+ not supported":

```bash
# Use Node 20 LTS
nvm install 20
nvm use 20
node --version  # Should show v20.x.x
```

### OpenAPI Spec Not Loading

1. Verify `docs/openapi.json` is valid JSON
2. Check the `openapi` field in `docs/docs.json` points to the correct file
3. Ensure the OpenAPI spec version is 3.0 or 3.1

### Deployment Fails

1. Check the Mintlify dashboard for error logs
2. Verify your `docs/docs.json` is valid
3. Ensure all referenced files exist
4. Check that MDX files have valid frontmatter

### Custom Domain Not Working

1. Verify DNS records are correct
2. Wait up to 48 hours for DNS propagation
3. Check the domain status in Mintlify dashboard
4. Ensure CNAME points to `cname.mintlify.com`

---

## Resources

- [Mintlify Dashboard](https://dashboard.mintlify.com)
- [Mintlify Documentation](https://mintlify.com/docs)
- [GitHub App Settings](https://github.com/apps/mintlify/installations/new)
- [Mintlify Community](https://mintlify.com/community)
