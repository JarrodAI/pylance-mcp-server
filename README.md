# Pylance MCP Documentation

This directory contains the Mintlify documentation for Pylance MCP Server.

## Local Development

1. Install Mintlify CLI:
```bash
npm install -g mintlify
```

2. Start the development server:
```bash
cd website/docs
mintlify dev
```

3. Open http://localhost:3000 in your browser

## Deployment

### Option 1: Mintlify Hosting (Recommended)

1. Push your docs to GitHub
2. Go to [mintlify.com](https://mintlify.com)
3. Connect your repository
4. Set the docs directory to `website/docs`
5. Deploy - your docs will be available at `https://docs.pylancemcp.com`

### Option 2: Vercel

1. Install Vercel CLI:
```bash
npm install -g vercel
```

2. Deploy:
```bash
cd website/docs
vercel
```

3. Configure custom domain: `docs.pylancemcp.com`

## Structure

```
website/docs/
├── mint.json              # Mintlify configuration
├── introduction.mdx       # Home page
├── quickstart.mdx         # Quick start guide
├── installation.mdx       # Installation instructions
├── concepts/             # Core concepts
├── guides/               # Client-specific guides
├── api-reference/        # API documentation
└── deployment/           # Deployment guides
```

## Writing Documentation

### Creating a New Page

1. Create a new `.mdx` file in the appropriate directory
2. Add frontmatter:
```mdx
---
title: 'Page Title'
description: 'Page description'
---

# Your content here
```

3. Add the page to `mint.json` navigation

### Using Mintlify Components

```mdx
<Card title="Card Title" icon="rocket" href="/path">
  Card content
</Card>

<CardGroup cols={2}>
  <Card>Card 1</Card>
  <Card>Card 2</Card>
</CardGroup>

<Accordion title="Accordion Title">
  Collapsible content
</Accordion>

<Info>Info callout</Info>
<Warning>Warning callout</Warning>
<Note>Note callout</Note>
```

### Code Blocks

```python
# Python code with syntax highlighting
def hello_world():
    print("Hello from Pylance MCP!")
```

## Updating Configuration

Edit `mint.json` to update:
- Navigation structure
- Colors and branding
- Top bar links
- Footer links
- Anchors (GitHub, Discord, etc.)

## Domain Setup

Once deployed, configure your DNS:

```
Type: CNAME
Name: docs
Value: cname.vercel-dns.com (or Mintlify's CNAME)
```

Then update the docs link in the main website navigation.
