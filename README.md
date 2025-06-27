# SAAS Turborepo Template

A modern, full-stack SAAS starter template built with Turborepo, featuring Next.js, Better Auth, Prisma, and Shadcn UI. This monorepo template provides a solid foundation for building scalable SAAS applications.

## 🚀 Features

- **Monorepo Architecture**: Built with Turborepo for optimal development experience
- **Modern Stack**:
  - Next.js for the frontend
  - Better Auth for authentication
  - Prisma for database management
  - Shadcn UI for beautiful, accessible components
- **Type Safety**: Full TypeScript support
- **Code Quality**: ESLint and Prettier for consistent code style
- **Package Management**: PNPM for efficient dependency management

## 📦 Project Structure

```
saas-turbo/
├── apps/
│   ├── app/      # Main application
│   ├── studio/   # Admin dashboard
│   └── web/      # Marketing website
├── packages/     # Shared packages and configurations
└── ...
```

## 🛠️ Prerequisites

- Node.js >= 20
- PNPM >= 10.4.1

## 🚀 Getting Started

1. **Clone the repository**
   ```bash
   git clone 
   cd saas-turbo
   ```

2. **Install dependencies**
   ```bash
   pnpm install
   ```

3. **Set up environment variables**
   - Copy `.env.example` to `.env` in each app directory
   - Fill in the required environment variables

4. **Start the development server**
   ```bash
   pnpm dev
   ```

## 📝 Available Scripts

- `pnpm dev` - Start development server
- `pnpm build` - Build all applications and packages
- `pnpm lint` - Run ESLint
- `pnpm format` - Format code with Prettier

## Usage Guide

1. **packages/auth**
   - Exports:
      - `@repo/auth/client` which is a better auth client exporting `signIn`, `signOut`, `signUp`, `useSession`, `resetPassword`, `forgetPassword`
      - `@repo/auth/server` which exports `better-auth` instance to be used in the server component.
      - `@repo/auth/actions` which has augmented functions which check for the users in the database and return either the error message in case the form data is violated. 
   - Setup the env variables in the respective app directory and then run `pnpm run auth:db:generate` to generate User and Account Schema in your database `@repo/db`, make sure you migrate the database after that.
   - exports  `better-auth/client` and `better-auth/server`
   - `client` exports functions `signIn, signOut, signUp, useSession, resetPassword, forgetPassword`
   - The `apps/app/lib/actions.ts` file exports two functions to signin, signup the user from the FormData.
   - Reset Password: in `auth/setver.ts`, for email and password login, reset link is sent using the send `sendResetPasswordEmail` defined in the `@repo/email`


2. **packages/db**
   - exprots prisma client as prisma

## 🔒 Adding Protected Routes in a New Next.js App

To add protected routes and use Better Auth in a new Next.js app in this monorepo, follow these steps:

### 1. Create a (protected) Route Group
- In your new app (e.g., `apps/portal`), create a folder: `apps/portal/app/(protected)`
- Add your protected pages inside this folder, e.g.:
  ```
  apps/portal/app/(protected)/dashboard/page.tsx
  apps/portal/app/(protected)/settings/page.tsx
  ```

### 2. Protect the Routes
- In each protected page, use the following pattern to require authentication and get user details:
  ```tsx
  import { getSession } from "@repo/auth/actions";
  import { headers } from "next/headers";

  export default async function DashboardPage() {
    const session = await getSession(await headers());

    if (!session) {
      // Optionally, redirect or render a not-authenticated component
      return <div>You must be signed in to view this page.</div>;
    }

    // Access user details via session.user
    return <div>Welcome, {session.user.email}!</div>;
  }
  ```
- `getSession(await headers())` fetches the current user session using the request headers.

### 3. Middleware (Optional but Recommended)
- To automatically redirect unauthenticated users, use the middleware from your `@repo/auth` package.
- In your new app, add or update `middleware.ts`:
  ```ts
  export { authMiddleware as middleware } from "@repo/auth/middleware";
  ```
- This will protect all routes you specify in the middleware logic (e.g., those under `/dashboard`).

### 4. Getting User Details
- After calling `getSession`, you can access user info:
  - `session.user.id`
  - `session.user.email`
  - `session.user.name`
  - ...and any other fields you store.

### 5. Example: Protecting All Routes in (protected)
- You can update the middleware logic in `@repo/auth/middleware.ts` to match any route under `/protected`:
  ```ts
  const isProtectedRoute = (request: NextRequest) => {
    return request.nextUrl.pathname.startsWith("/protected");
  };
  ```
- Or, use a more flexible check as needed.








