---
title: 'Supabase setup guide'
description: 'Description of your new file.'
---
# Supabase Setup Guide

This guide covers setting up Supabase for both new and existing projects.

## Prerequisites

- Docker Desktop installed and running
- Node.js 18+ and pnpm installed
- Supabase CLI installed (`pnpm add -g supabase`)

## Option 1: Fresh Installation

### 1. Local Development

Run the automated setup script:

```bash
./scripts/customer/setup.sh
```

This script will:

- Create necessary environment files
- Install project dependencies
- Start Supabase locally
- Run database migrations
- Generate TypeScript types
- Provide local credentials

### 2. Production Setup

1. Create a new project on [supabase.com](https://supabase.com)

2. Link your project:

```bash
pnpm sb:link --project-ref your-project-ref
```

3. Push the schema:

```bash
pnpm sb:push
```

4. Update `.env.local` with production credentials from your Supabase dashboard

## Option 2: Adding to Existing Project

If you already have a Supabase project and want to add our schema:

### 1. Available Migrations

We provide modular migrations that can be applied selectively:

- `20240310000000_initial_schema.sql`

  - Base projects table
  - RLS policies
  - Project names view

- `20240310000001_content_tables.sql`

  - Content management
  - Social links
  - Directory links

- `20240310000002_stripe_tables.sql`

  - Stripe products
  - Pricing
  - Subscriptions

- `20240310000003_views.sql`
  - Content project view
  - Additional views

### 2. Integration Steps

1. Link to your existing project:

```bash
pnpm sb:link --project-ref your-project-ref
```

2. Review and apply desired migrations:

```bash
# View migration contents
cat supabase/migrations/20240310000000_initial_schema.sql

# Apply specific migration
pnpm sb:push
```

3. Generate types:

```bash
pnpm sb:generate-types
```

## Project-Specific Commands

```bash
# Database Operations
pnpm sb:generate-migration  # Create new migration from diff
pnpm sb:generate-seed      # Generate seed file
pnpm sb:push              # Push to production
pnpm sb:pull              # Pull remote schema
pnpm sb:generate-types    # Generate TypeScript types
```

## Environment Setup

Add these to your `.env.local`:

```bash
# Local Development
NEXT_PUBLIC_SUPABASE_URL=http://127.0.0.1:54321
NEXT_PUBLIC_SUPABASE_ANON_KEY=<from-setup-script>
SUPABASE_SERVICE_ROLE_KEY=<from-setup-script>

# Production
NEXT_PUBLIC_SUPABASE_URL=<your-project-url>
NEXT_PUBLIC_SUPABASE_ANON_KEY=<your-anon-key>
SUPABASE_SERVICE_ROLE_KEY=<your-service-role-key>
```

## Security Best Practices

1. **Service Role Key**

   - Only use in secure server contexts
   - Never expose in client-side code
   - Keep separate from anon key usage

2. **RLS Policies**

   - Projects: Users can only access their own projects
   - Content: Public access for published content
   - Stripe: Protected tables with specific policies

3. **Environment Variables**
   - Never commit `.env.local`
   - Rotate production keys regularly
   - Use different keys per environment

## Next Steps

1. Review generated `types_db.ts` for available tables and types
2. Set up authentication in Supabase dashboard
3. Test with provided seed data
4. Start building your features!
