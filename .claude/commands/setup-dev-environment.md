# Setup Dev Environment

Help the user set up their local development environment for contributing to the Payload CMS fork.

## Steps

### 1. Check Prerequisites

Verify the following tools are installed and meet version requirements:

```bash
node --version  # Requires Node.js >= 20.9.0
pnpm --version  # Requires pnpm >= 9.0.0
git --version
```

If any are missing, provide installation instructions for the user's OS.

### 2. Clone and Install

```bash
# Install dependencies from the repo root
pnpm install
```

This will install all workspace dependencies across all packages in the monorepo.

### 3. Build Core Packages

Build packages in dependency order:

```bash
pnpm build:core
```

If that script doesn't exist, build manually:

```bash
pnpm --filter @payloadcms/translations build
pnpm --filter payload build
pnpm --filter @payloadcms/ui build
pnpm --filter @payloadcms/next build
```

### 4. Configure Environment Variables

Check which test/dev apps need environment variables:

```bash
ls test/
ls apps/
```

For each app that has a `.env.example`, copy it:

```bash
cp .env.example .env
```

Common required variables:
- `MONGODB_URI` or `POSTGRES_URL` — database connection string
- `PAYLOAD_SECRET` — secret key for Payload (any random string works locally)
- `NEXT_PUBLIC_SERVER_URL` — usually `http://localhost:3000`

### 5. Start a Dev App

List available dev/test apps and let the user choose:

```bash
# Run the dev server for a specific test app
pnpm dev --filter <app-name>

# Or from the app directory
cd test/<app-name>
pnpm dev
```

### 6. Run Tests

Explain the test setup:

```bash
# Unit tests
pnpm test:unit

# Integration tests (requires running database)
pnpm test:int

# E2E tests (requires running dev server)
pnpm test:e2e

# Run tests for a specific package
pnpm --filter payload test
```

### 7. Verify Setup

Confirm everything works by:
1. Checking the dev server starts without errors
2. Running `pnpm build` successfully
3. Running at least one test suite

## Common Issues

### Port already in use
```bash
lsof -ti:3000 | xargs kill -9
```

### pnpm workspace issues
```bash
pnpm install --force
```

### TypeScript build errors after pulling changes
```bash
pnpm clean
pnpm install
pnpm build
```

### Database connection errors
- For MongoDB: ensure `mongod` is running locally or use MongoDB Atlas free tier
- For PostgreSQL: ensure `postgres` service is running
- Check connection string format in `.env`

## Notes

- This is a monorepo managed with pnpm workspaces and Turborepo
- Always run commands from the repo root unless working in a specific package
- The `packages/` directory contains publishable packages; `test/` contains integration test apps
- Check `turbo.json` for available pipeline tasks
