## Tea Estate Maintenance & Repair — Client

This folder contains the React single-page application used by estate employees, managers, and technicians to submit and track repair and maintenance requests.

Key points
- Built with Create React App (React 18).
- Uses React Router for client-side routing and Axios for HTTP requests to the backend API.
- Organized into three main feature areas under `src/`:
	- `MREmployee/` — employee-facing pages (add requests, view status, edit requests).
	- `MRManager/` — manager dashboard and views for requests by status.
	- `MRTechnician/` — technician views for in-progress work.

Main libraries
- react, react-dom, react-router-dom
- axios for API calls
- chart.js + react-chartjs-2 for charts
- @fortawesome/react-fontawesome for icons

Application routes (client-side)
The app mounts routes in `src/App.js`. Important routes include:

- /MREmployee — Employee dashboard
- /MREmployee/addrepair — Add a repair request
- /MREmployee/addmaintenance — Add a maintenance request
- /MREmployee/viewstatus — View status of employee requests
- /MREmployee/edit — Edit a request

- /MRManager — Manager dashboard
- /MRManager/maintenancerequests — Maintenance requests list
- /MRManager/maintenancestatus — Maintenance status overview
- /MRManager/maintenancepending — Pending maintenance
- /MRManager/maintenanceinprogress — In-progress maintenance
- /MRManager/maintenancecompleted — Completed maintenance

- /MRManager/repairrequests — Repair requests list
- /MRManager/repairstatus — Repair status overview
- /MRManager/repairpending — Pending repairs
- /MRManager/repairinprogress — In-progress repairs
- /MRManager/repaircompleted — Completed repairs

- /Technician — Technician dashboard
- /Technician/maintenancerequest — Technician maintenance work list
- /Technician/repairrequest — Technician repair work list

Connecting to the backend API
- The backend server is expected to run separately (see `../server`).
- The server mounts API routes at these base paths:
	- /maintenance
	- /repair
	- /manager
	- /technician
- By default the backend listens on port 8070 (configured in `server/server.js`). Set the client env var `REACT_APP_API_URL` to point to the backend, for example:

	REACT_APP_API_URL=http://localhost:8070

	In components the app typically uses Axios with this base URL. If you need to change it, create or update `client/.env` with the value above.

Local setup
1. Make sure the backend server is running (see `server/`).
2. From the `client/` folder install dependencies and start the dev server:

```powershell
cd client
npm install
npm start
```

The client dev server runs on http://localhost:3000 by default.

Build for production
1. From `client/`:

```powershell
npm run build
```

2. Serve the contents of the `build` folder with any static server, or integrate with the backend to serve static files.

Environment variables
- `REACT_APP_API_URL` — (recommended) base URL of the backend API, e.g. `http://localhost:8070`.

Troubleshooting
- CORS errors: make sure the backend `server.js` has CORS enabled (it does by default in this project).
- API 404/500 errors: confirm backend is running and `REACT_APP_API_URL` points to the correct port.
- Port conflicts: change the client port by setting `PORT` in `client/.env`.

How to contribute
- Follow the existing component structure under `src/`.
- Keep styling consistent using the component-level CSS files already present (for example `MREmployee/Dashboard.css`).

More
- For backend setup, database URL, and environment variables see the top-level `server/` README or `server/server.js`.

---

If you want, I can also:
- add a short usage examples section to this README showing how components call the API, or
- create a `client/.env.example` with the recommended `REACT_APP_API_URL` value.
