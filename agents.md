# Fridgesheet - AI Agent Context Guide

This document provides AI coding agents with essential context about the fridgesheet project.

---

## 1. Project Overview

**What it does:** Fridgesheet is a full-stack web application built with TanStack Start, featuring user authentication and a protected dashboard system.

**Who it's for:** This is currently a starter template that can be extended into a production application with custom features.

**Core value prop:** A modern, type-safe, full-stack React application with built-in authentication, database ORM, and a complete development toolchain.

---

## 2. Tech Stack

### Frontend
- **Framework:** React 19 with React Compiler
- **Routing:** TanStack Router (file-based routing)
- **Data Fetching:** TanStack Query
- **Full-Stack:** TanStack Start (SSR/SSG)
- **Styling:** Tailwind CSS v4
- **UI Components:** shadcn/ui + Radix UI
- **Forms:** React Hook Form (when needed)
- **Icons:** Lucide React

### Backend
- **Runtime:** Nitro v3 (alpha)
- **Build Tool:** Vite 8 (beta)
- **Database:** PostgreSQL
- **ORM:** Drizzle ORM
- **Authentication:** Better Auth
- **Validation:** Zod

### DevTools
- **Package Manager:** pnpm
- **Linting:** ESLint 9 + TypeScript ESLint
- **Formatting:** Prettier (with plugins: organize-imports, tailwindcss)
- **Type Checking:** TypeScript 5.9

---

## 3. Project Structure

```
fridgesheet/
├── src/
│   ├── routes/                         # File-based routing (TanStack Router)
│   │   ├── __root.tsx                  # Root layout, wraps all routes
│   │   ├── index.tsx                   # Home page (/)
│   │   ├── (auth-pages)/               # Route group for auth pages
│   │   │   ├── route.tsx               # Layout for auth pages
│   │   │   ├── login.tsx               # Login page (/login)
│   │   │   └── signup.tsx              # Signup page (/signup)
│   │   ├── (authenticated)/            # Route group for protected pages
│   │   │   └── dashboard/
│   │   │       └── index.tsx           # Dashboard (/dashboard)
│   │   └── api/auth/                   # Better Auth API routes
│   │       └── [...all].ts             # Catch-all for auth endpoints
│   │
│   ├── lib/                            # Core utilities and configs
│   │   ├── auth/
│   │   │   ├── auth.ts                 # Better Auth server config
│   │   │   ├── auth-client.ts          # Better Auth client instance
│   │   │   ├── middleware.ts           # Auth middleware for server functions
│   │   │   ├── queries.ts              # TanStack Query options for auth
│   │   │   └── functions.ts            # Auth utility functions
│   │   ├── db/
│   │   │   ├── index.ts                # Drizzle client initialization
│   │   │   └── schema/
│   │   │       ├── index.ts            # Schema exports
│   │   │       └── auth.schema.ts      # Better Auth tables (auto-generated)
│   │   └── utils.ts                    # General utilities (cn, etc.)
│   │
│   ├── components/                     # React components
│   │   ├── ui/                         # shadcn/ui components
│   │   │   ├── button.tsx
│   │   │   ├── input.tsx
│   │   │   ├── label.tsx
│   │   │   ├── dropdown-menu.tsx
│   │   │   └── sonner.tsx              # Toast notifications
│   │   ├── theme-provider.tsx          # Dark mode context provider
│   │   ├── theme-toggle.tsx            # Theme switcher component
│   │   ├── sign-in-social-button.tsx   # OAuth login buttons
│   │   ├── sign-out-button.tsx         # Sign out button
│   │   ├── default-catch-boundary.tsx  # Error boundary
│   │   └── default-not-found.tsx       # 404 page
│   │
│   ├── env/                            # Environment variable validation
│   │   ├── server.ts                   # Server-side env (DATABASE_URL, etc.)
│   │   └── client.ts                   # Client-side env (VITE_BASE_URL)
│   │
│   ├── router.tsx                      # Router setup and configuration
│   └── routeTree.gen.ts                # Auto-generated route tree (DO NOT EDIT)
│
├── drizzle/                            # Database migrations (generated)
├── public/                             # Static assets
│
├── .env                                # Environment variables (gitignored)
├── .env.example                        # Environment template
├── drizzle.config.ts                   # Drizzle Kit configuration
├── vite.config.ts                      # Vite + TanStack Start config
├── tsconfig.json                       # TypeScript configuration
└── package.json                        # Dependencies and scripts
```

### Naming Conventions
- **Routes:** Use file-based routing. Folders in parentheses are route groups: `(auth-pages)`
- **Components:** PascalCase for component files and exports
- **Utilities:** camelCase for functions and variables
- **Database:** snake_case (enforced by Drizzle config)
- **CSS:** Tailwind utility classes, kebab-case for custom classes

---

## 4. Common Commands

### Development
```bash
pnpm install              # Install dependencies
pnpm dev                  # Start dev server (http://localhost:3000)
pnpm build                # Build for production
pnpm preview              # Preview production build
pnpm start                # Start production server
```

### Database
```bash
pnpm db push              # Push schema to database (no migrations)
pnpm db generate          # Generate migrations from schema
pnpm db migrate           # Run migrations
pnpm db studio            # Open Drizzle Studio (visual DB browser)
pnpm db                   # Run any drizzle-kit command
```

### Authentication
```bash
pnpm auth:secret          # Generate BETTER_AUTH_SECRET
pnpm auth:generate        # Regenerate auth.schema.ts from auth config
```

### Code Quality
```bash
pnpm format               # Run Prettier
pnpm lint                 # Run ESLint
pnpm check-types          # TypeScript type check
pnpm check                # Run format + lint + check-types
```

### UI Components
```bash
pnpm ui add <component>   # Add shadcn/ui component (e.g., pnpm ui add card)
```

### Dependencies
```bash
pnpm deps                 # Interactive dependency updater
pnpm deps:major           # Check for major version updates
```

---

## 5. Feature Specs

### Current Features

#### Authentication
- Email/password signup and login
- OAuth providers (GitHub, Google) - requires API keys in `.env`
- Session management with PostgreSQL
- Protected routes using middleware
- Email verification support

#### UI/UX
- Dark/light mode toggle with persistence
- Responsive design (mobile-first)
- Toast notifications (Sonner)
- Loading states and suspense boundaries
- Error boundaries for graceful failures

#### Developer Experience
- Hot module replacement
- TypeScript strict mode
- Auto-generated route types
- TanStack DevTools (Router, Query, React)
- Database visual browser (Drizzle Studio)

### Planned Features
(Add your upcoming features here as you plan them)

---

## 6. Database Schema

### Current Tables (Better Auth)

#### `user`
```typescript
{
  id: text (PK)
  name: text
  email: text (unique)
  emailVerified: boolean (default: false)
  image: text (nullable)
  createdAt: timestamp (default: now)
  updatedAt: timestamp (auto-update)
}
```

#### `session`
```typescript
{
  id: text (PK)
  expiresAt: timestamp
  token: text (unique)
  createdAt: timestamp (default: now)
  updatedAt: timestamp (auto-update)
  ipAddress: text (nullable)
  userAgent: text (nullable)
  userId: text (FK -> user.id, cascade delete)
}
// Indexes: userId
```

#### `account`
```typescript
{
  id: text (PK)
  accountId: text
  providerId: text (e.g., "github", "google", "credential")
  userId: text (FK -> user.id, cascade delete)
  accessToken: text (nullable)
  refreshToken: text (nullable)
  idToken: text (nullable)
  accessTokenExpiresAt: timestamp (nullable)
  refreshTokenExpiresAt: timestamp (nullable)
  scope: text (nullable)
  password: text (nullable, hashed)
  createdAt: timestamp (default: now)
  updatedAt: timestamp (auto-update)
}
// Indexes: userId
```

#### `verification`
```typescript
{
  id: text (PK)
  identifier: text (e.g., email address)
  value: text (verification code/token)
  expiresAt: timestamp
  createdAt: timestamp (default: now)
  updatedAt: timestamp (auto-update)
}
// Indexes: identifier
```

### Relationships
- `session.userId` → `user.id` (CASCADE delete)
- `account.userId` → `user.id` (CASCADE delete)

### Adding New Schemas
1. Create schema in `src/lib/db/schema/<name>.schema.ts`
2. Export from `src/lib/db/schema/index.ts`
3. Run `pnpm db push` to sync to database
4. (Optional) Run `pnpm db generate` to create migrations

**Example:**
```typescript
// src/lib/db/schema/items.schema.ts
import { pgTable, text, timestamp } from "drizzle-orm/pg-core";
import { user } from "./auth.schema";

export const items = pgTable("items", {
  id: text("id").primaryKey(),
  name: text("name").notNull(),
  userId: text("user_id")
    .notNull()
    .references(() => user.id, { onDelete: "cascade" }),
  createdAt: timestamp("created_at").defaultNow().notNull(),
});
```

---

## 7. Code Patterns

### Server Functions (TanStack Start)

Server functions run only on the server and can access databases, APIs, etc.

```typescript
import { createServerFn } from "@tanstack/react-start";
import { authMiddleware } from "~/lib/auth/middleware";
import { db } from "~/lib/db";

// Protected server function
export const getItems = createServerFn()
  .middleware([authMiddleware])
  .handler(async ({ context }) => {
    const { user } = context; // user is typed and guaranteed to exist

    const items = await db.query.items.findMany({
      where: (items, { eq }) => eq(items.userId, user.id),
    });

    return items;
  });

// Public server function (no middleware)
export const getPublicData = createServerFn().handler(async () => {
  // Anyone can call this
  return { message: "Hello, world!" };
});
```

### Protected Routes

Use `beforeLoad` in route files to enforce authentication:

```typescript
// src/routes/(authenticated)/route.tsx
import { createFileRoute, redirect } from "@tanstack/react-router";
import { auth } from "~/lib/auth/auth";

export const Route = createFileRoute("/(authenticated)")({
  beforeLoad: async ({ context }) => {
    const session = await auth.api.getSession({
      headers: context.request.headers,
    });

    if (!session) {
      throw redirect({ to: "/login" });
    }

    return { user: session.user };
  },
});
```

### TanStack Query Integration

```typescript
import { queryOptions, useSuspenseQuery } from "@tanstack/react-query";
import { getItems } from "~/lib/items/functions"; // server function

// Create query options
export const itemsQueryOptions = () =>
  queryOptions({
    queryKey: ["items"],
    queryFn: () => getItems(),
  });

// Use in component
function ItemsList() {
  const { data: items } = useSuspenseQuery(itemsQueryOptions());

  return (
    <ul>
      {items.map((item) => (
        <li key={item.id}>{item.name}</li>
      ))}
    </ul>
  );
}
```

### Database Queries (Drizzle ORM)

```typescript
import { db } from "~/lib/db";
import { items } from "~/lib/db/schema";
import { eq } from "drizzle-orm";

// Insert
await db.insert(items).values({
  id: crypto.randomUUID(),
  name: "New Item",
  userId: user.id,
});

// Select
const userItems = await db
  .select()
  .from(items)
  .where(eq(items.userId, user.id));

// Update
await db
  .update(items)
  .set({ name: "Updated Name" })
  .where(eq(items.id, itemId));

// Delete
await db.delete(items).where(eq(items.id, itemId));

// Query builder (preferred)
const result = await db.query.items.findFirst({
  where: (items, { eq }) => eq(items.id, itemId),
  with: {
    user: true, // if relation is defined
  },
});
```

### Environment Variables

```typescript
// src/env/server.ts - Add new server env vars here
import { createEnv } from "@t3-oss/env-core";
import * as z from "zod";

export const env = createEnv({
  server: {
    DATABASE_URL: z.url(),
    BETTER_AUTH_SECRET: z.string().min(1),
    NEW_API_KEY: z.string().min(1), // Add your new vars
  },
  runtimeEnv: process.env,
});

// Usage
import { env } from "~/env/server";
const apiKey = env.NEW_API_KEY;
```

### Styling Patterns

Use Tailwind utility classes. For reusable styles, use the `cn()` utility:

```typescript
import { cn } from "~/lib/utils";

<div className={cn(
  "flex items-center gap-2", // base styles
  isActive && "bg-blue-500", // conditional
  className // allow overrides via props
)} />
```

---

## 8. Git Workflow

### Branch Naming
```
feature/short-description    # New features
fix/bug-description          # Bug fixes
refactor/what-changed        # Code refactoring
docs/what-changed            # Documentation
chore/what-changed           # Maintenance tasks
```

### Commit Style

Use conventional commits:

```
feat: add user profile page
fix: resolve session expiry bug
refactor: simplify auth middleware
docs: update setup instructions
chore: upgrade dependencies
style: format with prettier
test: add user creation tests
```

### Pull Request Process
1. Create a feature branch from `main`
2. Make changes and commit with descriptive messages
3. Push branch and create PR
4. Ensure CI passes (lint, type-check, build)
5. Request review
6. Merge after approval

---

## 9. Security Rules

### Authentication
- **ALWAYS** use `authMiddleware` for protected server functions
- **ALWAYS** check session in `beforeLoad` for protected routes
- **NEVER** trust client-side data without validation
- **NEVER** expose sensitive data in client components

### Input Validation
- **ALWAYS** validate user input with Zod schemas
- **ALWAYS** sanitize data before database insertion
- **NEVER** concatenate user input into SQL queries (use Drizzle's query builder)

### Environment Variables
- **ALWAYS** add new env vars to validation schemas (`src/env/`)
- **NEVER** commit `.env` file (it's gitignored)
- **NEVER** expose server env vars to client code

### Database
- **ALWAYS** use parameterized queries (Drizzle handles this)
- **ALWAYS** enforce foreign key constraints
- **NEVER** delete users without cascade cleanup

### OAuth
- **ALWAYS** validate OAuth redirect URIs
- **ALWAYS** use HTTPS in production
- **NEVER** commit OAuth secrets to git

### Example: Secure Server Function
```typescript
import { createServerFn } from "@tanstack/react-start";
import { authMiddleware } from "~/lib/auth/middleware";
import { db } from "~/lib/db";
import { items } from "~/lib/db/schema";
import { z } from "zod";

const createItemSchema = z.object({
  name: z.string().min(1).max(100),
  description: z.string().max(500).optional(),
});

export const createItem = createServerFn({ method: "POST" })
  .middleware([authMiddleware])
  .validator(createItemSchema)
  .handler(async ({ data, context }) => {
    const { user } = context;

    // Data is validated via Zod schema
    const newItem = await db.insert(items).values({
      id: crypto.randomUUID(),
      name: data.name,
      description: data.description,
      userId: user.id, // Authenticated user from context
    }).returning();

    return newItem[0];
  });
```

---

## 10. Current Status / Roadmap

### ✅ Completed
- [x] Project setup and dependencies installed
- [x] PostgreSQL database connected
- [x] Better Auth configured with email/password
- [x] Basic routing structure (home, login, signup, dashboard)
- [x] Protected routes implementation
- [x] Dark/light theme toggle
- [x] shadcn/ui component library integration
- [x] Environment variable validation
- [x] Database schema (Better Auth tables)

### 🚧 In Progress
- [ ] Database schema pushed to PostgreSQL (ready to run `pnpm db push`)
- [ ] Development server running

### 📋 Planned
- [ ] Add custom application features (TBD)
- [ ] OAuth provider setup (GitHub, Google)
- [ ] Email verification flow
- [ ] User profile page
- [ ] Additional database tables for app features
- [ ] API endpoints for custom features
- [ ] Testing setup (Vitest, Playwright)
- [ ] Production deployment (Vercel/other)

### 🎯 Current Focus
Setting up the development environment and preparing to build custom features.

---

## Notes for AI Agents

1. **Always check the schema** before modifying database code
2. **Use the middleware pattern** for protected server functions
3. **Follow the file-based routing structure** - don't create routes outside `src/routes/`
4. **Regenerate auth schema** after modifying `src/lib/auth/auth.ts` using `pnpm auth:generate`
5. **Run type-check** before committing: `pnpm check-types`
6. **Check `.env.example`** for required environment variables
7. **Read existing code patterns** before adding new features - consistency matters
8. **This is a TanStack Start project**, not Next.js - the patterns are different

---

**Last Updated:** 2025-12-05
**Project Version:** 0.1.0 (Initial Setup)
