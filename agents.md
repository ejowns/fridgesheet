# Fridgesheet - AI Agent Context Guide

This document provides AI coding agents with essential context about the fridgesheet project.

---

## 1. Project Overview

**What it does:** Fridgesheet is a web application that lets parents create QR codes containing emergency information for babysitters, caretakers, and grandparents. Scanning the QR code displays all critical info: allergies, emergency contacts, poison control, hospital directions, house rules, and more.

**Who it's for:** Parents with young children who use babysitters or leave kids with caretakers and want to have all essential information in one place.

**Core value prop:** "Every fridge needs a Fridgesheet" - one sheet of emergency info, always updated, accessible via QR scan.

**Domain:** fridgesheet.com

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

### Design System

**Brand Colors:**
- Primary: `#14B8A6` (Teal) - Main brand color for CTAs, links, important UI elements
- Primary Dark: `#0F766E` - Hover states, emphasis
- Primary Light: `#CCFBF1` - Backgrounds, subtle highlights
- Secondary: `#F97316` (Orange) - Accent color for secondary actions
- Secondary Light: `#FFEDD5` - Secondary backgrounds

**UI Colors:**
- Background: `#FAFAF9` - Main page background
- Surface: `#FFFFFF` - Cards, modals, elevated surfaces
- Dark: `#1E293B` - Text, headers
- Muted: `#64748B` - Secondary text, hints

**Semantic Colors:**
- Danger: `#EF4444` (Red) - Errors, allergies, critical warnings
- Danger Light: `#FEE2E2` - Error backgrounds
- Success: `#22C55E` (Green) - Success messages, confirmations
- Success Light: `#DCFCE7` - Success backgrounds
- Warning: `#EAB308` (Yellow) - Warnings, important notices
- Warning Light: `#FEF9C3` - Warning backgrounds

**Typography:**
- Logo Font: Teko (bold, condensed sans-serif for branding)
- Body Font: Nunito (friendly, rounded sans-serif)

**Usage in Code:**
```tsx
// Tailwind CSS v4 classes
<button className="bg-brand-primary text-white hover:bg-brand-primary-dark">
  Get Started
</button>

<div className="text-danger bg-danger-light">
  Allergy Warning
</div>

<h1 className="font-logo text-4xl">Fridgesheet</h1>
<p className="font-sans">Body text uses Nunito</p>
```

---

## 3. Project Structure

```
fridgesheet/
├── src/
│   ├── routes/                         # File-based routing (TanStack Router)
│   │   ├── __root.tsx                  # Root layout, wraps all routes
│   │   ├── index.tsx                   # Landing page (marketing, static)
│   │   ├── pricing.tsx                 # Pricing page (static)
│   │   ├── signin.tsx                  # Auth: sign in page
│   │   ├── signup.tsx                  # Auth: sign up page
│   │   ├── dashboard.tsx               # Protected: main dashboard
│   │   ├── dashboard.edit.tsx          # Protected: edit emergency info form
│   │   ├── dashboard.download.tsx      # Protected: download/print QR code
│   │   ├── qr.$shortcode.tsx           # Public: emergency info display (NO AUTH)
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
- **Routes:** Use file-based routing. Dot notation for nested paths: `dashboard.edit.tsx` = `/dashboard/edit`
- **Components:** PascalCase for component files and exports
- **Utilities:** camelCase for functions and variables
- **Database:** snake_case (enforced by Drizzle config)
- **CSS:** Tailwind utility classes, kebab-case for custom classes

### Route Specifications

#### Public Routes (No Authentication)
- `/` - Landing page with hero, features, pricing preview, CTA
- `/pricing` - Pricing tiers and feature comparison
- `/qr/$shortcode` - **CRITICAL:** Public emergency info display (babysitter view)
  - Example: `/qr/XK9P2A`
  - Fast loading, mobile-first
  - Large tappable phone numbers (click-to-call)
  - Display order: Child info → Parents → Emergency contacts → Medical → Numbers

#### Auth Routes
- `/signin` - Sign in with email/password
- `/signup` - Create new account

#### Protected Routes (Require Authentication)
- `/dashboard` - Main hub
  - If no profile: "Create your first Fridgesheet" CTA
  - If profile exists: Summary card with edit/download buttons
  - MVP: One Fridgesheet per account
- `/dashboard/edit` - Multi-section form for emergency info
  - Sections: Child Info, Parents, Emergency Contacts, Medical, Important Numbers, Home Info
- `/dashboard/download` - View and download QR code
  - Display QR code large
  - Download as PNG button
  - Print instructions

#### Route Protection Pattern
```typescript
// dashboard.tsx example
import { createFileRoute, redirect } from '@tanstack/react-router'
import { authQueryOptions } from '~/lib/auth/queries'

export const Route = createFileRoute('/dashboard')({
  beforeLoad: async ({ context }) => {
    const session = await context.queryClient.ensureQueryData(authQueryOptions())

    if (!session) {
      throw redirect({ to: '/signin' })
    }

    return { user: session.user }
  },
  component: Dashboard,
})
```

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

### Detailed Feature Specifications

#### 1. Landing Page (`/`)
**Purpose:** Convert visitors to signups

**Sections:**
- **Hero:** "Every fridge needs a Fridgesheet" with CTA button
- **How it works:** 3 steps (Sign up → Fill info → Get QR code)
- **Features/benefits:** Emergency info always updated, accessible to caretakers
- **Pricing preview:** Free tier highlighted
- **CTA:** "Get Started Free" button

**Design:** Clean, trustworthy, parent-friendly aesthetic

---

#### 2. Pricing Page (`/pricing`)
**Purpose:** Show pricing tiers and features

**MVP Pricing:**
- **Free Tier:** 1 Fridgesheet, basic features
- **Future:** Premium tiers for multiple kids, photo uploads, analytics

---

#### 3. Authentication (`/signin`, `/signup`)
**Provider:** Better Auth (already configured)
**Features:**
- Email + password signup/signin
- Session management
- Protected route middleware

---

#### 4. Dashboard (`/dashboard`)
**Purpose:** Main hub after login

**States:**
1. **No profile created yet:**
   - Show: "Create your first Fridgesheet" CTA button
   - Action: Navigate to `/dashboard/edit`

2. **Profile exists:**
   - Show summary card with:
     - Child name (if filled)
     - QR code thumbnail
     - "Edit Info" button → `/dashboard/edit`
     - "Download QR" button → `/dashboard/download`

**MVP Constraint:** One Fridgesheet per account

---

#### 5. Edit Form (`/dashboard/edit`)
**Purpose:** Create/update emergency information

**Form Sections (Multi-step or Single Page):**

1. **Child Info**
   - First name (text)
   - Birthdate (date picker)
   - Photo upload (optional, future: Cloudinary/S3)

2. **Parents**
   - Parent 1: Name + Phone
   - Parent 2: Name + Phone

3. **Emergency Contacts** (2 contacts)
   - Name
   - Relationship (e.g., "grandma", "neighbor")
   - Phone number

4. **Medical Information**
   - Allergies (textarea)
   - Medications (textarea)
   - Medical conditions (textarea)
   - Pediatrician name + phone

5. **Important Numbers**
   - Poison Control (pre-filled: 1-800-222-1222)

6. **Home Info**
   - Home address
   - Nearest hospital/urgent care
   - WiFi password

**UX:**
- Auto-save drafts (prevent data loss)
- Clear validation messages
- Mobile-friendly inputs
- Save button at bottom

---

#### 6. QR Code Generation
**Implementation:**
- Library: `qrcode` or `qrcode.react`
- Generate 6-character alphanumeric short code (e.g., "XK9P2A")
- QR encodes URL: `https://fridgesheet.com/qr/XK9P2A`
- Store short code in `short_codes` table linked to profile

**When to generate:**
- Automatically when profile is first saved
- Regenerate option (invalidates old code)

---

#### 7. Public Info Page (`/qr/$shortcode`)
**Purpose:** What babysitters/caretakers see after scanning QR code

**Requirements:**
- **NO authentication required** (critical!)
- Fast loading (minimal JS)
- Mobile-first design
- Large, tappable phone numbers (click-to-call via `tel:` links)

**Display Order:**
1. Child name + photo (if available)
2. **Parent phone numbers** (prominent, large buttons)
3. Emergency contacts (name, relationship, phone)
4. **Medical info** (allergies highlighted in warning color)
5. Medications & medical conditions
6. Pediatrician contact
7. Poison Control number (1-800-222-1222)
8. Home address
9. Nearest hospital
10. WiFi password (last, less critical)

**Design:**
- High contrast for readability
- Clear sections
- Emergency numbers in red/orange
- Print-friendly styles

---

#### 8. Download Page (`/dashboard/download`)
**Purpose:** View and download QR code for printing

**Features:**
- Display QR code large (300x300px or bigger)
- "Download as PNG" button (use canvas.toDataURL())
- Print instructions: "Print this page and post on your fridge"
- Show the URL: `fridgesheet.com/qr/XK9P2A`

**Future:**
- PDF generation with formatted layout
- Multiple design templates

---

### Future Enhancements (Post-MVP)
- Multiple children per account
- Multiple Fridgesheets per user
- Photo uploads for children
- Access analytics (view count)
- Password-protected sheets
- Link expiration for temporary caretakers
- Multi-language support (Spanish, etc.)
- Custom print templates
- Mobile app
- Voice notes
- Calendar integration

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

---

### Planned Fridgesheet Tables

#### MVP Schema (Simplified - One Profile Per User)

For MVP, we're using a simplified schema with one `profile` table per user that contains all emergency info. This avoids complex relationships and makes the initial build faster.

#### `profiles`
```typescript
{
  id: uuid (PK)
  userId: uuid (FK -> user.id, CASCADE delete, UNIQUE - one profile per user in MVP)

  // Child info
  childFirstName: text (nullable)
  childBirthdate: date (nullable)
  childPhotoUrl: text (nullable)

  // Parents
  parent1Name: text (nullable)
  parent1Phone: text (nullable)
  parent2Name: text (nullable)
  parent2Phone: text (nullable)

  // Emergency contacts
  emergency1Name: text (nullable)
  emergency1Relationship: text (nullable)
  emergency1Phone: text (nullable)
  emergency2Name: text (nullable)
  emergency2Relationship: text (nullable)
  emergency2Phone: text (nullable)

  // Medical
  allergies: text (nullable)
  medications: text (nullable)
  medicalConditions: text (nullable)
  pediatricianName: text (nullable)
  pediatricianPhone: text (nullable)

  // Home info
  homeAddress: text (nullable)
  nearestHospital: text (nullable)
  wifiPassword: text (nullable)

  // Timestamps
  createdAt: timestamp (default: now)
  updatedAt: timestamp (auto-update)
}
// Indexes: userId (unique)
// Relationships: userId -> user.id (CASCADE delete)
```

#### `short_codes`
```typescript
{
  id: uuid (PK)
  code: varchar(8) (unique, alphanumeric, e.g., "XK9P2A")
  profileId: uuid (FK -> profiles.id, CASCADE delete)
  createdAt: timestamp (default: now)
}
// Indexes: code (unique), profileId
// Relationships: profileId -> profiles.id (CASCADE delete)
// Note: code is what appears in QR code URL: /qr/XK9P2A
```

### MVP Relationships
```
user (1) -> (1) profile (one profile per user in MVP)
profile (1) -> (1) short_code
```

### Future Schema (Post-MVP)
After MVP validation, we can migrate to a more normalized schema to support:
- Multiple children per profile
- Multiple Fridgesheets per user
- Separate tables for allergies, medications, emergency contacts
- Photo galleries
- Access analytics

The future schema would include separate tables for: `fridgesheets`, `children`, `allergies`, `medications`, `emergency_contacts`, `quick_dial_numbers`, `house_info`, and `access_logs`.

---

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

// PROTECTED: Get user's profile (requires authentication)
export const getProfile = createServerFn({ method: "GET" })
  .middleware([authMiddleware])
  .handler(async ({ context }) => {
    const { user } = context; // user is typed and guaranteed to exist

    const profile = await db.query.profiles.findFirst({
      where: (profiles, { eq }) => eq(profiles.userId, user.id),
    });

    return profile;
  });

// PROTECTED: Update profile (requires authentication)
export const updateProfile = createServerFn({ method: "POST" })
  .middleware([authMiddleware])
  .handler(async ({ context, data }) => {
    const { user } = context;

    // IMPORTANT: Always verify ownership!
    const existingProfile = await db.query.profiles.findFirst({
      where: (profiles, { eq }) => eq(profiles.userId, user.id),
    });

    if (!existingProfile) {
      // Create new profile
      const [newProfile] = await db.insert(profiles).values({
        userId: user.id,
        ...data,
      }).returning();

      return newProfile;
    }

    // Update existing profile
    const [updated] = await db
      .update(profiles)
      .set({ ...data, updatedAt: new Date() })
      .where(eq(profiles.userId, user.id))
      .returning();

    return updated;
  });

// PUBLIC: Get profile by short code (NO authentication required!)
export const getProfileByShortCode = createServerFn({ method: "GET" })
  .handler(async ({ data }) => {
    const { code } = data;

    // Find short code
    const shortCode = await db.query.shortCodes.findFirst({
      where: (shortCodes, { eq }) => eq(shortCodes.code, code),
      with: {
        profile: true, // Get associated profile
      },
    });

    if (!shortCode) {
      throw new Error("Fridgesheet not found");
    }

    return shortCode.profile;
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

### Fridgesheet-Specific Security
- **Public Fridgesheet Pages:**
  - NO authentication required for viewing (by design)
  - Rate limit QR code scans to prevent abuse
  - Consider optional password protection for sensitive info
  - Log access attempts for analytics (optional)

- **Sensitive Information:**
  - WiFi passwords, security codes, and medical info should be clearly marked
  - Consider encryption at rest for highly sensitive fields
  - Warn users about what info they're making public
  - Provide option to hide certain sections from public view

- **QR Code & Slug Security:**
  - Use UUIDs or cryptographically random slugs (not sequential IDs)
  - Implement QR code regeneration to invalidate old links
  - Consider short-lived access tokens for temporary caretakers
  - Monitor for brute-force slug guessing attempts

- **User Data Ownership:**
  - Only allow users to edit/delete their own Fridgesheets
  - Cascade delete all child data when Fridgesheet is deleted
  - Provide data export functionality (GDPR compliance)
  - Clear data retention policy

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
- [x] Project setup and dependencies installed (pnpm)
- [x] PostgreSQL database connected (local: fridgesheet db)
- [x] Better Auth configured with email/password
- [x] Basic routing structure (home, login, signup, dashboard)
- [x] Protected routes implementation
- [x] Dark/light theme toggle
- [x] shadcn/ui component library integration
- [x] Environment variable validation (.env configured)
- [x] Database schema pushed (Better Auth tables created)
- [x] Development server running at localhost:3000

### 🚧 In Progress
- [ ] Design Fridgesheet database schema
- [ ] Plan user flows and wireframes
- [ ] Define MVP feature set

### 📋 Phase 1: Core Fridgesheet Features (MVP)
- [ ] Create Fridgesheet database tables (fridgesheet, child, allergy, emergency_contact, etc.)
- [ ] Build Fridgesheet creation form (multi-step wizard)
- [ ] Implement QR code generation (using qrcode library)
- [ ] Create public view page (no-auth, read-only display)
- [ ] Dashboard: list user's Fridgesheets
- [ ] Basic edit/update functionality
- [ ] Printable PDF generation
- [ ] Mobile-responsive design for public view

### 📋 Phase 2: Enhanced Features
- [ ] Multiple children per Fridgesheet
- [ ] Photo upload for children (cloudinary/S3)
- [ ] Emergency contact management (add/edit/delete)
- [ ] Medication tracking
- [ ] House info section (WiFi, security codes, etc.)
- [ ] Quick dial numbers with click-to-call
- [ ] QR code regeneration (invalidate old links)

### 📋 Phase 3: Advanced Features
- [ ] Access analytics (view count tracking)
- [ ] Link expiration for temporary caretakers
- [ ] Password protection option
- [ ] Multi-language support
- [ ] Print template designs
- [ ] Email sharing functionality
- [ ] OAuth provider setup (GitHub, Google)

### 📋 Phase 4: Production
- [ ] SEO optimization
- [ ] Testing setup (Vitest, Playwright)
- [ ] Production deployment (Vercel)
- [ ] Custom domain setup (fridgesheet.com)
- [ ] Analytics (Plausible/Umami)
- [ ] Error tracking (Sentry)
- [ ] User feedback system

### 🎯 Current Focus
Phase 1 Planning - Defining the MVP database schema and core features for Fridgesheet emergency info system.

---

## Notes for AI Agents

### General Development Guidelines
1. **Always check the schema** before modifying database code
2. **Use the middleware pattern** for protected server functions
3. **Follow the file-based routing structure** - don't create routes outside `src/routes/`
4. **Regenerate auth schema** after modifying `src/lib/auth/auth.ts` using `pnpm auth:generate`
5. **Run type-check** before committing: `pnpm check-types`
6. **Check `.env.example`** for required environment variables
7. **Read existing code patterns** before adding new features - consistency matters
8. **This is a TanStack Start project**, not Next.js - the patterns are different

### Fridgesheet-Specific Guidelines

1. **Public vs. Protected Routes:**
   - `/dashboard*` → PROTECTED (require auth)
   - `/qr/$shortcode` → PUBLIC (NO auth, critical!)
   - Use TanStack Router's `beforeLoad` hook for route protection
   - Route naming: `dashboard.edit.tsx` = `/dashboard/edit` (dot notation)

2. **MVP Constraints:**
   - One profile per user (enforced by unique constraint on userId)
   - Simplified schema (no separate tables for children/contacts)
   - All fields nullable (users can save partial info)
   - 6-character alphanumeric short codes (e.g., "XK9P2A")

3. **Server Function Patterns:**
   - **ALWAYS** use `authMiddleware` for protected functions (getProfile, updateProfile)
   - **NEVER** use middleware for public QR view (getProfileByShortCode)
   - **ALWAYS** verify user ownership before updates: `eq(profiles.userId, user.id)`
   - Use Drizzle's query builder (NOT raw SQL)

4. **QR Code Implementation:**
   - Library: `qrcode` or `qrcode.react`
   - Generate 6-char code: Use crypto random (not sequential IDs)
   - Example function: `generateShortCode()` → "XK9P2A"
   - QR encodes: `https://fridgesheet.com/qr/XK9P2A`
   - Store in `short_codes` table linked to profileId

5. **Public View Page (`/qr/$shortcode`) Requirements:**
   - **CRITICAL:** NO authentication
   - Mobile-first (babysitters use phones)
   - Large tap targets for phone numbers
   - Use `tel:` links for click-to-call
   - Display order: Child → Parents → Contacts → Medical → Numbers
   - Highlight allergies in warning color (red/orange)
   - Fast load time (minimal JS, no heavy frameworks on public page)

6. **Form Design:**
   - Single-page form OR multi-step wizard (decide in implementation)
   - All fields optional (users can save partial data)
   - Pre-fill Poison Control: "1-800-222-1222"
   - Phone input formatting (optional enhancement)
   - Date picker for child birthdate
   - Textarea for allergies, medications, medical conditions

7. **Data Ownership & Security:**
   - Users can ONLY edit their own profile
   - Verify `userId` matches session user in all update operations
   - No user can access another user's profile data
   - Public short codes are intentionally public (by design)
   - Consider rate limiting public QR page views (future)

8. **User Experience:**
   - Dashboard shows "Create Fridgesheet" if no profile
   - Dashboard shows QR preview + edit/download buttons if profile exists
   - Clear CTAs: "Get Started Free" on landing
   - Toast notifications for save success/errors
   - Mobile-responsive (parents AND babysitters use mobile)

---

**Last Updated:** 2025-12-05
**Project Version:** 0.1.0 (Initial Setup - Auth & Database Ready)
**Next Milestone:** Phase 1 MVP - Core Fridgesheet Features
