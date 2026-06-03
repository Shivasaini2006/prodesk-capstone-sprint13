# TaskMatrix – AI Developer Prompt Playbook

This document contains specialized engineering prompts designed to generate production-ready code blocks during the 5-week build phase. Copy and adapt these prompts when prompting coding assistants for code generation.

---

## 1. Environment & Global Setup Context Prompt
Use this prompt first in any code-generation session to set the baseline guidelines:

```markdown
Role: Senior Full Stack Engineer & Software Architect
Project Name: TaskMatrix (Enterprise Agile Project Management platform)
Frontend Stack: Next.js 14+ (App Router), Tailwind CSS, Shadcn UI, Zustand
Backend Stack: Node.js, Express, MongoDB (Mongoose), Socket.io, Cloudinary
Coding Standards:
- Write TypeScript/JavaScript code following clean-code principles (SOLID, DRY).
- Ensure all functions contain defensive type checks, error-handling blocks, and informative logging.
- Next.js: Use React Server Components (RSC) where possible, and mark client-bound files with 'use client'.
- Express: Adopt a routes-controllers-models structure. Wrap handlers in an async controller wrapper to catch thrown errors.
```

---

## 2. Week 1: JWT & RBAC Middleware Generation Prompt
Use this prompt to build the secure backend authentication pipeline:

```markdown
Generate an Express.js authentication middleware file (`src/middleware/auth.js`) and database models for TaskMatrix.

Requirements:
1. Parse the JWT token from either the HTTP Authorization header (as `Bearer <token>`) or an HttpOnly cookie named `token`.
2. Verify the token using `jsonwebtoken`. If invalid, expired, or missing, return a `401 Unauthorized` JSON payload.
3. If valid, fetch the User record from MongoDB (excluding the `passwordHash` field) and attach it to `req.user`.
4. Export an authorization middleware function called `requireRoles(rolesArray)` that checks if `req.user.role` matches the permitted roles (e.g., 'Admin', 'ProjectManager'). If it does not match, return a `403 Forbidden` response.
5. Provide a Mongoose `User` schema containing: name, email (unique, indexed), passwordHash, role (enum: ['Admin', 'ProjectManager', 'TeamMember']), and avatarUrl.
```

---

## 3. Week 2: Workspace Project CRUD and Member Board Rules Prompt
Use this to generate project workspaces and membership validation:

```markdown
Build an Express router (`src/routes/projects.js`) and matching controllers for managing Project CRUD in TaskMatrix.

Requirements:
1. Models: Use a Mongoose Project schema containing: name, key (unique, 3-5 uppercase characters, e.g. 'PROJ'), description, owner (ref: User), and members (array of refs: User).
2. Endpoints to implement:
   - `GET /` : Returns all projects where the logged-in user is the owner OR a member.
   - `POST /` : Allows Admins and ProjectManagers to create projects. Ensure it autogenerates unique uppercase keys.
   - `PUT /:projectId` : Update project details. Allow only the project owner or Admins to update.
   - `DELETE /:projectId` : Cascade-delete projects, associated tasks, and comments. Restrict this action to users with the 'Admin' role.
3. Include error logs and input validation using an validation library (like express-validator or custom schema checks).
```

---

## 4. Week 3: Kanban Board & React DnD Client UI Prompt
Use this to build the drag-and-drop client interface:

```markdown
Create a React Kanban board component for Next.js App Router using Tailwind CSS, Shadcn UI card primitives, and React DnD (or @hello-pangea/dnd).

Requirements:
1. Columns: Render four columns side-by-side on desktop (To Do, In Progress, In Review, Done). Provide horizontal scrolling on overflow.
2. Cards: Display Task Cards containing title, priority label (colored indicators: Low=green, Medium=blue, High=orange, Critical=red), assignee avatars, and due dates.
3. Drag-and-Drop:
   - Make cards draggable.
   - Make columns drop targets.
   - On drop, execute an optimistic state update in the Zustand store (`useBoardStore`) to move the card instantly in the UI.
   - Call the backend endpoint `PUT /api/tasks/:taskId` to persist the new status, displaying a toast notification on success or rolling back the UI state on failure.
4. Styling: Design with a dark modern layout (dark charcoal cards, blurred glassmorphism headers, micro-animations on hover).
```

---

## 5. Week 3: Zustand Board State Store Prompt
Use this to create the central client store for state updates:

```markdown
Write a Zustand store (`src/store/useBoardStore.js`) to manage the state of the Kanban board in the TaskMatrix client.

Requirements:
1. State parameters: `tasks` (array of tasks), `isLoading` (boolean), `error` (string | null).
2. Actions:
   - `fetchTasks(projectId)`: Calls `GET /api/tasks?projectId=id` and sets state.
   - `moveTaskOptimistic(taskId, sourceStatus, destinationStatus)`: Instantly moves a task card between columns in client-side state.
   - `updateTaskStatus(taskId, newStatus)`: Calls the backend `PUT /api/tasks/:taskId` and updates the DB. If the call fails, rolls back the client state to the previous status.
3. Ensure state modifications return immutably updated arrays.
```

---

## 6. Week 4: Socket.io Real-Time Activity Feed Prompt
Use this to coordinate live state synchronization across browsers:

```markdown
Build a real-time collaboration module linking Socket.io with Express and Next.js.

Requirements:
1. Server-side: Integrate socket setup inside the main Express app. When a user connects:
   - Listen for `join:project` events and place sockets into project-specific rooms.
   - Listen for task moves or comment additions, broadcast notifications to all other connections in that project room.
2. Client-side: Create a React hook `useSocket(projectId)` that:
   - Establishes a Socket.io client connection on mount.
   - Emits a `join:project` room registration call.
   - Listens for `task:updated` and `comment:added` broadcasts.
   - Updates the client Zustand store data without forcing a full page reload and calls a toast alert (e.g. 'Alex moved task TM-12 to In Progress').
```

---

## 7. Week 5: Dashboard Analytics & KPI Generation Prompt
Use this to generate charts, aggregations, and widgets:

```markdown
Create the metrics dashboard page (`src/app/dashboard/page.tsx`) and an aggregation endpoint.

Requirements:
1. Endpoint `GET /api/analytics/:projectId` using Mongoose `$facet` aggregate pipeline returning:
   - Total count of tasks grouped by status (To Do, In Progress, In Review, Done).
   - Total count of tasks grouped by priority levels.
   - Workload counts (active tasks assigned per user email).
2. Frontend Widget:
   - Fetch this aggregate data and display it using Recharts components.
   - Display a bar chart indicating assignees' active workloads.
   - Display a pie/donut chart representing status distribution.
   - Render a row of clean metric widgets using glassmorphic cards with responsive layouts.
```
