# ProDesk / TaskMatrix – Developer Prompt

This document contains the primary developer prompt used to generate and guide the development of the ProDesk capstone project.

---

## Main Full-Stack Application Prompt

Use this prompt to initialize and build the MERN full-stack Agile Project Management platform.

### Prompt Text

```markdown
Build a complete, production-ready full-stack Agile Project Management platform named ProDesk (TaskMatrix).

## Tech Stack Requirements
- **Frontend**: Next.js 14+ (App Router) / React, Tailwind CSS, Zustand for state management.
- **Backend**: Node.js + Express.js, modular structure (routes, controllers, models, middleware).
- **Database**: MongoDB + Mongoose.
- **Real-Time**: Socket.io for live activity updates and workspace synchronization.

## Database Schema (ERD)
The database contains the following tables and relationships:

### Tables
- **users**:
  - `id`: string (Primary Key)
  - `name`: string
  - `email`: string (Unique)
  - `password`: string
  - `role`: string
  - `avatar`: string
  - `createdAt`: timestamp

- **projects**:
  - `id`: string (Primary Key)
  - `title`: string
  - `description`: text
  - `createdBy`: string (Foreign Key -> users.id)
  - `deadline`: date
  - `status`: string
  - `createdAt`: timestamp

- **tasks**:
  - `id`: string (Primary Key)
  - `title`: string
  - `description`: text
  - `priority`: string
  - `status`: string
  - `assignedTo`: string (Foreign Key -> users.id)
  - `projectId`: string (Foreign Key -> projects.id)
  - `dueDate`: date
  - `createdAt`: timestamp

- **comments**:
  - `id`: string (Primary Key)
  - `taskId`: string (Foreign Key -> tasks.id)
  - `userId`: string (Foreign Key -> users.id)
  - `message`: text
  - `createdAt`: timestamp

- **activity_logs**:
  - `id`: string (Primary Key)
  - `userId`: string (Foreign Key -> users.id)
  - `projectId`: string (Foreign Key -> projects.id)
  - `action`: string
  - `timestamp`: timestamp

