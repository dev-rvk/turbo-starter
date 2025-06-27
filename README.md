# SAAS Turborepo Template

A modern, full-stack SAAS starter template built with Turborepo, featuring Next.js, Better Auth, Prisma, and Shadcn UI. This monorepo provides a solid foundation for scalable SAAS applications.

## 🚀 Features

- **Monorepo** with Turborepo
- **Next.js** frontend
- **Better Auth** for authentication
- **Prisma** for database
- **Shadcn UI** for components
- **TypeScript** throughout
- **ESLint** and **Prettier** for code quality
- **Bun** for fast, modern package management

## 📦 Project Structure

```
turbo-starter/
├── apps/
│ ├── app/ # Main application
│ ├── studio/ # Admin dashboard
│ └── web/ # Marketing website
├── packages/ # Shared packages and configurations
└── ...
```


## 🛠️ Prerequisites

- [Bun](https://bun.sh/) >= 1.0.0
- Node.js >= 20 (for some tools)

## 🚀 Getting Started

1. **Clone the repository**
   ```bash
   git clone <your-repo-url>
   cd turbo-starter
   ```

2. **Install dependencies**
   ```bash
   bun install
   ```

3. **Set up environment variables**
   - Copy `.env.example` to `.env` in each app directory
   - Fill in the required environment variables

4. **Start the development server**
   ```bash
   bun run dev
   ```

## 📝 Scripts

- `bun run dev` — Start development server
- `bun run build` — Build all apps and packages
- `bun run lint` — Run ESLint
- `bun run format` — Format code with Prettier

## Usage Guide

### Auth Package (`packages/auth`)
- Exports:
  - `@repo/auth/client`: Better Auth client (`signIn`, `signOut`, `signUp`, `useSession`, `resetPassword`, `forgetPassword`)
  - `@repo/auth/server`: Better Auth instance for server components
  - `@repo/auth/actions`: Functions for user actions and validation
- Set up env variables in each app and run:
  ```bash
  bun run auth:db:generate
  ```
  to generate User and Account schema in `@repo/db`. Migrate your database after that.

### DB Package (`packages/db`)
- Exports Prisma client as `prisma`

## 🔒 Protecting Routes in Next.js

1. **Create a (protected) Route Group**
   - Example: `apps/app/app/(protected)/dashboard/page.tsx`

2. **Require Authentication in Pages**
   ```tsx
   import { getSession } from "@repo/auth/actions";
   import { headers } from "next/headers";

   export default async function DashboardPage() {
     const session = await getSession(await headers());
     if (!session) return <div>You must be signed in to view this page.</div>;
     return <div>Welcome, {session.user.email}!</div>;
   }
   ```

3. **Middleware (Optional)**
   - In your app, add or update `middleware.ts`:
     ```ts
     export { authMiddleware as middleware } from "@repo/auth/middleware";
     ```
