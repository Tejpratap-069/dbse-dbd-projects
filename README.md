# dbse-dbd-projects
# Student Course Enrollment and Academic Management System

**Student Portal Pro — Cluster Edition**

A web application for course enrollment and academic administration, built with React, Node.js, Express and MySQL.

The system provides separate student and administrator workflows. Students select an academic cluster, enroll in eligible course sections and view their timetable and academic records. Administrators inspect student enrollments, publish academic results, record attendance and manage official CGPA values.

## Table of Contents

- [Project Overview](#project-overview)
- [Problem Definition](#problem-definition)
- [Objectives](#objectives)
- [Features](#features)
- [Technology Stack](#technology-stack)
- [Application Architecture](#application-architecture)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Installation and Setup](#installation-and-setup)
- [Running the Application](#running-the-application)
- [Accounts and Login](#accounts-and-login)
- [Database Design](#database-design)
- [MySQL Demonstration Queries](#mysql-demonstration-queries)
- [REST API](#rest-api)
- [Postman Testing](#postman-testing)
- [Enrollment Rules](#enrollment-rules)
- [CGPA Management](#cgpa-management)
- [Security](#security)
- [Testing and Verification](#testing-and-verification)
- [Project Demonstration](#project-demonstration)
- [Troubleshooting](#troubleshooting)
- [Limitations and Future Development](#limitations-and-future-development)
- [Team](#team)

## Project Overview

Student Portal Pro centralizes course registration and academic records in a relational database.

The React interface communicates with the Express backend through REST APIs. The backend authenticates requests, checks permissions and validates academic rules before reading or updating MySQL records.

The application supports four academic clusters, persistent enrollment records, waitlists, academic summaries and administrator-controlled CGPA updates.

## Problem Definition

Course enrollment requires consistent information about students, prerequisites, schedules and available seats.

Manual coordination can lead to:

- Duplicate registrations.
- Overlapping class schedules.
- Enrollment without the required prerequisites.
- Course selections from incompatible clusters.
- Registration beyond section capacity.
- Fragmented grade and attendance records.
- Academic changes without a clear history.

The project addresses these issues through backend validation, relational constraints and database transactions.

## Objectives

1. Provide student and administrator interfaces connected to persistent records.
2. Validate course enrollment on the server.
3. Maintain one academic cluster per student for the current semester.
4. Check prerequisites, timetable conflicts, section capacity and credit limits.
5. Support waitlisting and promotion when seats become available.
6. Calculate academic summaries from published grades and recorded attendance.
7. Allow authorized administrators to manage official CGPA values.
8. Maintain an audit history of enrollment and academic changes.
9. Document and demonstrate the REST API using Postman and OpenAPI.

## Features

### Student Portal

- Student registration and login.
- Personal academic dashboard.
- Course catalog and section details.
- Academic cluster selection.
- Enrollment planner.
- Course enrollment, section switching and course drops.
- Timetable based on selected sections.
- Grades and academic summaries.
- Recorded attendance information.
- Enrollment notifications.
- Transcript CSV export.
- Profile and password management.

### Administrator Portal

- Administrator login.
- Dashboard with academic and enrollment summaries.
- Student directory with enrolled courses and cluster details.
- Enrollment and drop management.
- Grade publication.
- Attendance recording.
- Official CGPA updates with a reason.
- Cluster and section-group management.
- Waitlist inspection.
- Academic audit history.
- Roster CSV export.

### Cluster System

The configured semester contains four clusters:

| Cluster | Section Groups |
|---|---:|
| Cluster 1 | 2 |
| Cluster 2 | 3 |
| Cluster 3 | 2 |
| Cluster 4 | 3 |

A student selects one cluster before enrollment.

Students may choose different section groups for different subjects, provided every selected offering belongs to the same cluster.

The first enrollment or waitlist entry locks the cluster choice for the semester. Dropping a course does not remove this lock.

## Technology Stack

| Layer | Technology | Responsibility |
|---|---|---|
| Frontend | React | Student and administrator interfaces |
| Frontend tooling | Vite | Development server and frontend build |
| Routing | React Router | Navigation between application pages |
| Backend runtime | Node.js | Server-side JavaScript execution |
| Backend framework | Express | REST routes, middleware and HTTP responses |
| Database | MySQL | Persistent relational data |
| Database driver | mysql2 | Connection pooling, queries and transactions |
| Authentication | JWT | Authenticated API sessions |
| Password protection | Salted scrypt hashes | Password storage and verification |
| API testing | Postman | Requests, authorization and response inspection |
| API contract | OpenAPI | Endpoint and request documentation |
| Database client | MySQL Workbench | SQL execution and record inspection |
| Development | VS Code | Source editing and terminal execution |
| Version control | Git and GitHub | Repository management |

This implementation uses Express as its backend framework. FastAPI is not part of the delivered application.

## Application Architecture

The application uses a modular monolithic architecture.

| Component | Responsibility |
|---|---|
| React pages | Display data and collect user actions |
| Frontend API service | Send HTTP requests to the backend |
| Express routes | Handle API endpoints |
| Authentication middleware | Verify tokens and access permissions |
| Enrollment services | Apply academic rules and registration decisions |
| mysql2 connection pool | Execute parameterized queries and transactions |
| MySQL database | Store related academic records |

### Request Lifecycle

1. A user performs an action in the React interface.
2. The frontend sends an HTTP request to the Express API.
3. Protected requests include a JWT Bearer token.
4. Middleware verifies authentication and applicable permissions.
5. The backend validates the request and academic rules.
6. The backend reads or updates MySQL records.
7. Express returns a JSON response.
8. React updates the interface using that response.

In production demo mode, one Express application serves both the API and the built React frontend.

## Project Structure

The following paths are relative to the application root, which contains `package.json`.

| Path | Purpose |
|---|---|
| `src/pages/` | Student interface pages |
| `src/pages/admin/` | Administrator interface pages |
| `src/components/` | Reusable React components |
| `src/context/AppContext.jsx` | Shared application state |
| `src/services/api.js` | Frontend API communication |
| `src/App.jsx` | Application routes |
| `server/server.js` | Backend startup and database connection check |
| `server/app.js` | Express configuration, routes and error handling |
| `server/config/db.js` | MySQL connection pool |
| `server/middleware/auth.js` | Authentication and permission checks |
| `server/routes/` | REST endpoint handlers |
| `server/services/enrollment.js` | Enrollment transactions and waitlist handling |
| `server/services/rules.js` | Validation and academic rule helpers |
| `server/services/security.js` | Password hashing and JWT handling |
| `server/db/schema.sql` | Database schema and stored logic |
| `server/db/seed.js` | Demonstration data insertion |
| `server/db/clusters.js` | Cluster setup and offering mappings |
| `server/db/initDb.js` | Database initialization |
| `server/openapi.js` | OpenAPI specification |
| `server/tests/` | Backend unit and integration checks |
| `postman/Student_Portal_API.postman_collection.json` | Prepared API requests |
| `scripts/setup.mjs` | Configuration and setup workflow |
| `scripts/build.mjs` | Frontend build workflow |
| `docs/VERIFICATION.md` | Recorded verification results |
| `server/.env.example` | Configuration example |
| `README.md` | Project documentation |

## Prerequisites

Install the following:

| Requirement | Version or Purpose |
|---|---|
| Node.js | Version 20 or later |
| npm | Included with Node.js |
| MySQL Community Server | Version 8.0.16 or later |
| VS Code | Source editing and terminal access |
| MySQL Workbench | Optional database inspection client |
| Postman | Optional API testing client |
| Git | Required if cloning the repository |

MySQL Server runs the database. MySQL Workbench connects to that server.

Installing Workbench alone does not provide a running database server.

## Installation and Setup

### 1. Obtain the Project

Clone the repository:

```bash
git clone https://github.com/Tejpratap-069/dbse-dbd-projects.git
cd dbse-dbd-projects/Project
```

Alternatively, extract the project ZIP and open its `Student-Portal-Pro` folder in VS Code.

Run the following commands from the application folder containing the root `package.json`.

### 2. Start MySQL

Ensure the MySQL service is running.

On Windows:

1. Press `Windows + R`.
2. Enter `services.msc`.
3. Locate your MySQL service.
4. Start it if it is stopped.

The service name depends on the MySQL installation.

### 3. Install Dependencies

```bash
npm install
npm --prefix server install
```

If PowerShell blocks `npm.ps1`, use:

```powershell
npm.cmd install
npm.cmd --prefix server install
```

### 4. Run Setup

```bash
npm run setup
```

Enter the requested MySQL connection details.

Setup configures the application, initializes the database, inserts demonstration data and builds the frontend.

The default database name is:

```text
student_portal_pro
```

Use an isolated database for this project.

### Alternative: Manual Configuration

Copy `server/.env.example` to `server/.env`.

In Windows PowerShell:

```powershell
Copy-Item server/.env.example server/.env
```

In Linux or macOS:

```bash
cp server/.env.example server/.env
```

Edit `server/.env`:

```env
DB_HOST=127.0.0.1
DB_PORT=3306
DB_USER=root
DB_PASSWORD=your_mysql_password
DB_NAME=student_portal_pro
PORT=5000
JWT_SECRET=replace_with_a_random_secret_at_least_32_characters
```

Replace the password and JWT secret with your own values.

Generate a random JWT secret:

```bash
node -e "console.log(require('node:crypto').randomBytes(32).toString('hex'))"
```

Then initialize and build:

```bash
npm run db:setup
npm run build
```

Do not commit `server/.env` to GitHub.

The automatic setup may store the database password as `DB_PASSWORD_B64` to preserve special characters. Base64 is encoding, not encryption. If correcting the password manually, remove `DB_PASSWORD_B64` and set `DB_PASSWORD`.

## Running the Application

### Production Demo Mode

After setup:

```bash
npm start
```

Open:

```text
http://localhost:5000
```

Keep the backend terminal open.

### Development Mode

```bash
npm run dev
```

Open:

```text
http://localhost:3000
```

The Vite development server forwards `/api` requests to the Express backend on port `5000`.

### Separate Frontend and Backend Terminals

Backend:

```bash
npm run dev:server
```

Frontend:

```bash
npm run dev:client
```

### Windows Launcher Files

The ZIP includes:

- `SETUP-WINDOWS.bat` for initial setup.
- `START-WINDOWS.bat` for subsequent execution.

Terminal commands provide an alternative if a launcher fails.

### Stopping the Application

Press `Ctrl + C` in the running terminal.

### Subsequent Runs

1. Start MySQL.
2. Open the application folder.
3. Run `npm start`.
4. Open `http://localhost:5000`.

Database setup is not required for every normal run.

## Accounts and Login

Use the application's login page for student or administrator access.

The login API determines the account role from the verified account.

### GitHub Version

The uploaded GitHub version uses generated demonstration passwords during initial database setup. Retain the setup output containing those credentials.

Its demonstration student identifiers include `DEMO1001`, `DEMO1002` and `DEMO1003`.

### Previously Supplied ZIP

The previously supplied v3 ZIP contains these demonstration accounts:

| Role | Username or Student ID | Password |
|---|---|---|
| Administrator | `admin` | `Admin@2026!` |
| Student | `2520030477` | `Student@2026!` |
| Student occupying a one-seat section | `2520030002` | `Student@2026!` |
| Additional student | `2024CSB1084` | `Student@2026!` |

These fixed passwords apply to that ZIP version. They should not be assumed to work in the GitHub version.

Repeated setup preserves existing account passwords. New student accounts use the password entered during registration.

All supplied academic records are illustrative demonstration data.

## Database Design

The MySQL schema separates identity, course offerings, registration and academic records.

### Main Tables

| Table | Purpose |
|---|---|
| `students` | Student accounts and profile information |
| `admins` | Administrator accounts |
| `faculty` | Faculty information |
| `courses` | Course catalog |
| `course_sections` | Section schedules, instructors and capacities |
| `course_syllabus` | Course syllabus entries |
| `course_outcomes` | Course outcome information |
| `course_assessments` | Course assessment information |
| `course_prerequisites` | Prerequisite relationships |
| `enrollments` | Student course and section registrations |
| `grades` | Published academic results |
| `attendance` | Recorded attendance sessions |
| `timetable` | Student timetable records |
| `waitlist` | Waiting enrollment requests |
| `notifications` | Student notifications |
| `enrollment_audit_log` | Enrollment change history |
| `academic_clusters` | Semester cluster definitions |
| `cluster_groups` | Section groups within clusters |
| `section_clusters` | Offering-to-group mappings |
| `student_cluster_choices` | Student semester cluster selections and locks |
| `cgpa_overrides` | Official administrator-entered CGPA values |
| `academic_audit` | Administrative academic change history |

### Relational Integrity

- Primary keys identify records.
- Foreign keys connect related tables.
- Unique constraints prevent duplicate values where required.
- Check constraints restrict invalid numeric values.
- Transactions keep related registration changes consistent.

### Views and Stored Logic

The schema includes:

- Academic summary views.
- Student rankings.
- Course enrollment statistics.
- Enrollment audit triggers.
- A student transcript procedure.
- An academic calculation function.

`schema.sql` defines the database structure.

`seed.js` inserts demonstration data through Node.js and mysql2. A separate `seed.sql` file is therefore not required.

## MySQL Demonstration Queries

Connect to the same MySQL server configured in `server/.env`.

Run these commands individually in MySQL Workbench.

### Select the Database

```sql
USE student_portal_pro;
```

### Display Tables

```sql
SHOW TABLES;
```

### Display Student Profiles

```sql
SELECT
    id,
    roll_number,
    full_name,
    department,
    semester
FROM students;
```

### Display Courses

```sql
SELECT
    course_code,
    course_name,
    credits,
    semester
FROM courses;
```

### Display Sections

```sql
SELECT
    id,
    course_id,
    section_name,
    faculty_name,
    schedule,
    total_seats
FROM course_sections;
```

### Display Enrollments

```sql
SELECT * FROM enrollments;
```

### Display Cluster Definitions and Student Choices

```sql
SELECT * FROM academic_clusters;
SELECT * FROM cluster_groups;
SELECT * FROM student_cluster_choices;
```

### Display Academic Summaries

```sql
SELECT * FROM v_student_academic_summary;
```

### Display Official CGPA Overrides

```sql
SELECT * FROM cgpa_overrides;
```

### Display Course Enrollment Statistics

```sql
SELECT * FROM v_course_enrollment_stats;
```

### Display Waitlist Records

```sql
SELECT * FROM waitlist;
```

### Display Enrollment Audit History

```sql
SELECT *
FROM enrollment_audit_log
ORDER BY log_id DESC;
```

### Display Administrative Academic Changes

```sql
SELECT *
FROM academic_audit
ORDER BY id DESC;
```

### Inspect a Table Definition

```sql
SHOW CREATE TABLE enrollments;
```

### Inspect Triggers

```sql
SHOW TRIGGERS;
```

### Execute the Transcript Procedure

Replace the example ID with a student ID present in your database:

```sql
CALL sp_student_transcript('DEMO1001');
```

For the original ZIP, use its corresponding student ID.

The setup script executes schema statements individually. When manually executing multi-statement procedure or trigger definitions in Workbench, use appropriate `DELIMITER` handling.

## REST API

The default API base URL is:

```text
http://localhost:5000/api
```

### Representative Endpoints

| Method | Endpoint | Purpose |
|---|---|---|
| GET | `/api/health` | Check API and database availability |
| POST | `/api/auth/login` | Authenticate an account |
| POST | `/api/auth/register` | Register a student |
| GET | `/api/auth/me` | Retrieve the authenticated account |
| GET | `/api/courses` | Retrieve the course catalog |
| GET | `/api/clusters` | Retrieve clusters and offerings |
| POST | `/api/enrollments` | Request enrollment or waitlisting |
| PUT | `/api/clusters/students/:id` | Select a student's cluster |
| PUT | `/api/clusters/students/:id/cgpa` | Set or clear official CGPA |
| GET | `/api/admin/overview` | Retrieve administrator statistics |
| GET | `/api/openapi.json` | Retrieve the OpenAPI contract |

`:id` is a path parameter. Replace it with the actual student ID.

### Authentication Header

Protected requests require:

```http
Authorization: Bearer <token>
```

### API Documentation

Local documentation:

```text
http://localhost:5000/api/docs
```

OpenAPI specification:

```text
http://localhost:5000/api/openapi.json
```

The local documentation page displays the contract. Postman can import the OpenAPI JSON.

## Postman Testing

Postman tests the backend independently of the React interface.

### Import the Collection

1. Start the application.
2. Open Postman.
3. Select **Import**.
4. Import:

```text
postman/Student_Portal_API.postman_collection.json
```

5. Inspect collection variables and set the base URL to the running backend.
6. Update credentials and student IDs to match your installed version.

### Suggested Request Sequence

1. Health.
2. Login Student.
3. Student Profile.
4. Courses.
5. List clusters and offerings.
6. Enrollment request.
7. Admin Overview with a student token.
8. Login Admin.
9. Admin Overview with an administrator token.

The supplied login requests can save the returned token through their collection scripts. Inspect the request authorization configuration before sending protected requests.

### Manual Login Example

Request:

```http
POST http://localhost:5000/api/auth/login
Content-Type: application/json
```

JSON body:

```json
{
  "studentIdOrEmail": "admin",
  "password": "YOUR_ADMIN_PASSWORD"
}
```

Copy the returned token into Postman's Bearer Token authorization field for a protected request.

### Important Checks

| Check | Evidence |
|---|---|
| Health request | API reports database connectivity |
| Valid login | Response contains an authentication token |
| Invalid login | Backend rejects the credentials |
| Course retrieval | JSON records match MySQL course data |
| Cross-cluster request | Backend rejects the incompatible section |
| Unauthorized admin access | Student token cannot access admin operations |
| Authorized admin access | Admin token grants access |
| Accepted update | Matching database record reflects the change |

Use the OpenAPI contract or prepared collection for request bodies. Enrollment identifiers must match real records in the current database.

## Enrollment Rules

Before accepting an enrollment, the backend checks:

- Student identity and ownership.
- Current semester compatibility.
- Selected academic cluster.
- Whether the cluster choice is locked.
- Course and section relationships.
- Duplicate active enrollment.
- Passing published prerequisite results.
- The configured 24-credit limit.
- Timetable conflicts.
- Available section capacity.
- Earlier eligible waitlist entries.

### Transaction Handling

Enrollment operations:

1. Start a database transaction.
2. Lock section records in a stable order.
3. Lock the student record.
4. Validate the registration request.
5. Apply enrollment or waitlist changes.
6. Save related timetable and notification changes.
7. Commit on success.
8. Roll back on failure.

The current implementation locks all section rows during these operations. This prioritizes consistency for the demonstration workload and limits concurrent write throughput.

### Waitlist Promotion

When a seat becomes available, the service considers waiting students and rechecks eligibility.

An ineligible waiting entry can remain in the queue while a later eligible student receives the seat.

Promotion runs during relevant enrollment and drop operations.

## CGPA Management

The application distinguishes calculated academic results from official CGPA overrides.

### Calculated GPA

Published grades contribute according to their course credits:

```text
GPA = Sum(Grade Point × Course Credits) / Sum(Course Credits)
```

The application uses its configured project grading scale.

Semester GPA filters results to the student's current semester.

### Official CGPA Override

An administrator can enter an official CGPA between `0` and `10`.

The update requires a reason.

The effective academic value uses the override when one exists. Clearing the override restores the grade-derived calculation.

Published grades remain available regardless of the override.

### Administrator Workflow

1. Log in as an administrator.
2. Open **Student Directory**.
3. Select the student.
4. Choose **Edit academic record**.
5. Enter the official CGPA.
6. Enter the reason.
7. Save the update.
8. Inspect the student's academic summary and audit history.

## Security

Implemented controls include:

- Salted scrypt password hashing.
- Signed JWT authentication.
- Eight-hour token expiration.
- Student ownership checks.
- Administrator role checks.
- Parameterized SQL queries.
- Authentication request rate limiting.
- A 64 KB JSON body limit.
- Security response headers.
- Backend validation of academic changes.
- Controlled error responses.

Logout removes the browser token. Server-side token revocation is not implemented.

The current server binds to `127.0.0.1`. Public deployment requires deliberate hosting configuration and HTTPS.

## Testing and Verification

### Core Tests

```bash
npm test
```

### Frontend Build

```bash
npm run build
```

### Database and API Integration Checks

```bash
npm run test:integration
```

Integration checks require a disposable database whose name ends in `_test`.

They recreate that test database and modify its records. Do not run them against the ordinary demonstration database.

Back up your configuration, point `DB_NAME` to a dedicated test database, initialize it and restore the normal configuration afterward.

### Recorded Verification Results

The supplied verification record reports:

| Check | Recorded Result |
|---|---|
| Core unit tests | 7 passed |
| Database/API integration checks | 40 passed |
| React server-render smoke checks | 8 passed |
| Production frontend build | Passed |
| Repeated database setup | Existing academic records preserved |

Recorded environment:

- Node.js 24.
- MariaDB 10.11.14.
- Verification date: 1 October 2026.

MySQL 8.0.16+ is the intended installation.

These results do not establish Windows launcher compatibility, separate MySQL server validation or interactive browser end-to-end coverage. See `docs/VERIFICATION.md` for the recorded scope.

## Project Demonstration

A recommended evaluation sequence:

1. Show the project source in VS Code.
2. Start the backend and open the health endpoint.
3. Open MySQL Workbench and display the database tables.
4. Log in as a student.
5. Select a cluster.
6. Enroll in an eligible course section.
7. Open the timetable.
8. Attempt an incompatible cluster selection.
9. Log in as an administrator.
10. Inspect the student and enrolled subjects.
11. Update official CGPA with a reason.
12. Query the academic summary and audit records in MySQL.
13. Demonstrate waitlisting using a full section.
14. Release a seat and inspect promotion.
15. Send an API request in Postman.
16. Show the repository and setup documentation.

Use student IDs and course sections from the current database.

Demonstration data can change after earlier enrollments or drops. Rehearse the sequence against the installed database before evaluation.

## Troubleshooting

| Issue | Action |
|---|---|
| `npm` is not recognized | Install Node.js and reopen the terminal |
| PowerShell blocks `npm.ps1` | Use `npm.cmd` |
| `package.json` cannot be found | Open the application root before running commands |
| Connection refused on port 3306 | Start MySQL and check `DB_HOST` and `DB_PORT` |
| MySQL access denied | Correct the database username and password |
| Unknown database | Check `DB_NAME` and run `npm run db:setup` |
| JWT secret error | Set a random secret containing at least 32 characters |
| Frontend build missing | Run `npm run build` |
| Port 5000 already occupied | Stop the conflicting application |
| Empty academic records | Confirm setup completed and the correct database is selected |
| Login fails | Verify credentials for the installed ZIP or GitHub version |
| Protected request rejected | Supply a valid token with the required role |
| Enrollment rejected | Read the response for prerequisites, cluster, credits or timetable restrictions |

The frontend proxy expects the backend on port `5000`. If changing the backend port for development, update the proxy configuration in `vite.config.js`.

## Limitations and Future Development

Current limitations:

- One backend application with a local database.
- Section locking serializes enrollment writes.
- Event-driven waitlist promotion without a background worker.
- JWT sessions without server-side revocation.
- No dedicated faculty login.
- No complete multi-year academic history model.
- No institutional single sign-on.
- No completed public cloud deployment.

Planned improvements:

- HTTPS deployment.
- Institutional authentication.
- Password recovery.
- Automated backups and restoration checks.
- Broader Windows and MySQL verification.
- Interactive browser end-to-end tests.
- Operational logging and monitoring.
- Multi-year academic records.
- Performance measurement and narrower locking where justified.

The delivered project does not implement FastAPI, Spring Boot, MongoDB, vector databases, Redis, Kafka, Docker or Kubernetes.

## Team

| Team Member | Student ID |
|---|---|
| V. Abhidev | 2520030325 |
| Tej Pratap Singh | 2520030476 |
| G. Shashank | 2520030495 |

**Guide:** Dr. Y Subbarayudu  
Assistant Professor  
Department of Computer Science and Engineering  
Koneru Lakshmaiah Education Foundation

## Repository

[Student Course Enrollment Project](https://github.com/Tejpratap-069/dbse-dbd-projects/tree/main/Project)

The repository contains the source code, database setup, API collection and project documentation.
