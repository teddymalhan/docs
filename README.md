# RetroWatch API Documentation

This directory contains the Mintlify documentation for the RetroWatch API.

## Prerequisites

Mintlify requires Node.js LTS version (18, 20, or 22). You currently have Node v25.2.1 installed.

To use Mintlify, you need to switch to an LTS version:

### Using nvm (Node Version Manager)

```bash
# Install Node 20 LTS
nvm install 20

# Use Node 20
nvm use 20

# Verify version
node --version
```

## Running Locally

Once you're on an LTS Node version:

```bash
# Navigate to docs directory
cd docs

# Start the dev server
pnpm dlx mintlify dev
```

The documentation will be available at `http://localhost:3000`

## Project Structure

```
docs/
├── docs.json              # Main configuration file
├── openapi.json           # OpenAPI specification
├── introduction.mdx       # Introduction page
├── authentication.mdx     # Authentication guide
├── api-reference/
│   └── overview.mdx      # API overview page
├── logo-light.svg        # Light mode logo
└── logo-dark.svg         # Dark mode logo
```

## Features

- **OpenAPI Integration**: Automatic API reference generation from your OpenAPI spec
- **Authentication Guide**: Detailed JWT authentication documentation
- **Custom Pages**: Introduction and overview pages for better user experience
- **Responsive Design**: Beautiful documentation that works on all devices

## Customization

### Updating the OpenAPI Spec

To update the API documentation, replace `docs/openapi.json` with the latest spec:

```bash
# If your API is running locally
curl http://localhost:8080/api-docs > docs/openapi.json

# From production
curl https://retrowatch.malhan.ca/api-docs > docs/openapi.json
```

### Modifying Configuration

Edit `docs/docs.json` to customize:
- Colors and theme
- Navigation structure
- Footer and social links
- Logo and branding

### Adding Pages

Create new `.mdx` files in the `docs/` directory and add them to the navigation in `docs.json`.

## Deployment

Mintlify can be deployed to:
- Mintlify hosting (recommended)
- Vercel
- Netlify
- Any static hosting provider

### Deploy to Mintlify

1. Sign up at https://mintlify.com
2. Connect your GitHub repository
3. Point to the `docs/` directory
4. Deploy automatically on every push

## Resources

- [Mintlify Documentation](https://mintlify.com/docs)
- [OpenAPI Integration Guide](https://mintlify.com/docs/api-playground/openapi-setup)
- [Component Library](https://mintlify.com/docs/content/components)
