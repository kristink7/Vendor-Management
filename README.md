# Vendor Club Approval Manager

Vendor Club Approval Manager is an internal operations workspace for managing vendor-club approvals across campuses and states. It brings request tracking, document readiness, role-based work queues, vendor follow-up, contracts, assignments, and Asana handoffs into one place.

**Live application:** [Vendor Club Approval Manager](https://lts-vendor-club-approvals.vertex-educa-5906.chatgpt.site)

## What it does

- Tracks vendor-club requests from intake through approval.
- Provides role-aware workspaces for Procurement, DSO, Accounting, Executive, and System Administrator users.
- Shows request status, uploaded documents, missing documents, start dates, owners, campuses, and states.
- Offers searchable and state-filtered views of all requests.
- Sorts the “All Requests” table by vendor and club, status, document counts, start date, or owner.
- Supports vendor-level rollups and campus assignment visibility.
- Provides an end-to-end workflow guide for the approval process.
- Connects request review and follow-up actions to Asana.
- Supports document uploads and vendor task updates through Asana actions.
- Generates Arizona vendor contracts from the included contract template.
- Provides an administrator area for invitations, access codes, and assignment-directory maintenance.

## Access and security

The application uses invitation-based access. Users sign in with an invited email address and access code; uninvited visitors are blocked. Sessions expire after twelve hours.

Access is role-aware:

- Executives can view and act across roles.
- Procurement, DSO, and Accounting users see the work relevant to their workflow stage.
- Campus-scoped users are limited to their assigned campuses for actions.
- System Administrators manage invitations, access codes, and assignment mappings.

Keep all secrets in the deployment environment. Never commit `.env` files, access codes, Asana tokens, or database credentials.

## Technology

- Next.js 16 with React 19 and TypeScript
- vinext and Vite for the application build
- Cloudflare Workers-compatible deployment
- Cloudflare D1 with Drizzle ORM for durable application data
- Asana API integration for request sync, actions, attachments, and task updates
- Pizzip for contract-template generation

## Local development

### Prerequisites

- Node.js `>=22.13.0`
- npm or pnpm
- An Asana access token for live request synchronization

### Setup

```bash
git clone https://github.com/kristink7/Vendor-Management.git
cd Vendor-Management
npm install
cp .env.example .env.local
```

Add the required values to `.env.local`:

```dotenv
ASANA_ACCESS_TOKEN=
AUTH_PEPPER=
BOOTSTRAP_ADMIN_CODE=
```

Then start the development server:

```bash
npm run dev
```

Open the local URL printed by the dev server. The `/access` route is the invitation sign-in screen.

## Commands

```bash
npm run dev          # Start local development
npm run build        # Create a production build
npm start            # Start the production server
npm test             # Build and run the rendered-output checks
npm run lint         # Run ESLint
npm run db:generate  # Generate Drizzle migrations
```

## Project structure

```text
app/
  access/             Invitation sign-in route
  api/                Auth, Asana, invitation, and directory endpoints
  components/         Approval manager UI and workflow views
  lib/                Auth, access, Asana, assignment, contract, and data logic
db/                   Drizzle database connection and schema
drizzle/              Database migrations
public/               Contract templates, user guides, and static assets
tests/                Rendered-output checks
worker/               Cloudflare Worker entry point
```

## Data and integrations

When `ASANA_ACCESS_TOKEN` is configured, the manager synchronizes request data from Asana without caching stale request results in the browser. Request actions, comments, document uploads, and related task updates are sent through the application API.

The application uses Cloudflare D1 for durable records and assignment data. Database schema changes should be accompanied by a generated Drizzle migration and verified before deployment.

## Deployment

The project is configured for Site-hosted Cloudflare deployment. Before publishing:

1. Install dependencies with the repository’s lockfile.
2. Run `npm run build` and `npm test`.
3. Configure runtime secrets in the hosting environment.
4. Apply any required D1 migrations.
5. Publish the built Worker and verify the protected `/access` route.

## Operational notes

- Use the System Administrator area to issue or revoke invitations.
- Keep assignment mappings current so campus requests route to the right contact.
- Review missing-document flags before advancing a request.
- Treat the live workspace as operational data; avoid using production credentials in local development.

## License

This repository is maintained for internal operational use. Add the organization’s approved license or usage terms here before making the project public.
