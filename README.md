# 🏢 TRAI Office Complaint Management System (CMS)

A full-stack, role-based internal IT and administrative support platform engineered for a multi-floor organizational structure (**Floors 4 to 7** across **IT, NSL, QoS, Administration, Legal, Finance, and Broadcasting** divisions). 

Designed with a **social-media-style employee directory and live ticket activity feed**, the platform streamlines daily workplace issue reporting, smart hardware onboarding for new hires, bulk employee profile provisioning, and multi-dimensional complaint analytics (**Daily, Weekly, Monthly, and Yearly**).

## ✨ Key Features

### 👥 1. Three-Role Access Control (`ADMIN`, `ENGINEER`, `USER`)
* **Users (Office Employees):** Create and customize social office profiles, browse the multi-floor office directory, request seat confirmations, raise cascading support tickets, and converse with engineers on a live ticket timeline.
* **Engineers (IT & Admin Technicians):** Maintain technical specialization badges (`Hardware`, `Network`, `Cloud/OneDrive`, `Printer`, `Telephone`), manage assigned floor/division queues, upload diagnostic logs/images, and post real-time progress updates (`OPEN` → `ASSIGNED` → `IN_PROGRESS` → `WAITING_FOR_PARTS` → `RESOLVED` → `CLOSED`).
* **Admins (System Administrators):** Bulk-import initial employee and engineer profiles via CSV, verify and approve **Seat Confirmation Orders**, assign or reroute tickets, and monitor organization-wide SLA metrics via the analytics dashboard.

### 🌐 2. Social-Media-Inspired Office Directory & Profiles
* **Comprehensive Employee Profiles:** Stores `Full Name`, `Employee Code`, `Designation`, `Division` (`IT`, `NSL`, `QoS`, `ADMINISTRATION`, `LEGAL`, `BROADCASTING`, `FINANCE`), `Floor No.` (`4th` to `7th Floor`), `Room / Seat No.`, `Intercom Extension No.`, and an interactive **"About the Work I Do in Office"** bio.
* **First-Time Launch Bulk CSV Import:** Eliminates manual one-by-one account creation at launch. Admins can upload a single `.csv` roster (or run `npx prisma db seed`) to bulk-upsert hundreds of employee and engineer profiles in a single database transaction using `multer`, `csv-parse`, and `zod`.
* **First-Login Profile Completion Prompt:** Bulk-imported users log in with a temporary onboarding password and are greeted by a guided banner to personalize their office work bio and avatar while their official floor, seat, and division are pre-populated.

### 🚀 3. Smart Onboarding & Seat Confirmation Gate
* **Seat Verification Workflow:** New joiners can submit a **Request Seat Confirmation** directly from the glassmorphic login screen (`login.png` UI) using their official Admin Seat Allocation Order reference number.
* **Automated Hardware Gate:** Enforces API and UI guards that prevent unconfirmed seats (`isSeatConfirmed: false`) from raising new PC, monitor, or peripheral provisioning tickets until verified by an Administrator.

### 🎫 4. Dynamic Cascading Ticketing & Live Timeline Feed
* **Strict Category Routing:** Tickets are organized into 6 primary categories that dynamically cascade into specialized sub-categories:
  * **Hardware:** Mouse, Keyboard, Monitor / Display, Desktop CPU / SMPS, UPS / Battery Backup, Docking Station
  * **Software:** Windows 11 OS, MS Office 365, e-Office / NIC Portal, Antivirus / Endpoint Security, PDF / DSC Token
  * **Telephone:** IP Phone No Dial Tone, Extension Mapping / Transfer, Handset / Display Fault, Video Conferencing Unit
  * **Printer:** Paper Jam, Toner / Cartridge Replacement, Network Printer Offline, High-Speed Scanner Issue
  * **Network:** LAN Port Inactive, TRAI Wi-Fi Dropouts, VPN / Remote Access, Firewall / Website Whitelisting
  * **Cloud / OneDrive:** OneDrive Sync Error, GovCloud / Quota Upgrade, Shared Division Folder Access, MFA / Password Lockout
* **Social Activity Thread:** Every complaint acts as an interactive chronological thread where users and engineers post replies, status transitions, and file attachments.

### 📊 5. Multi-Dimensional Admin Data Analytics
* **Customizable Timeframes:** Powered by PostgreSQL `DATE_TRUNC` aggregations (`day`, `week`, `month`, `year`) and rendered with **Recharts**.
* **Operational Telemetry:** Tracks raised vs. resolved ticket volumes, Mean Time to Resolution (MTTR), **Floor 4–7 & Division heatmaps**, cascading category distributions, engineer workload leaderboards, and top ticket-raising users to identify recurring hardware or network bottlenecks.

## 💻 Tech Stack

| Layer | Technologies Used |
| :--- | :--- |
| **Frontend** | React 18 (via Vite), Tailwind CSS 3, React Router 6, Axios, Recharts, Lucide Icons |
| **Backend** | Node.js 24 LTS, Express.js, Prisma 6 (`prisma@6` & `@prisma/client@6`), Zod |
| **Database** | PostgreSQL 16 (containerized via Docker Compose) |
| **Security & Auth** | JWT (`jsonwebtoken`), `bcrypt`, Role-Based Access Control (`USER`, `ENGINEER`, `ADMIN`), CORS allow-list, Login Rate Limiting (`express-rate-limit`) |
| **File Uploads & CSV** | `multer` (attachments & avatars), `csv-parse` (bulk profile onboarding) |
| **Testing & Quality** | Node built-in test runner (`node --test`), ESLint 8 (`eslint-plugin-react`, `eslint-plugin-react-hooks@4`), Prettier, EditorConfig |
| **Dev Environment** | Ubuntu 26.04 LTS, `fnm` with `.node-version`, `npm ci` with lockfiles, Docker Engine & Compose, VS Code |
| **Deployment & Ops** | Docker Compose, Nginx (serving static frontend & reverse-proxying `/api`), `systemd` timer running daily `pg_dump` backups, `ufw` host firewall |

## 📁 Repository Structure

```text
trai-cms/
├── .editorconfig
├── .node-version
├── docker-compose.yml
├── ops/
│   ├── nginx.conf
│   ├── trai-cms-backup.service
│   └── trai-cms-backup.timer
├── backend/
│   ├── .env.example
│   ├── package.json
│   ├── prisma/
│   │   ├── schema.prisma
│   │   ├── seed.js
│   │   └── seed-data/
│   │       └── initial_staff.csv
│   ├── uploads/
│   ├── src/
│   │   ├── app.js
│   │   ├── server.js
│   │   ├── lib/
│   │   │   └── prisma.js
│   │   ├── middleware/
│   │   │   ├── auth.js
│   │   │   ├── rateLimiter.js
│   │   │   └── upload.js
│   │   ├── controllers/
│   │   │   ├── auth.controller.js
│   │   │   ├── profile.controller.js
│   │   │   ├── bulkImport.controller.js
│   │   │   ├── complaint.controller.js
│   │   │   └── analytics.controller.js
│   │   └── routes/
│   │       ├── auth.routes.js
│   │       ├── profile.routes.js
│   │       ├── complaint.routes.js
│   │       └── analytics.routes.js
│   └── tests/
│       └── api.test.js
└── frontend/
    ├── .eslintrc.cjs
    ├── index.html
    ├── package.json
    ├── postcss.config.js
    ├── tailwind.config.js
    ├── vite.config.js
    └── src/
        ├── main.jsx
        ├── index.css
        ├── App.jsx
        ├── api/
        │   └── axiosClient.js
        ├── context/
        │   └── AuthContext.jsx
        ├── constants/
        │   └── cascadingCategories.js
        ├── components/
        │   ├── TraiLogo.jsx
        │   ├── Navbar.jsx
        │   └── SeatConfirmationModal.jsx
        └── pages/
            ├── LoginPage.jsx
            ├── DirectoryPage.jsx
            ├── RaiseTicketPage.jsx
            ├── ComplaintFeedPage.jsx
            ├── AdminAnalyticsPage.jsx
            └── BulkImportPage.jsx
```

## 🛠️ Getting Started (Ubuntu 26.04 LTS)

### 1. Prerequisites
Ensure your Ubuntu Linux machine has the following installed:
* **Node.js 24 LTS** (managed via `fnm` using the repository's `.node-version` file)
* **Docker Engine & Docker Compose v2** (`docker compose`)
* **Git** configured over SSH

```bash
# Install fnm and activate Node.js 24 LTS
curl -fsSL https://fnm.vercel.app/install | bash
source ~/.bashrc
fnm install 24
fnm use 24
```

### 2. Clone the Repository
```bash
git clone git@github.com:your-username/trai-office-cms.git
cd trai-office-cms
```

### 3. Start PostgreSQL 16 via Docker Compose
```bash
docker compose up -d postgres
```

### 4. Configure & Launch the Backend (`http://localhost:4000`)
```bash
cd backend
cp .env.example .env
npm ci
```

Update `backend/.env` with your local credentials:
```ini
PORT=4000
DATABASE_URL="postgresql://cms_admin:strong_postgres_password@localhost:5432/trai_cms_db?schema=public"
JWT_SECRET="replace-with-64-character-random-hex-secret"
CORS_ORIGIN="http://localhost:5173"
DEFAULT_ONBOARDING_PASSWORD="Welcome@TRAI2026"
```

Run Prisma 6 database migrations and seed initial staff profiles across Floors 4–7:
```bash
npx prisma migrate dev --name init_trai_cms
npx prisma generate
npx prisma db seed
npm run dev
```

### 5. Configure & Launch the Frontend (`http://localhost:5173`)
Open a second terminal window:
```bash
cd frontend
npm ci
npm run dev
```
Open `http://localhost:5173` in your browser to access the TRAI-CMS login screen.

## 📥 Bulk Importing Employee & Engineer Profiles at First Launch

Instead of registering accounts one-by-one, prepare a CSV file matching the following columns and either upload it via **Admin Dashboard → Bulk CSV Profile Import** (`POST /api/admin/users/bulk-import`) or place it at `backend/prisma/seed-data/initial_staff.csv`:

```csv
employeeCode,email,fullName,role,designation,division,floorNo,roomOrSeatNo,extensionNo,aboutWorkBio,isSeatConfirmed,seatOrderRefNo
TRAI-001,admin@trai.gov.in,Rajesh Kumar Sharma,ADMIN,Joint Advisor (IT),IT,5,Room 501-A,4501,"Leading TRAI digital infrastructure, e-Office governance, and IT service delivery.",true,TRAI/ADM/SEAT/2026-001
TRAI-104,amit.verma@trai.gov.in,Amit Verma,ENGINEER,Senior Network Engineer,IT,5,IT Lab 504-B,4504,"Managing L3 switches, floor Wi-Fi controllers, and hardware support for Floors 4-7.",true,TRAI/ADM/SEAT/2026-014
TRAI-212,priya.nair@trai.gov.in,Dr. Priya Nair,USER,Senior Research Officer (QoS),QOS,5,Cubicle 518-C,4518,"Analyzing telecom Quality of Service benchmarks and 5G latency telemetry.",true,TRAI/ADM/SEAT/2026-089
```

* **Idempotent Upsert:** Re-uploading an updated CSV updates existing employees' floor/seat assignments (matched via `employeeCode`) and inserts newly added staff without duplicating records.
* **Row-Level Zod Validation:** Automatically validates that `floorNo` is between `4` and `7`, `role` is `ADMIN | ENGINEER | USER`, and email addresses are well-formed before committing the transaction.

## 🧪 Running Tests & Code Quality Checks

```bash
# Run backend unit and integration tests using Node 24's built-in test runner
cd backend
node --test

# Run ESLint 8 and Prettier formatting checks across frontend and backend
cd ../frontend
npm run lint
```

## 🚀 Production Deployment on Linux

1. **Build the Frontend Static Assets:**
   ```bash
   cd frontend && npm ci && npm run build
   ```
2. **Serve via Nginx & Docker Compose:**
   Configure Nginx (`ops/nginx.conf`) to serve `frontend/dist` on ports `80/443` and reverse-proxy `/api/` and `/uploads/` to the Express backend on `http://127.0.0.1:4000`.
3. **Enable Automated Daily PostgreSQL Backups (`systemd` Timer):**
   ```bash
   sudo cp ops/trai-cms-backup.service ops/trai-cms-backup.timer /etc/systemd/system/
   sudo systemctl daemon-reload
   sudo systemctl enable --now trai-cms-backup.timer
   ```
4. **Harden Host Firewall (`ufw`):**
   ```bash
   sudo ufw default deny incoming
   sudo ufw default allow outgoing
   sudo ufw allow OpenSSH
   sudo ufw allow 'Nginx Full'
   sudo ufw enable
   ```

## 🤝 Contributing

1. Create a feature branch linked to a GitHub Issue (`git checkout -b feat/issue-12-floor-heatmap`).
2. Ensure all code passes `npm run lint` and `node --test`.
3. Open a Pull Request against `main` and link it to the corresponding Milestone on the GitHub Projects board.
