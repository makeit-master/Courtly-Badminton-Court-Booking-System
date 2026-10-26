Courtly

https://img.shields.io/badge/status-under%20development-orange
https://img.shields.io/badge/stage-early%20development-yellow

Table of Contents
Overview
Features
Technology Stack
System Architecture
Project Structure (Proposed)
Getting Started (Windows)
Environment Variables
Database Setup with Prisma
API Overview (Planned)
Testing
Security Considerations
Roadmap
Contributing
License
Author
Overview

Finding and booking a badminton court often means calling venues, messaging owners, or relying on informal group chats. Availability is hard to see, double bookings happen, and owners have no simple way to track reservations.

Courtly aims to solve this by providing a single web application where:

Players can browse courts, see open time slots by date, book a court, and manage their reservations.
Court owners can list and manage their courts and monitor incoming bookings.

The project is also a practical exercise in building a full-stack TypeScript application with authentication, role-based access control, relational data modeling, and booking-conflict handling.

Features
Implemented
 None yet. This section will be updated as features are completed and verified.
Planned
#	Feature	Status
1	User registration and login	Planned
2	Browse and view badminton courts	Planned
3	Search and filter courts	Planned
4	View available time slots by date	Planned
5	Book a court	Planned
6	View personal booking history	Planned
7	Cancel eligible bookings	Planned
8	Role-based access for players and court owners	Planned
9	Court management for owners	Planned
10	Prevent double booking of the same court time slot	Planned
11	Automated tests for key API endpoints and booking logic	Planned
Technology Stack

The stack below is planned. Items are not confirmed as installed or configured until the repository reflects them.

Layer	Technology
Frontend	React, TypeScript, Vite
Backend	Node.js, Express.js, TypeScript
Database	PostgreSQL (via Supabase)
ORM	Prisma
Validation	Zod
Testing	Vitest, Supertest
Version control	Git, GitHub
Deployment (planned)	Vercel (frontend); a suitable Node.js hosting service for the backend (to be decided)
System Architecture

Courtly is planned as a three-tier web application:

Frontend (client): A React + TypeScript single-page app built with Vite. It handles the user interface and communicates with the backend over HTTP.
Backend API (server): A Node.js + Express + TypeScript REST API. It handles authentication, authorization, request validation (Zod), and booking business logic.
Database: PostgreSQL hosted on Supabase, used purely as a managed Postgres database.
ORM: Prisma provides type-safe database access, schema definition, and migrations. The backend talks to the database through Prisma.
Backend API (server)
Frontend (client)
HTTP / JSON
User Browser
React + TypeScript + Vite
Express.js + TypeScript
Zod Validation
Prisma ORM
PostgreSQL on Supabase

This diagram shows the planned architecture and may change during development.

Project Structure (Proposed)

This structure is proposed and has not been verified against the actual repository. Folder and file names may change.

text
courtly/
├── client/                  # React + TypeScript frontend (Vite)
│   ├── src/
│   │   ├── components/
│   │   ├── pages/
│   │   ├── services/        # API calls
│   │   └── main.tsx
│   ├── index.html
│   ├── package.json
│   └── vite.config.ts
├── server/                  # Express + TypeScript backend
│   ├── prisma/
│   │   └── schema.prisma
│   ├── src/
│   │   ├── controllers/
│   │   ├── middleware/
│   │   ├── routes/
│   │   ├── schemas/         # Zod schemas
│   │   ├── services/        # Business logic (e.g., booking)
│   │   ├── app.ts
│   │   └── server.ts
│   ├── tests/
│   ├── .env.example
│   ├── package.json
│   └── tsconfig.json
├── .gitignore
└── README.md
Getting Started (Windows)

These instructions describe the intended local setup. Because the project is in early development, some steps may not work until the corresponding code is added. Verify against the actual package.json scripts in each folder.

Prerequisites
Node.js (current LTS version recommended)
npm (included with Node.js)
Git
A Supabase account and project (for the PostgreSQL database)
A code editor such as VS Code

Verify your installations in PowerShell:

powershell
node --version
npm --version
git --version
1. Clone the repository
powershell
git clone <your-repository-url>
cd courtly
2. Install dependencies
powershell
cd server
npm install
cd ..\client
npm install
cd ..
3. Configure environment variables

See Environment Variables, then set up the database as described in Database Setup with Prisma.

4. Run the development servers

Use two separate PowerShell windows.

Backend:

powershell
cd server
npm run dev

Frontend:

powershell
cd client
npm run dev

The Vite dev server typically runs at http://localhost:5173. The backend port is determined by your PORT setting.

The dev script names above are assumptions. Confirm them in each package.json.

Environment Variables

Create a .env file inside server by copying the template. Never commit .env files or real credentials.

powershell
cd server
Copy-Item .env.example .env

Proposed server/.env.example:

env
# Server
PORT=4000
NODE_ENV=development

# Database (replace with your own Supabase PostgreSQL connection string)
DATABASE_URL="postgresql://USER:PASSWORD@HOST:PORT/DATABASE"

# Authentication (use a long, random, private value)
JWT_SECRET="replace-with-a-long-random-secret"
JWT_EXPIRES_IN="1d"

# CORS
CLIENT_ORIGIN="http://localhost:5173"

If the frontend needs configuration, create client/.env.example as well. Vite only exposes variables prefixed with VITE_, and anything exposed to the browser is public:

env
VITE_API_BASE_URL="http://localhost:4000/api"

Make sure .gitignore excludes .env files:

gitignore
.env
.env.*
!.env.example
Database Setup with Prisma

Important: Prisma commands and configuration can change between versions. The commands below are common examples. Always check that they match your installed Prisma version and your project configuration, and consult the official Prisma documentation if anything differs.

Create a PostgreSQL project in Supabase and copy its connection string into DATABASE_URL in server/.env.
Define your data models in server/prisma/schema.prisma.
From the server directory, run:
powershell
cd server

# Generate the Prisma Client
npx prisma generate

# Create and apply a migration during development
npx prisma migrate dev --name init

# (Optional) Browse data in Prisma Studio
npx prisma studio

For deployed environments, migrations are typically applied with npx prisma migrate deploy. Verify this against your Prisma version and hosting setup before use.

Supabase may offer both pooled and direct connection strings. Which one is appropriate for migrations versus runtime queries depends on your configuration, so check the Supabase and Prisma docs.

API Overview (Planned)

All endpoints below are planned and not yet implemented. Routes, methods, and payloads may change.

Method	Endpoint	Description	Access
POST	/api/auth/register	Register a new user	Public
POST	/api/auth/login	Log in and receive a token	Public
GET	/api/courts	List courts (with search/filter)	Public
GET	/api/courts/:id	View court details	Public
GET	/api/courts/:id/slots?date=YYYY-MM-DD	View available time slots for a date	Public
POST	/api/courts	Create a court	Owner
PUT	/api/courts/:id	Update a court	Owner (own courts)
DELETE	/api/courts/:id	Delete a court	Owner (own courts)
POST	/api/bookings	Book a court time slot	Player
GET	/api/bookings/me	View personal booking history	Player
PATCH	/api/bookings/:id/cancel	Cancel an eligible booking	Player (own bookings)
GET	/api/owner/bookings	View bookings for owned courts	Owner
Testing

Automated testing is planned using Vitest and Supertest. Tests are not yet implemented, and no test results are claimed.

Once tests exist, the intended workflow is:

powershell
cd server
npm test

Planned test focus areas:

Authentication and authorization behavior
Court endpoints
Booking creation, cancellation rules, and double-booking prevention

The test script name is an assumption. Confirm it in server/package.json.

Security Considerations

The following are design goals for the project, not claims about the current state of the code:

Password hashing: Passwords should never be stored in plain text. Use a well-established algorithm such as bcrypt or Argon2.
Authentication: Use signed tokens (e.g., JWT) or secure sessions, with a strong secret and sensible expiry.
Authorization: Enforce role checks (player vs. court owner) on the server, and verify resource ownership (e.g., owners may only modify their own courts; players may only cancel their own bookings).
Input validation: Validate and sanitize all incoming request data with Zod before it reaches business logic.
Environment variables: Keep secrets in .env files that are excluded from version control. Provide only a placeholder .env.example.
Preventing duplicate bookings: Enforce uniqueness at the database level (e.g., a unique constraint or an exclusion approach on court and time slot) and handle conflicts safely in a transaction, so concurrent requests cannot book the same slot.
Other practices to consider: CORS restricted to known origins, rate limiting on auth routes, and avoiding leaking sensitive details in error messages.
Roadmap
 Phase 1: Setup
 Initialize client and server projects with TypeScript
 Configure Prisma and connect to Supabase PostgreSQL
 Set up linting and basic project scripts
 Phase 2: Authentication
 User registration and login
 Role-based access (player / owner)
 Phase 3: Courts
 Court listing, details, search, and filters
 Owner court management
 Phase 4: Booking
 Available time slots by date
 Create, view, and cancel bookings
 Double-booking prevention
 Phase 5: Quality
 Tests for key endpoints and booking logic
 Phase 6: Deployment
 Deploy frontend (Vercel)
 Choose a host and deploy the backend
 Future ideas: payments, reviews, notifications, owner analytics
Contributing

Courtly is a personal learning project, but feedback and suggestions are welcome.

Open an issue to report a bug, suggest an improvement, or ask a question.
If you’d like to contribute code, please open an issue first to discuss your idea.
Fork the repository, create a feature branch (git checkout -b feature/your-feature), and open a pull request with a clear description.
Keep changes small and focused, and avoid committing secrets or .env files.

Response times may vary, as this is a side project.

License

No license has been selected yet. Until a license is added, all rights are reserved by the author by default. If you wish to use or build on this code, please contact the author.

Author

GitHub: makeit-master <!-- https://github.com/makeit-master -->

Feel free to reach out through GitHub with feedback or questions.