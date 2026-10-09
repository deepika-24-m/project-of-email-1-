# AI Email Management Agent — Frontend

A production-grade, enterprise-ready React + TypeScript frontend dashboard for an autonomous **AI Email Management Agent**.

Engineered with modern SaaS aesthetics, real-time WebSocket telemetry, resilient API architecture with transparent fallback for standalone testing, and strict client-side security (Zero-Secret architecture).

---

## ⚡ Tech Stack

* **React 18** with **TypeScript** (Strict mode)
* **Vite** — High-speed build tooling and HMR
* **Tailwind CSS** — Professional SaaS design system with dark/light mode
* **React Router v6** — Declarative client-side routing
* **Axios** — Centralized HTTP client with unified error interceptors
* **Recharts** — Responsive charting for email volume, classifications, and agent latency
* **Lucide React** — Crisp icon system
* **WebSocket / SSE** — Sub-second real-time agent activity stream

---

## 🔒 Security Architecture (Strict Zero-Secret Guarantee)

This frontend strictly abides by enterprise security guidelines:
1. **Zero LLM Credentials**: Never touches OpenAI, Gemini, Claude, or Anthropic API keys.
2. **Zero OAuth Secrets**: Gmail OAuth client IDs, client secrets, and refresh tokens are strictly backend-managed.
3. **Safe Content Sanitization**: Email HTML and body strings are rendered safely without executing scripts.
4. **Environment Variables**: Only public, non-secret endpoint locations are read from Vite environment variables (`VITE_API_BASE_URL`, `VITE_WS_URL`).

---

## 📁 Project Structure

```text
frontend/
├── public/
├── src/
│   ├── components/
│   │   ├── agent/
│   │   │   └── AgentActivityTimeline.tsx   # Real-time event log with filters & timestamps
│   │   ├── common/
│   │   │   ├── ConfirmDialog.tsx           # Generic confirmation modal
│   │   │   ├── EmptyState.tsx              # Empty states with actions
│   │   │   ├── LoadingSkeleton.tsx         # Skeleton loaders for rows/cards/charts
│   │   │   ├── Modal.tsx                   # Accessible modal container
│   │   │   └── StatusBadge.tsx             # Badges for classification, priority, state
│   │   ├── dashboard/
│   │   │   ├── ActivityChart.tsx           # Recharts volume area chart
│   │   │   ├── AgentStatusWidget.tsx       # Hero AI agent status card with pause/scan
│   │   │   ├── RecentActivityList.tsx      # Checkmarked activity log
│   │   │   └── StatCard.tsx                # Metric KPI card
│   │   ├── email/
│   │   │   ├── EmailDetail.tsx             # Complete email reader + AI action toolbar
│   │   │   ├── EmailList.tsx               # Filterable, searchable, sortable list
│   │   │   ├── EmailRow.tsx                # Single email row with badges & check
│   │   │   └── ReplyComposer.tsx           # Editable AI reply generator with tone picker
│   │   ├── layout/
│   │   │   ├── DashboardLayout.tsx         # Main layout wrapper
│   │   │   ├── Navbar.tsx                  # Omnibox search, notifications, theme toggle
│   │   │   └── Sidebar.tsx                 # Router nav + Agent status footer card
│   │   └── notifications/
│   │       └── NotificationItem.tsx        # Individual notification card
│   ├── context/
│   │   ├── AgentContext.tsx                # Global agent status & streaming state
│   │   ├── ThemeContext.tsx                # Dark / Light / System theme provider
│   │   └── ToastContext.tsx                # Global toast notifications
│   ├── data/
│   │   └── mockData.ts                     # Realistic mock dataset for standalone UI demo
│   ├── hooks/
│   │   └── useAgentActivity.ts             # Real-time WebSocket + simulator hook
│   ├── pages/
│   │   ├── AgentActivity.tsx               # Real-time live agent execution monitor
│   │   ├── Analytics.tsx                   # Recharts volume, breakdown & agent stats
│   │   ├── Dashboard.tsx                   # Main SaaS overview & KPIs
│   │   ├── Important.tsx                   # High-priority emails with AI reasons
│   │   ├── Inbox.tsx                       # Master-detail email inbox
│   │   ├── Notifications.tsx               # Notification center with filters
│   │   ├── Settings.tsx                    # Autonomous agent toggles & preferences
│   │   └── Spam.tsx                        # Threat quarantine table & actions
│   ├── services/
│   │   ├── agentService.ts                 # Agent status, toggle & scan APIs
│   │   ├── analyticsService.ts             # Analytics & dashboard statistics APIs
│   │   ├── api.ts                          # Central Axios client with mock fallback
│   │   ├── emailService.ts                 # Full email CRUD & AI action triggers
│   │   ├── notificationService.ts          # Notification management APIs
│   │   └── settingsService.ts              # Agent & notification configuration APIs
│   ├── types/
│   │   └── index.ts                        # Complete TypeScript type definitions
│   ├── App.tsx                             # Application routes
│   ├── index.css                           # Tailwind directives & custom scrollbars
│   └── main.tsx                            # React DOM entry point
├── .env.example
├── index.html
├── package.json
├── postcss.config.js
├── tailwind.config.js
├── tsconfig.json
├── tsconfig.node.json
└── vite.config.ts
```

---

## 🚀 Quick Start & Running Instructions

### 1. Installation

Navigate to the `frontend/` directory and install dependencies:

```bash
cd frontend
npm install
```

### 2. Configure Environment (Optional)

Copy `.env.example` to `.env`:

```bash
cp .env.example .env
```

Default values:
```env
VITE_API_BASE_URL=http://localhost:8000/api
VITE_WS_URL=ws://localhost:8000/ws/agent
VITE_ENABLE_MOCK_FALLBACK=true
```

> **Note**: With `VITE_ENABLE_MOCK_FALLBACK=true`, the application works out of the box with realistic mock data even before your backend server is booted! When your backend is active, it seamlessly proxies real network calls.

### 3. Launch Development Server

```bash
npm run dev
```

Open [http://localhost:5173](http://localhost:5173) in your browser.

### 4. Build for Production

```bash
npm run build
```

---

## 🔌 Backend API Specification Expected by Frontend

The backend should expose standard REST endpoints and a WebSocket server:

### Emails
* `GET /api/emails` — Fetch emails (supports query params: `search`, `tab`, `sortBy`)
* `GET /api/emails/{id}` — Get single email details
* `POST /api/emails/{id}/generate-reply` — Body: `{ tone: "professional" | "casual" | ... }`
* `POST /api/emails/{id}/send-reply` — Body: `{ replyText: string, tone?: string, subject?: string }`
* `POST /api/emails/{id}/classify` — Returns: `{ classification: string, reason: string }`
* `POST /api/emails/{id}/summarize` — Returns: `{ summary: string }`
* `POST /api/emails/{id}/archive` — Mark email as archived
* `PATCH /api/emails/{id}/read` — Body: `{ isRead: boolean }`
* `PATCH /api/emails/{id}/star` — Toggle starred status
* `DELETE /api/emails/{id}` — Delete email

### Spam Quarantine
* `GET /api/spam` — Retrieve emails flagged as spam
* `POST /api/spam/{id}/not-spam` — Restore quarantined email to inbox
* `POST /api/spam/block-sender` — Body: `{ senderEmail: string }`

### Important Emails
* `GET /api/important` — Retrieve emails flagged with high priority and AI reasoning

### Agent Status & Telemetry
* `GET /api/agent/status` — Returns agent state (`online` / `paused`), current task, uptime
* `POST /api/agent/toggle` — Body: `{ active: boolean }`
* `GET /api/agent/activities?limit=20` — Chronological log of recent agent actions
* `POST /api/agent/scan` — Trigger on-demand inbox re-scan

### Real-Time WebSocket Telemetry
* `ws://localhost:8000/ws/agent`
  * Emits event stream:
    ```json
    {
      "type": "activity",
      "data": {
        "id": "act-99",
        "timestamp": "2026-10-08T14:32:10Z",
        "type": "classifying",
        "status": "info",
        "title": "Classifying email",
        "description": "Classification: IMPORTANT",
        "processingTimeMs": 310
      }
    }
    ```

### Notifications & Analytics
* `GET /api/notifications` — Notification items
* `PATCH /api/notifications/{id}/read` — Mark notification read
* `POST /api/notifications/mark-all-read` — Mark all read
* `GET /api/analytics` — KPI counters and timeseries for Recharts
* `GET /api/settings` & `PUT /api/settings` — App and agent toggle preferences
