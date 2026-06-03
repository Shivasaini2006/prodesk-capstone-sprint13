# ProDesk / TaskMatrix – Developer Prompt

This document contains the primary developer prompt used to generate and guide the development of the ProDesk capstone project.


### Prompt Text

```markdown
Build a complete, production-ready full-stack  Project Management platform 

## Tech Stack Requirements
- **Frontend**: Next.js  (App Router) / React, Tailwind CSS, Zustand for state management.
- **Backend**: Node.js + Express.js, modular structure (routes, controllers, models, middleware).
- **Database**: MongoDB + Mongoose.
- **Real-Time**: Socket.io for live activity updates and workspace synchronization.

## Database Schema (ERD DBML)
```dbml
Table users {
id string [pk]
name string
email string [unique]
password string
role string
avatar string
createdAt timestamp
}

Table projects {
id string [pk]
title string
description text
createdBy string
deadline date
status string
createdAt timestamp
}

Table tasks {
id string [pk]
title string
description text
priority string
status string
assignedTo string
projectId string
dueDate date
createdAt timestamp
}

Table comments {
id string [pk]
taskId string
userId string
message text
createdAt timestamp
}

Table activity_logs {
id string [pk]
userId string
projectId string
action string
timestamp timestamp
}

Ref: projects.createdBy > users.id
Ref: tasks.assignedTo > users.id
Ref: tasks.projectId > projects.id
Ref: comments.taskId > tasks.id
Ref: comments.userId > users.id
Ref: activity_logs.userId > users.id
Ref: activity_logs.projectId > projects.id
```

