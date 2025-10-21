# Server — Tea Estate Maintenance & Repair Management System

This folder contains the Express.js API server used by the React client to store and manage maintenance and repair requests.

## Summary
- Node + Express server
- MongoDB via Mongoose
- PDF generation for reports using `pdfkit`
- API mount points: `/maintenance`, `/repair`, `/manager`, `/technician`

## Requirements
- Node.js (v16+ recommended)
- npm
- MongoDB instance (local or remote)

## Environment
Create a `.env` file in the `server/` folder (or copy `.env.example`) with at least these variables:

MONGODB_URL — MongoDB connection string (example in `.env.example`)
PORT — port to run the server (default 8070)

## Install
From the top-level `server/` folder:

```powershell
cd server
npm install
```

## Run
- Development (auto-restart with nodemon):

```powershell
npm run dev
```

- Production:

```powershell
npm start
```

The server logs the port it listens on (defaults to 8070). The client expects the API to be available at that port unless configured otherwise.

## Main models
- `models/maintenance.js` — Maintenance model with fields: item_id, details, date_created, tech_id, start_date, status, end_date, cost
- `models/repair.js` — Repair model with the same fields as maintenance

## Routes and endpoints
The server mounts the routers in `server.js` at these base paths. Below are the important endpoints provided by each router.

### /maintenance
- POST /maintenance/add — Add a maintenance request (body: item_id, details, date_created, tech_id, start_date, status, end_date, cost)
- GET /maintenance/ — Get all maintenance requests
- PUT /maintenance/update/:id — Update a maintenance request by MongoDB _id
- DELETE /maintenance/delete/:id — Delete request by _id
- GET /maintenance/get/:id — Get a maintenance request by _id (note: implementation has a bug; see Troubleshooting)
- GET /maintenance/generatePDF — Generates and downloads a maintenance_report.pdf

### /repair
- POST /repair/add — Add a repair request (same body as maintenance)
- GET /repair/ — Get all repair requests
- PUT /repair/update/:id — Update a repair request
- DELETE /repair/delete/:id — Delete repair by _id
- GET /repair/get/:id — Get a repair request by _id (note: implementation has a bug; see Troubleshooting)
- GET /repair/generatePDF — Generates and downloads a repair_report.pdf

### /manager
Provides higher-level operations for both maintenance and repair requests:
- GET /manager/:type — Get all requests of `type` (`maintenance` or `repair`). Optional query `status` to filter.
- PUT /manager/:type/:id/approve — Approve or reject a request (body: status = 'Approved'|'Rejected')
- PUT /manager/:type/:id/assign — Assign a technician (body: tech_id). Requires the request be 'Approved'.
- GET /manager/requests — Returns both maintenanceRequests and repairRequests combined

### /technician
Technician actions for starting/completing jobs (expects a `type` in the request body for `checkRequestStatus` middleware):
- PUT /technician/:id/start — Start a job (body must include type: 'maintenance' or 'repair')
- PUT /technician/:id/complete — Complete a job (body must include type)

## Notes / Troubleshooting
- CORS is enabled by default in `server/server.js`.
- The server reads `MONGODB_URL` from `.env`. Make sure MongoDB is reachable.
- The `GET /maintenance/get/:id` and `GET /repair/get/:id` routes contain incorrect variable names (`Maintences` vs `Maintenance`) and return logic — these endpoints may not work as-is and need fixing.
- The `server/package.json` uses `nodemon` as a dependency; use `npm run dev` for auto-reload.

## Suggested improvements (optional contributions)
- Add input validation (Joi or express-validator) to all POST/PUT endpoints.
- Fix the `get/:id` endpoints to correctly return the found document.
- Replace direct PDF generation streaming with a more robust paginated layout for large result sets.
- Add authentication/authorization for manager and technician actions.

## Contact
For questions about the server code, check the top-level repo owner metadata in `package.json` or contact the project maintainer listed there.
