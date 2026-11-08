# Quiz application

The Vite/React frontend and Express API are deployed by the same Node.js server. Quiz definitions and candidate submissions are stored in MySQL; admin credentials and database connection details are read only by the server from environment variables.

## Project folders

- `frontend/` contains the React application, Vite entry page, and frontend build configuration.
- `backend/` contains the Express API server.
- `database/` contains the local MySQL setup script.
- `.env` and `package.json` remain at the project root so both frontend tooling and the backend can use the same project configuration.

## Requirements

- Node.js 22 LTS
- MySQL 8.0+ (or a compatible MySQL service)

## Configure local environment

1. If `.env` does not exist, copy `.env.example` to `.env`. Do not overwrite an existing `.env`.
2. Set `ADMIN_USERNAME`, `ADMIN_PASSWORD`, and `SESSION_SECRET` to private values. `SESSION_SECRET` must be at least 32 characters.
3. Open `database/setup-mysql.sql` in MySQL Workbench and execute it while connected as a MySQL administrator. It creates the `quiz_app` database and an app user. The generated app-user password is already written to the ignored local `.env` file.
4. If you did not use the local setup script, set `MYSQL_HOST`, `MYSQL_PORT`, `MYSQL_USER`, `MYSQL_PASSWORD`, and `MYSQL_DATABASE` to match your MySQL service.
5. Install dependencies and run the app:

   ```powershell
   npm install
   npm run dev
   ```

The Vite frontend is available at `http://localhost:3000`; API requests are proxied to Express on port 3001. The server creates the required tables automatically on its first start. For local production-mode testing, run `npm run build` followed by `npm start`.

Never commit `.env` or use `VITE_` prefixes for admin or database secrets. The `.env` file is ignored by Git; `.env.example` contains placeholders only.

After the MySQL setup is complete, legacy quizzes and submissions in this browser's `localStorage` are imported into MySQL on the first successful admin login. This only works in the same browser and origin where the old app stored the data. The old browser data is removed only after the server confirms the import.

## Deploy frontend and backend together

Deploy this repository as a Node web service (for example, on Render):

- Build command: `npm ci && npm run build`
- Start command: `npm start`
- Add the admin and MySQL environment variables from `.env.example` in the hosting dashboard. Do not upload `.env` to source control.
- Set MySQL host, username, password, and database to the values from a managed MySQL provider reachable by the web service.
- Let the platform set `PORT`; the server listens on that port in production.

The deployment URL serves both the web interface and `/api` endpoints. Use HTTPS and a strong, unique admin password and session secret.

## Checks

```powershell
npm run lint
npm run build
```
