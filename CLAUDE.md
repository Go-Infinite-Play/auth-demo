# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

### Core Next.js Commands
- `npm run dev` - Start development server with Turbopack
- `npm run build` - Build for production
- `npm run start` - Start production server
- `npm run lint` - Run ESLint checks

### Database Operations
- `npm run db:generate` - Generate Drizzle database migrations
- `npm run db:migrate` - Run pending database migrations
- `npm run db:seed` - Seed database with initial data (uses Bun runtime)

## Architecture Overview

### Tech Stack
- **Framework**: Next.js 15 with App Router
- **Database**: PostgreSQL with Drizzle ORM
- **Authentication**: Clerk (@clerk/nextjs, @clerk/backend)
- **UI**: Tailwind CSS v4 + Radix UI components
- **Animation**: Framer Motion
- **Runtime**: Supports both Node.js and Bun (seed script uses Bun)

### Key Architecture Patterns

#### Database Layer
- **Schema**: Located in `db/schema/prompts-schema.ts`
- **Connection**: Centralized in `db/index.ts` using postgres-js client
- **Migrations**: Auto-generated in `db/migrations/` via Drizzle Kit

#### Server Actions Pattern
- All database operations are handled through Next.js Server Actions in `actions/prompts-actions.ts`
- Actions include: `getPrompts()`, `createPrompt()`, `updatePrompt()`, `deletePrompt()`
- Each action includes error handling and development delays via `devDelay()` utility

#### UI Architecture
- **Component Library**: Custom components in `components/ui/` following Radix UI + Tailwind patterns
- **Page Structure**: App Router with nested layouts
- **Loading States**: Uses React Suspense with dedicated loading components
- **State Management**: Server-side rendering with optimistic updates

### Important Files & Directories

#### Core Configuration
- `drizzle.config.ts` - Database migration configuration
- `components.json` - Tailwind/Radix UI component configuration 
- `tsconfig.json` - TypeScript config with `@/*` path mapping

#### Database
- `db/schema/prompts-schema.ts` - Prompts table schema with auto-timestamps
- `db/seed/index.ts` - Database seeding script (run with Bun)
- Environment variable required: `DATABASE_URL` (loaded from `.env.local`)

#### Development Utilities
- `lib/dev-delay.ts` - Adds artificial delays in development for testing loading states
- `lib/utils.ts` - Shared utilities (likely includes `cn()` for className merging)

### Environment Setup
- Database URL must be configured in `.env.local`
- Clerk authentication keys needed for auth functionality
- Uses dotenv for environment variable loading

### Development Notes
- Force dynamic rendering on prompts page (`export const dynamic = "force-dynamic"`)
- Development server includes artificial delays for testing loading states
- Components follow shadcn/ui patterns with Tailwind CSS
- Error boundaries and proper error handling implemented in server actions
- Do not edit migration files. We use Drizzle to manage migrations automatically and never touch those files.

### Auth Rules 
- We use Clerk for auth 
- Use Clerk middleware to protect routes 
- Use Clerk components (<UserButton>, <SignInButton>) for UI 
- Use Clerk server-side helpers (auth()) in Server Actions to get user ID

