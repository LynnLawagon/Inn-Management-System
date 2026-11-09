# General Inn Management System

A web-based inn operations system built with **React**, **Node.js/Express**, and **MySQL/MariaDB**. The interface uses a neutral, professional layout and configurable inn branding so the same application can be set up for different independent inns.

## Features

- Dashboard with room, inventory, damage, and sales summaries
- Room and room-status management
- Guest records
- Check-in and check-out transactions
- Inventory tracking
- Sales and guest purchases
- Guest and employee damage records
- Employee accounts and role-protected API routes
- Receipt and transaction utilities
- Configurable inn name, tagline, currency display, and hourly rates

> Available features depend on the database being initialized with the tables and columns expected by the backend API.

## Technology

| Part | Technology |
|---|---|
| Frontend | React 19, React Router, Chart.js |
| Backend | Node.js, Express |
| Database | MySQL 8+ or compatible MariaDB |
| Database driver | mysql2 |
| Authentication | JSON Web Tokens and bcrypt |

## Requirements

- Windows, macOS, or Linux
- Node.js LTS and npm
- Laragon (Windows) or another MySQL/MariaDB server
- A database manager such as HeidiSQL or phpMyAdmin

## Local setup with Laragon

### 1. Start the database

1. Open **Laragon**.
2. Click **Start All** (or start MySQL/MariaDB from the Laragon menu).
3. Open HeidiSQL from Laragon's Menu → MySQL → HeidiSQL, or open phpMyAdmin if it is installed.
4. Connect to the local database server. Typical Laragon defaults are host `127.0.0.1`, port `3306`, user `root`, and a blank password. Use the credentials configured on your own Laragon installation if they differ.
5. Create/import the database schema supplied for this project. The backend database name must match `DB_NAME` in the backend `.env` file.

### 2. Configure the backend

Open a terminal in the project directory and run:

```bash
cd backend/server
npm install
```

Create `backend/server/.env` by copying `backend/server/.env.example`, then set your local database credentials:

```env
PORT=5000
DB_HOST=127.0.0.1
DB_USER=root
DB_PASSWORD=
DB_NAME=inn_management

RATE_UP_TO_3_HOURS=80
RATE_UP_TO_8_HOURS=150
RATE_UP_TO_24_HOURS=200
RATE_EACH_EXTRA_HOUR=20

CLIENT_ORIGINS=http://localhost:3000,http://127.0.0.1:3000
```

Change the database name, password, and rates to match your setup and pricing. Do not commit `.env` files or share database passwords.

Start the API:

```bash
npm run dev
```

The API listens on `http://localhost:5000` by default. Check `http://localhost:5000/api/health` to confirm that the Express server responds. This health endpoint confirms the API is running; it does **not** by itself prove that MySQL is connected.

### 3. Configure and start the frontend

Open a **second terminal** at the project root (the folder containing the root `package.json`):

```bash
npm install
```

Create the root `.env` by copying `.env.example` and update the inn details:

```env
REACT_APP_INN_NAME=Your Inn Name
REACT_APP_INN_TAGLINE=Inn Management System

REACT_APP_RATE_UP_TO_3_HOURS=80
REACT_APP_RATE_UP_TO_8_HOURS=150
REACT_APP_RATE_UP_TO_24_HOURS=200
REACT_APP_RATE_EACH_EXTRA_HOUR=20
```

Keep the frontend rates in sync with the backend rates. Then start React:

```bash
npm start
```

Open `http://localhost:3000` in your browser. Keep both terminals running while using the application:

- Terminal 1: backend API (`backend/server`, port 5000)
- Terminal 2: React frontend (project root, port 3000)

The frontend development server proxies `/api` requests to `http://localhost:5000` using the `proxy` setting in the root `package.json`.

## Configuration for another inn

For each inn installation, configure the following values:

- `REACT_APP_INN_NAME`: inn/business name displayed in the interface
- `REACT_APP_INN_TAGLINE`: short description shown on the login page
- `REACT_APP_RATE_UP_TO_3_HOURS`: frontend rate for stays up to 3 hours
- `REACT_APP_RATE_UP_TO_8_HOURS`: frontend rate for stays up to 8 hours
- `REACT_APP_RATE_UP_TO_24_HOURS`: frontend rate for stays up to 24 hours
- `REACT_APP_RATE_EACH_EXTRA_HOUR`: frontend rate for each additional hour
- Backend `RATE_*` values: corresponding server-side pricing values
- `DB_*` values: that inn's database connection

Restart the frontend after editing its `.env`; Create React App reads environment variables when the development server starts or when the production build is created.

## Project structure

```text
General_Inn_Management_System/
├── public/                 # Static files and images
├── src/
│   ├── components/         # Reusable modals and UI components
│   ├── config/             # Frontend inn configuration
│   ├── lib/                # API helper
│   ├── pages/              # Dashboard and management screens
│   ├── utils/              # Authentication helpers
│   ├── App.jsx             # Application routes
│   ├── Layout.jsx          # Main navigation layout
│   ├── App.css             # Main application styling
│   └── index.css           # Global styling
├── backend/
│   └── server/
│       ├── middleware/     # Authentication middleware
│       ├── routes/          # REST API routes
│       ├── db.js            # MySQL connection pool
│       ├── index.js         # Express application
│       └── .env.example     # Backend configuration template
├── .env.example            # Frontend configuration template
└── README.md
```

## Troubleshooting

### `npm error Missing script: "dev"`

Run the backend command from `backend/server`, not from the `backend` parent folder. The backend package defines `dev` as `nodemon index.js`.

### CORS: origin is not allowed

Make sure the frontend is opened at `http://localhost:3000` and that `CLIENT_ORIGINS` in `backend/server/.env` includes that exact origin. Restart the backend after changing `.env`. `localhost` and `127.0.0.1` are different browser origins.

### MySQL connection error

- Confirm MySQL/MariaDB is running in Laragon.
- Check `DB_HOST`, `DB_USER`, `DB_PASSWORD`, `DB_NAME`, and the database port if customized.
- Confirm the database and required tables exist.
- Check the backend terminal for the actual MySQL error message.

### API is running but login or pages fail

The `/api/health` endpoint only checks the Express server. Check the backend terminal and browser developer console for database errors, API errors, or authentication issues. Confirm the database schema matches the SQL queries in `backend/server/routes`.

## Production and security notes

- Use HTTPS for deployed sites.
- Set `CLIENT_ORIGINS` to the exact trusted frontend domain(s), not `*` when credentials are enabled.
- Store production database credentials and JWT secrets in the hosting provider's environment settings.
- Never commit `.env`, passwords, tokens, or real guest information.
- Back up the database regularly and restrict database access to trusted services.
- This project is configured as a separate installation per inn. It is not yet a multi-tenant SaaS application where many inns share one database safely.
