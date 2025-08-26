# Railway Deployment Guide

This guide will help you deploy your Mercur marketplace to Railway with PostgreSQL and automatic CI/CD.

## Prerequisites

- Railway account (free): [railway.app](https://railway.app)
- GitHub repository with your Mercur code
- PostgreSQL database (Railway provides this)

## Quick Setup (5 minutes)

### 1. Connect to Railway

```bash
# Install Railway CLI
npm install -g @railway/cli

# Login to Railway
railway login

# Link your project (run from project root)
railway link
```

### 2. Add PostgreSQL Database

In Railway dashboard:

1. Click "New" → "Database" → "PostgreSQL"
2. Copy the connection string from the "Connect" tab
3. It will look like: `postgresql://postgres:password@host:port/database`

### 3. Configure Environment Variables

In Railway dashboard, go to your service → "Variables" tab and add:

```env
NODE_ENV=production
DATABASE_URL=${{PostgreSQL.DATABASE_URL}}
MEDUSA_ADMIN_ONBOARDING_TYPE=nextjs
JWT_SECRET=your-super-secret-jwt-key-here
COOKIE_SECRET=your-super-secret-cookie-key-here
CORS_ADMIN_URL=https://your-app.railway.app
CORS_STORE_URL=https://your-storefront.com
```

**Important**: Generate secure secrets:

```bash
# Generate JWT_SECRET and COOKIE_SECRET
node -e "console.log(require('crypto').randomBytes(64).toString('hex'))"
```

### 4. Deploy

```bash
# Deploy from your local machine
railway up

# Or setup automatic deployment (see CI/CD section below)
```

## Detailed Setup

### Environment Variables Reference

| Variable                | Description           | Example                        |
| ----------------------- | --------------------- | ------------------------------ |
| `NODE_ENV`              | Environment           | `production`                   |
| `DATABASE_URL`          | PostgreSQL connection | Railway provides this          |
| `JWT_SECRET`            | JWT token secret      | Generate with crypto           |
| `COOKIE_SECRET`         | Session cookie secret | Generate with crypto           |
| `CORS_ADMIN_URL`        | Admin panel URL       | `https://your-app.railway.app` |
| `CORS_STORE_URL`        | Storefront URL        | `https://your-storefront.com`  |
| `STRIPE_API_KEY`        | Stripe secret key     | `sk_live_...` or `sk_test_...` |
| `STRIPE_WEBHOOK_SECRET` | Stripe webhook secret | `whsec_...`                    |
| `RESEND_API_KEY`        | Resend email API key  | `re_...`                       |

### Custom Domain Setup

1. In Railway dashboard → Your service → "Settings" → "Domains"
2. Click "Custom Domain"
3. Enter your domain (e.g., `api.yourmarketplace.com`)
4. Update your DNS settings as shown
5. Update `CORS_ADMIN_URL` to use your custom domain

### Database Migration

Railway will automatically run migrations, but you can also run them manually:

```bash
# Run migrations
railway run pnpm db:migrate

# Seed database (optional)
railway run pnpm seed

# Create admin user
railway run npx medusa user --email admin@example.com --password secure-password
```

## CI/CD Setup (Automatic Deployment)

### 1. Setup GitHub Secrets

In your GitHub repository → Settings → Secrets and variables → Actions:

Add these secrets:

- `RAILWAY_TOKEN`: Get from Railway dashboard → Account Settings → Tokens
- `RAILWAY_SERVICE`: Your service ID from Railway (optional, for database commands)

### 2. Automatic Deployment

The GitHub Actions workflow (`.github/workflows/deploy.yml`) will:

- ✅ Run tests on every push/PR
- ✅ Deploy to Railway on push to `main` branch
- ✅ Run database migrations automatically
- ✅ Handle build caching for faster deployments

## Railway Configuration Files

### `railway.json`

```json
{
  "$schema": "https://railway.app/railway.schema.json",
  "build": {
    "builder": "NIXPACKS",
    "buildCommand": "pnpm install && pnpm build"
  },
  "deploy": {
    "startCommand": "cd apps/backend && pnpm start",
    "healthcheckPath": "/health",
    "healthcheckTimeout": 300,
    "restartPolicyType": "ON_FAILURE",
    "restartPolicyMaxRetries": 10
  }
}
```

### `nixpacks.toml`

```toml
[build]
cmd = 'pnpm install && pnpm build'

[start]
cmd = 'cd apps/backend && pnpm start'

[variables]
NODE_VERSION = '20'
PNPM_VERSION = '9'
```

## Deployment Commands

```bash
# Deploy manually
railway up

# Deploy specific service
railway up --service your-service-name

# Check deployment status
railway status

# View logs
railway logs

# Run commands on Railway
railway run pnpm db:migrate
railway run pnpm seed

# Connect to database
railway connect PostgreSQL
```

## Monitoring and Logs

### View Application Logs

```bash
# Real-time logs
railway logs --follow

# Recent logs
railway logs --tail 100
```

### Health Check

Your app will be available at: `https://your-app.railway.app`

Health check endpoint: `https://your-app.railway.app/health`

### Railway Dashboard

Monitor your app at: [railway.app/dashboard](https://railway.app/dashboard)

- CPU/Memory usage
- Request metrics
- Error tracking
- Database metrics

## Scaling

Railway automatically scales based on traffic, but you can also:

1. **Vertical Scaling**: Upgrade your plan for more CPU/RAM
2. **Horizontal Scaling**: Contact Railway support for multiple instances

## Cost Optimization

### Free Tier Limits

- $5 credit per month
- Enough for development and small production apps
- No credit card required

### Pricing

- **Hobby Plan**: $5/month execution time
- **Pro Plan**: $20/month + usage
- **PostgreSQL**: Included in all plans

### Cost-Saving Tips

```bash
# Sleep unused services
railway service sleep your-service-name

# Monitor usage in dashboard
railway usage
```

## Troubleshooting

### Common Issues

**Build Failures:**

```bash
# Check build logs
railway logs --deployment

# Force rebuild
railway up --force
```

**Database Connection Issues:**

- Verify `DATABASE_URL` is set correctly
- Check PostgreSQL service is running
- Ensure database name matches

**Environment Variables:**

- Check all required variables are set in Railway dashboard
- Restart service after changing variables

**Performance Issues:**

- Monitor resource usage in Railway dashboard
- Consider upgrading plan if hitting limits
- Optimize database queries

### Getting Help

1. **Railway Documentation**: [docs.railway.app](https://docs.railway.app)
2. **Railway Discord**: Join their community
3. **GitHub Issues**: Report bugs in your repository
4. **Medusa Documentation**: [docs.medusajs.com](https://docs.medusajs.com)

## Production Checklist

Before going live:

- [ ] Set `NODE_ENV=production`
- [ ] Use production database (not shared/dev)
- [ ] Generate secure `JWT_SECRET` and `COOKIE_SECRET`
- [ ] Configure custom domain
- [ ] Set up Stripe production keys
- [ ] Configure email service (Resend)
- [ ] Set up monitoring/alerting
- [ ] Create admin user account
- [ ] Test all critical flows
- [ ] Set up backups (Railway provides automatic backups)

## Security Best Practices

1. **Environment Variables**: Never commit secrets to git
2. **Database**: Use Railway's managed PostgreSQL
3. **HTTPS**: Railway provides SSL certificates automatically
4. **CORS**: Configure proper CORS origins
5. **Rate Limiting**: Consider adding rate limiting for production
6. **Monitoring**: Set up error tracking and monitoring

---

**Quick Start Summary:**

1. `railway login` → `railway link`
2. Add PostgreSQL database in Railway dashboard
3. Set environment variables in Railway dashboard
4. `railway up`
5. Done! Your marketplace is live 🚀
