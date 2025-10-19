# Project Overview

This is a [T3 Stack](https://create.t3.gg/) project bootstrapped with `create-t3-app`. It is a full-stack application that uses [Next.js](https://nextjs.org) for the frontend and backend, [tRPC](https://trpc.io) for typesafe API routes, [Prisma](https://prisma.io) as the ORM for database access, and [NextAuth.js](https://next-auth.js.org) for authentication. The database is PostgreSQL, and the styling is done with [Tailwind CSS](https://tailwindcss.com).

The project is structured as follows:

- `src/app/`: Contains the main application logic, including pages and components.
- `src/server/`: Contains the server-side logic, including the tRPC API, database schema, and authentication configuration.
- `prisma/`: Contains the database schema and migrations.

# Building and Running

**Development:**

To run the application in development mode, use the following command:

```bash
npm run dev
```

This will start the Next.js development server on `http://localhost:3000`.

**Building:**

To build the application for production, use the following command:

```bash
npm run build
```

This will create an optimized production build in the `.next` directory.

**Running:**

To run the production build, use the following command:

```bash
npm run start
```

This will start the Next.js production server on `http://localhost:3000`.

**Database:**

The project uses Prisma to manage the database. The following commands are available:

- `npm run db:generate`: Generate the Prisma client.
- `npm run db:migrate`: Run database migrations.
- `npm run db:push`: Push the Prisma schema to the database.
- `npm run db:studio`: Open the Prisma Studio.

Database server is PostgreSQL 17

# Development Conventions

**Coding Style:**

The project uses [Prettier](https://prettier.io) for code formatting and [ESLint](https://eslint.org) for linting. The configuration for these tools can be found in `prettier.config.js` and `eslint.config.js`, respectively.

**Testing:**

  npx playwright test
    Runs the end-to-end tests.

  npx playwright test --ui
    Starts the interactive UI mode.

  npx playwright test --project=chromium
    Runs the tests only on Desktop Chrome.

  npx playwright test example
    Runs the tests in a specific file.

  npx playwright test --debug
    Runs the tests in debug mode.

  npx playwright codegen
    Auto generate tests with Codegen.

We suggest that you begin by typing:

    npx playwright test

And check out the following files:
  - ./tests/example.spec.ts - Example end-to-end test
  - ./playwright.config.ts - Playwright Test configuration

Visit https://playwright.dev/docs/intro for more information.
