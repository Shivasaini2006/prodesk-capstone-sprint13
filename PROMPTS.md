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

### Relationships
- A user can create many projects (`projects.createdBy > users.id`).
- A task can be assigned to a user (`tasks.assignedTo > users.id`).
- A project contains many tasks (`tasks.projectId > projects.id`).
- A task contains many comments (`comments.taskId > tasks.id`).
- A comment is written by a user (`comments.userId > users.id`).
- An activity log is triggered by a user (`activity_logs.userId > users.id`).
- An activity log belongs to a project (`activity_logs.projectId > projects.id`).

## Key Features to Implement
1. **User Authentication & Authorization**: Signup, login, JWT-based route protection, role-based access control.
2. **Project Workspace Management**: CRUD endpoints and layouts for projects.
3. **Kanban Board Board View**: Drag-and-drop task card movement with optimistic state updates in Zustand.
4. **Real-time Synchronization**: Socket.io rooms to broadcast task updates and comment alerts to project members.
5. **Activity Log Feed**: Chronological list of user events per project.
```
