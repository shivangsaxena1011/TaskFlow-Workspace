# TaskFlow — Database Schema & Data Models (DATABASE_SCHEMA.md)

**Version:** 1.0  
**Status:** Approved for Architecture Phase  
**Author:** Data Architecture Team  
**Database:** MongoDB Atlas (Mongoose ODM)  

---

![Backend Database Schema](infographics/TaskFlow%20Backend%20Database%20Schema.png)


## 1. Entity Relationship Diagram (ERD)

```
┌─────────────────┐             1:N              ┌────────────────────────┐
│      User       ├─────────────────────────────►│     ProjectMember      │
└────────┬────────┘                              └───────────┬────────────┘
         │                                                   │
         │ 1:N (owner)                         N:1 (project) │
         ▼                                                   ▼
┌─────────────────┐             1:N              ┌────────────────────────┐
│     Project     ├─────────────────────────────►│          Task          │
└─────────────────┘                              └───────────┬────────────┘
         │                                                   │
         │ 1:N                                               │ 1:N
         ▼                                                   ▼
┌─────────────────┐                              ┌────────────────────────┐
│   ActivityLog   │                              │        Comment         │
└─────────────────┘                              └────────────────────────┘
         ▲
         │
┌────────┴────────┐
│  Notification   │ (Linked to recipient User & source Project/Task)
└─────────────────┘
```

---

## 2. Collection Schemas & Field Definitions

### 2.1 `User` Collection (`users`)
Stores core identity and credentials.

```javascript
const UserSchema = new mongoose.Schema({
  name: {
    type: String,
    required: [true, 'Name is required'],
    trim: true,
    maxlength: 60
  },
  email: {
    type: String,
    required: [true, 'Email is required'],
    unique: true,
    lowercase: true,
    trim: true,
    index: true,
    match: [/^\S+@\S+\.\S+$/, 'Please enter a valid email address']
  },
  passwordHash: {
    type: String,
    required: [true, 'Password hash is required'],
    select: false // Never return in default queries
  },
  avatar: {
    type: String,
    default: 'https://api.dicebear.com/7.x/initials/svg?seed=TF'
  },
  role: {
    type: String,
    enum: ['user', 'system_admin'],
    default: 'user'
  }
}, {
  timestamps: true
});
```

### 2.2 `Project` Collection (`projects`)
Represents an isolated collaborative workspace.

```javascript
const ProjectSchema = new mongoose.Schema({
  name: {
    type: String,
    required: [true, 'Project name is required'],
    trim: true,
    maxlength: 100
  },
  description: {
    type: String,
    trim: true,
    maxlength: 500,
    default: ''
  },
  owner: {
    type: mongoose.Schema.Types.ObjectId,
    ref: 'User',
    required: true,
    index: true
  },
  visibility: {
    type: String,
    enum: ['private', 'team', 'public'],
    default: 'private'
  },
  isArchived: {
    type: Boolean,
    default: false
  }
}, {
  timestamps: true
});
```

### 2.3 `ProjectMember` Collection (`project_members`)
Associates users to projects with granular RBAC permissions.

```javascript
const ProjectMemberSchema = new mongoose.Schema({
  project: {
    type: mongoose.Schema.Types.ObjectId,
    ref: 'Project',
    required: true,
    index: true
  },
  user: {
    type: mongoose.Schema.Types.ObjectId,
    ref: 'User',
    required: true,
    index: true
  },
  role: {
    type: String,
    enum: ['owner', 'admin', 'member', 'viewer'],
    default: 'member',
    required: true
  },
  joinedAt: {
    type: Date,
    default: Date.now
  }
}, {
  timestamps: true
});

// Composite unique constraint: User can only have one membership record per project
ProjectMemberSchema.index({ project: 1, user: 1 }, { unique: true });
```

### 2.4 `Task` Collection (`tasks`)
Kanban board work item.

```javascript
const TaskSchema = new mongoose.Schema({
  project: {
    type: mongoose.Schema.Types.ObjectId,
    ref: 'Project',
    required: true,
    index: true
  },
  title: {
    type: String,
    required: [true, 'Task title is required'],
    trim: true,
    maxlength: 150
  },
  description: {
    type: String,
    default: '',
    maxlength: 4000
  },
  status: {
    type: String,
    enum: ['todo', 'in_progress', 'review', 'done'],
    default: 'todo',
    index: true
  },
  priority: {
    type: String,
    enum: ['low', 'medium', 'high', 'urgent'],
    default: 'medium'
  },
  position: {
    type: Number,
    required: true,
    default: 0 // Fractional or sequential position index within column
  },
  assignee: {
    type: mongoose.Schema.Types.ObjectId,
    ref: 'User',
    default: null,
    index: true
  },
  createdBy: {
    type: mongoose.Schema.Types.ObjectId,
    ref: 'User',
    required: true
  },
  dueDate: {
    type: Date,
    default: null
  },
  tags: [{
    type: String,
    trim: true
  }]
}, {
  timestamps: true
});

// Compound index for high-velocity board fetching and ordering
TaskSchema.index({ project: 1, status: 1, position: 1 });
```

### 2.5 `Comment` Collection (`comments`)
Task-scoped conversational messages.

```javascript
const CommentSchema = new mongoose.Schema({
  task: {
    type: mongoose.Schema.Types.ObjectId,
    ref: 'Task',
    required: true,
    index: true
  },
  project: {
    type: mongoose.Schema.Types.ObjectId,
    ref: 'Project',
    required: true,
    index: true
  },
  author: {
    type: mongoose.Schema.Types.ObjectId,
    ref: 'User',
    required: true
  },
  content: {
    type: String,
    required: [true, 'Comment content cannot be empty'],
    maxlength: 2000
  },
  mentions: [{
    type: mongoose.Schema.Types.ObjectId,
    ref: 'User'
  }]
}, {
  timestamps: true
});
```

### 2.6 `Notification` Collection (`notifications`)
User-specific alerts for assignments, mentions, and updates.

```javascript
const NotificationSchema = new mongoose.Schema({
  recipient: {
    type: mongoose.Schema.Types.ObjectId,
    ref: 'User',
    required: true,
    index: true
  },
  actor: {
    type: mongoose.Schema.Types.ObjectId,
    ref: 'User',
    required: true
  },
  type: {
    type: String,
    enum: ['task_assigned', 'task_status_changed', 'comment_added', 'user_mentioned', 'project_invite'],
    required: true
  },
  project: {
    type: mongoose.Schema.Types.ObjectId,
    ref: 'Project',
    default: null
  },
  task: {
    type: mongoose.Schema.Types.ObjectId,
    ref: 'Task',
    default: null
  },
  message: {
    type: String,
    required: true
  },
  isRead: {
    type: Boolean,
    default: false,
    index: true
  }
}, {
  timestamps: true
});

NotificationSchema.index({ recipient: 1, isRead: 1, createdAt: -1 });
```

### 2.7 `ActivityLog` Collection (`activity_logs`)
Immutable audit trail for workspace events.

```javascript
const ActivityLogSchema = new mongoose.Schema({
  project: {
    type: mongoose.Schema.Types.ObjectId,
    ref: 'Project',
    required: true,
    index: true
  },
  actor: {
    type: mongoose.Schema.Types.ObjectId,
    ref: 'User',
    required: true
  },
  action: {
    type: String,
    enum: [
      'project_created', 'project_updated', 'member_added', 'member_removed',
      'task_created', 'task_updated', 'task_moved', 'task_deleted', 'comment_posted'
    ],
    required: true
  },
  targetEntity: {
    type: String,
    enum: ['project', 'task', 'comment', 'member'],
    required: true
  },
  targetId: {
    type: mongoose.Schema.Types.ObjectId,
    default: null
  },
  details: {
    type: mongoose.Schema.Types.Mixed,
    default: {}
  }
}, {
  timestamps: { createdAt: true, updatedAt: false }
});

ActivityLogSchema.index({ project: 1, createdAt: -1 });
```

---

## 3. Database Indexes & Performance Optimization

| Collection | Index Keys | Type | Purpose |
| :--- | :--- | :--- | :--- |
| `users` | `{ email: 1 }` | Unique | Fast login lookup and duplicate prevention. |
| `project_members` | `{ project: 1, user: 1 }` | Unique Compound | Fast authorization verification; guarantees single membership record. |
| `tasks` | `{ project: 1, status: 1, position: 1 }` | Compound | Fast retrieval of board columns and ordered card rendering. |
| `tasks` | `{ assignee: 1 }` | Standard | "My Assigned Tasks" queries across dashboard. |
| `comments` | `{ task: 1, createdAt: 1 }` | Compound | Chronological comment thread loading. |
| `notifications` | `{ recipient: 1, isRead: 1, createdAt: -1 }` | Compound | Fast unread notification badge calculation and timeline view. |
| `activity_logs` | `{ project: 1, createdAt: -1 }` | Compound | Reverse chronological audit feed pagination. |
