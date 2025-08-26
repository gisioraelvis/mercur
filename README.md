![Mercur Main Cover](https://cdn.prod.website-files.com/6790aeffc4b432ccaf1b56e5/67a225dc6fa298afc1cc4ae6_Mercur%20Cover.png)

<div align="center">
  <h1>Mercur <br> Open Source Marketplace Platform</h1> 
  <!-- Shields.io Badges -->
  <a href="https://github.com/mercurjs/mercur/tree/main?tab=MIT-1-ov-file">
    <img alt="License" src="https://img.shields.io/badge/license-MIT-blue.svg" />
  </a>
  <a href="#">
    <img alt="PRs Welcome" src="https://img.shields.io/badge/PRs-welcome-brightgreen.svg" />
  </a>
  <a href="https://rigbyjs.com/#contact">
    <img alt="Support" src="https://img.shields.io/badge/support-contact%20author-blueviolet.svg" />
  </a>
  <!-- Website Links -->
  <p>
    <a href="https://mercurjs.com/">Mercur</a> |   <a href="https://docs.mercurjs.com/">Docs</a> 
  </p> 
</div>

# What is Mercur?

<a href="https://www.mercurjs.com/">Mercur</a> is the first truly limitless open source marketplace platform that combines the simplicity of SaaS with the freedom of open source. Built on [MedusaJS](https://github.com/medusajs/medusa), it empowers businesses to create custom marketplaces without choosing between ownership and ease of use.

Mercur is a platform to start, customize, manage, and scale your marketplace for every business model with a modern technology stack.

## Announcing Mercur 1.0

After months of development, testing, and close collaboration with early adopters, we’re excited to announce the official release of **Mercur 1.0** - the first truly limitless marketplace platform. Version 1.0 is fully open source and ready to be self-hosted, giving you **full control over infrastructure, customizations, and data**.

With this version, **Mercur is production-ready for B2C marketplaces**. The first complete version includes a vendor system, admin panel, and a fully built B2C Storefront. Read more in **[official release announcement](https://www.mercurjs.com/updates/mercur-1-0-release)**

## Why Choose Mercur?

- Full Ownership: Unlike SaaS platforms, you own your marketplace with no transaction fees or vendor lock-in
- Modern Foundation: Built on MedusaJS, offering a modern tech stack that developers love
- Beautiful by Default: Create stunning storefronts without sacrificing customization

## Power Any Marketplace Model

- Custom B2B Marketplace: Build enterprise-grade platforms with specialized workflows
- Custom B2C Marketplace: Create engaging consumer marketplaces with modern UX
- eCommerce Extension: Transform your store into a marketplace (coming soon)

![Mercur Use Cases](https://cdn.prod.website-files.com/6790aeffc4b432ccaf1b56e5/67b46aa08180d5b8499c6a15_Use-cases.jpg)
&nbsp;

# Ready-to-go marketplace features

<b>Storefronts for Marketplace </b> <br>
Customizable storefronts designed for B2B and B2C with all elements including browsing and buying products across multiple vendors at once.

Discover <a href="https://github.com/mercurjs/b2c-marketplace-storefront">B2C Storefront Repository</a> - <a href="https://b2c.mercurjs.com/">🛍️ Check demo </a>

<b>Admin Panel</b> <br>
Control over whole marketplace: setting product categories, vendors, commissions and rules

<b>Vendor Panel</b> <br>
A powerful dashboard giving sellers complete control over their products, orders, and store management in one intuitive interface.

Discover <a href="https://github.com/mercurjs/vendor-panel">Vendor Panel</a> - <a href="https://www.mercurjs.com/contact"> Contact us to get demo </a>

<b>Integrations</b> <br>
Built-in integration with Stripe for payments and Resend for communication needs. More integrations coming soon.

![Mercur](https://cdn.prod.website-files.com/6790aeffc4b432ccaf1b56e5/67a1020f202572832c954ead_6b96703adfe74613f85133f83a19b1f0_Fleek%20Tilt%20-%20Readme.png)

&nbsp;

## Quickstart

#### Setup Medusa project

```bash
# Clone the repository
git clone https://github.com/mercurjs/mercur.git

# Change directory
cd mercur

# Install dependencies
pnpm install

# Build packages
pnpm build

# Go to backend folder
cd apps/backend

# Clone .env.template
cp .env.template .env

# In the .env file replace user, password, address and port parameters in the DATABASE_URL variable with your values
DATABASE_URL=postgres://[user]:[password]@[address]:[port]/$DB_NAME
# For example:
DATABASE_URL=postgres://postgres:postgres@localhost:5432/$DB_NAME

# Setup database and run migrations
pnpm medusa db:create && pnpm medusa db:migrate && pnpm run seed

# Create admin user
npx medusa user --email <email> --password <password>

# Go to root folder
cd ../..

# Start Mercur
pnpm dev
```

&nbsp;

## Development Commands

### Root Project Commands

Run these from the root directory (`mercur/`):

```bash
# Start all services in development mode
pnpm dev

# Build the entire project (all packages)
pnpm build

# Run linting across all packages
pnpm lint

# Format code across all packages
pnpm format

# Generate OpenAPI specifications
pnpm generate:oas

# Alternative way to start development (same as pnpm dev)
pnpm mercur-exec
```

### Backend-Specific Commands

Navigate to the backend directory first:

```bash
cd apps/backend
```

Then run:

```bash
# Start backend in development mode
pnpm dev

# Build the backend only
pnpm build

# Start backend in production mode
pnpm start

# Database operations
pnpm db:migrate          # Run database migrations
pnpm seed               # Seed database with sample data

# Testing
pnpm test:unit          # Run unit tests
pnpm test:integration:http    # Run HTTP integration tests
pnpm test:integration:modules # Run module integration tests

# Code quality
pnpm lint               # Lint backend code
pnpm lint:fix           # Fix linting issues automatically
pnpm format             # Format backend code

# Admin user management
npx medusa user --email <email> --password <password>  # Create admin user
```

## First-Time Setup Guide

### 1. Install Dependencies

```bash
cd mercur
pnpm install
```

### 2. Build All Packages

```bash
pnpm build
```

### 3. Configure Environment

```bash
cd apps/backend
cp .env.template .env
# Edit .env file with your configuration:
# - Database connection details
# - API keys for integrations (Stripe, Resend, etc.)
# - Other environment-specific settings
```

### 4. Database Setup

```bash
# Create database and run migrations
pnpm medusa db:create && pnpm medusa db:migrate

# Seed with sample data
pnpm seed
```

### 5. Create Admin User

```bash
npx medusa user --email admin@example.com --password your-secure-password
```

### 6. Start Development

```bash
cd ../..  # Go back to root
pnpm dev
```

## Accessing the Application

After running `pnpm dev`, you can access:

- **API Server**: `http://localhost:9000`
- **Admin Panel**: `http://localhost:9000/app`
- **API Documentation**: `http://localhost:9000/docs` (if enabled)

## Package Management with pnpm

### Workspace Commands

```bash
# Install dependencies for all packages
pnpm install

# Install a dependency to the root workspace
pnpm add <package-name> -w

# Install a dependency to a specific package
pnpm add <package-name> --filter <package-name>

# List all packages in the workspace
pnpm list --depth=0 --recursive

# Run a command in all packages
pnpm -r <command>

# Run a command in a specific package
pnpm --filter <package-name> <command>
```

### Key Workspace Packages

- `@mercurjs/framework` - Core framework utilities
- `@mercurjs/marketplace` - Marketplace-specific functionality
- `@mercurjs/seller` - Seller/vendor management
- `@mercurjs/commission` - Commission calculation system
- `@mercurjs/payout` - Payout management
- `@mercurjs/reviews` - Review system
- `@mercurjs/wishlist` - Wishlist functionality
- And many more modules in `packages/modules/`

## Troubleshooting

### Common Issues

**Build Errors:**

```bash
# Clean and rebuild
pnpm build --force

# Or clean node_modules and reinstall
rm -rf node_modules pnpm-lock.yaml
pnpm install
pnpm build
```

**Database Connection Issues:**

- Verify PostgreSQL is running
- Check DATABASE_URL in `.env` file
- Ensure database exists: `pnpm medusa db:create`

**Port Conflicts:**

- Check if ports 9000 (API) are available
- Modify ports in `.env` if needed

**Workspace Dependency Issues:**

```bash
# Reinstall all dependencies
pnpm install --force
```

## Production Deployment

For production deployment:

```bash
# Build all packages for production
pnpm build

# Start the backend in production mode
cd apps/backend
pnpm start
```

Make sure to:

- Set `NODE_ENV=production` in your environment
- Configure production database credentials
- Set up proper reverse proxy (nginx, etc.)
- Configure SSL certificates
- Set up monitoring and logging

&nbsp;

## Prerequisites

- Node.js v20+
- PostgreSQL
- Git CLI
- pnpm (installed via Corepack: `corepack enable`)

# Resources

#### Learn more about Mercur

- [Mercur Website](https://www.mercurjs.com/)
- [Mercur Docs](https://docs.mercurjs.com/introduction)

#### Learn more about Medusa

- [Medusa Website](https://www.medusajs.com/)
- [Medusa Docs](https://docs.medusajs.com/v2)
