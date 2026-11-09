# General Inn Management System

A React + Express + MySQL inn operations system. It includes a dashboard, room management, guest records, employee accounts, inventory, transactions, sales, damage charges, and receipts.

The project was originally configured for one inn. This version makes the displayed inn identity, CORS origins, and sample hourly room charges configurable for another inn.

## Requirements

- Node.js (LTS recommended)
- MySQL
- The existing project database schema

## Setup

### 1. Database

Create or import your MySQL database using the database schema supplied by your project/team. The application currently expects the existing tables and columns used by the backend routes. Set the database connection values in `backend/server/.env`.

### 2. Backend

```bash
cd backend/server
npm install
```

Copy `.env.example` to `.env`, then update the database credentials and room rates.

Start the API:

```bash
npm run dev
```

The API runs on port 5000 by default.

### 3. Frontend

In the project root:

```bash
npm install
```

Copy the root `.env.example` to `.env`. Change the inn name, tagline, currency, locale, and rates. The frontend rate values must match the backend rate values.

Start the frontend:

```bash
npm start
```

Open `http://localhost:3000`.

## Configure it for another inn

Update the root `.env`:

- `REACT_APP_INN_NAME`: the business name shown in the interface
- `REACT_APP_INN_TAGLINE`: the login-page description
- `REACT_APP_CURRENCY`: ISO 4217 currency code, such as `PHP`, `USD`, or `SGD`
- `REACT_APP_LOCALE`: display locale, such as `en-PH` or `en-US`

Update both root `.env` and `backend/server/.env` rate values to the inn's pricing policy:

- `RATE_UP_TO_3_HOURS` / `REACT_APP_RATE_UP_TO_3_HOURS`
- `RATE_UP_TO_8_HOURS` / `REACT_APP_RATE_UP_TO_8_HOURS`
- `RATE_UP_TO_24_HOURS` / `REACT_APP_RATE_UP_TO_24_HOURS`
- `RATE_EACH_EXTRA_HOUR` / `REACT_APP_RATE_EACH_EXTRA_HOUR`

Restart the frontend after changing its `.env`; React environment variables are read at build/start time.

## Deployment notes

- Set `CLIENT_ORIGINS` in the backend environment to the exact deployed frontend origin(s), separated by commas.
- Set all database credentials as hosting-provider environment variables. Never commit `.env` files.
- Build the frontend with `npm run build`.
- Deploy the backend and frontend with their respective environment variables.
- Use HTTPS in production.
- Back up the database regularly.

## Important scope note

This update does not automatically create a separate database tenant for every inn. Each inn should use its own database (or a future tenant-aware database design) and its own environment configuration. Existing room types, room records, inventory, employee accounts, and historical transactions remain database-managed and must be initialized for each deployment.

The original schema was not included in the uploaded project, so this package keeps the existing database structure and API contracts intact rather than inventing a new schema.
