<div align="center">
  <img src=".github/skylab.svg" alt="SKY LAB Logo" width="80" />
  <h1>SKY LAB Forms</h1>
  <p>
    A full-stack form builder and management platform built for<br/>
    <strong>Yıldız Technical University - SKY LAB</strong>
  </p>
  <p>
    <a href="https://forms.yildizskylab.com"><img src="https://img.shields.io/badge/Live-yildizskylab.com-7c3aed?style=for-the-badge" alt="Live" /></a>
    <img src="https://img.shields.io/badge/Next.js-16-000000?style=for-the-badge&logo=nextdotjs" alt="Next.js" />
    <img src="https://img.shields.io/badge/.NET-9.0-512BD4?style=for-the-badge&logo=dotnet" alt=".NET" />
    <img src="https://img.shields.io/badge/Docker-Ready-2496ED?style=for-the-badge&logo=docker&logoColor=white" alt="Docker" />
  </p>
</div>

<br/>

> Create, manage, and analyze dynamic forms with drag-and-drop editing, conditional logic, reusable component groups, approval workflows, and Excel exports - powered by a modular .NET backend and a Next.js frontend.

<div align="center">
  <br/>
  <img src=".github/admin-preview.gif" alt="SKY LAB Forms Preview" width="800" />
  <br/><br/>
</div>

---

## Repository Structure

This repository uses **Git submodules** to manage the frontend and backend as separate projects:

```
skylab-forms/
├── frontend/          # Next.js 16 form builder UI (submodule)
├── backend/           # .NET 9.0 modular monolith API (submodule)
├── docker-compose.yml
└── README.md
```

## Features

### Form Builder
- **13 field types** - short text, long text, toggle, dropdown, multi-choice, date picker, time picker, file upload, link, slider, matrix, and more
- **Drag-and-drop** reordering with smooth animations (dnd-kit)
- **Rich text editor** for form descriptions (TipTap)
- **Conditional logic** - show or hide fields based on user responses
- **Form linking** - link fields to other forms for nested submissions
- **Live preview** - see your form as you build it

### Reusable Component Groups
- Save commonly used field sets as reusable templates
- Import component groups into any form

### Response Management
- View, filter, search, and paginate responses
- **Approval workflow** - approve, reject, or leave pending with notes
- **Archive** responses to keep your workspace clean
- **Export to Excel** for offline analysis

### Admin Dashboard
- At-a-glance statistics with weekly trend charts
- Recent forms with quick-action shortcuts

### Form Settings
- Anonymous response collection
- Single / multiple response control per user
- Manual review before acceptance
- Publish / unpublish on demand
- SEO metadata for shared form links

---

## Tech Stack

### Frontend

| Layer | Technology |
|-------|-----------|
| Framework | Next.js 16 (App Router) |
| UI | React 19 |
| Auth | NextAuth.js v5 + Keycloak |
| State / Data | TanStack React Query v5 |
| Styling | Tailwind CSS v4 |
| Animations | Framer Motion |
| Rich Text | TipTap |
| Drag & Drop | dnd-kit |
| Charts | Recharts |

### Backend

| Layer | Technology |
|-------|-----------|
| Runtime | .NET 9.0 / C# 12 |
| Database | PostgreSQL 16 (EF Core + Npgsql) |
| API | ASP.NET Core Minimal APIs |
| Service Discovery | Steeltoe Eureka |
| Excel Export | ClosedXML |
| Documentation | Swagger / OpenAPI |
| Container | Docker (multi-stage build) |

---

## Architecture

### Backend - Modular Monolith

The backend follows Clean Architecture and DDD principles. Each module has its own Domain, Application, and Infrastructure layers:

```
backend/src/
├── API/Skylab.Api/               # Entry point, endpoints, DI config
├── Modules/
│   ├── Forms/                    # Form management module
│   │   ├── Forms.Domain/         #   Entities, enums, domain models
│   │   ├── Forms.Application/    #   Services, DTOs, business logic
│   │   └── Forms.Infrastructure/ #   DbContext, entity configs, migrations
│   └── Exports/                  # Excel export module
│       └── Exports.Application/  #   Excel file generation service
└── Shared/                       # Shared libraries across modules
    ├── Skylab.Shared.Domain/
    └── Skylab.Shared.Application/
```

### Frontend - App Router

```
frontend/
├── app/
│   ├── (public)/        # Landing page & form fill routes
│   └── admin/           # Protected dashboard, form editor, responses
├── components/          # Shared UI components
├── lib/                 # API clients, hooks, utilities
└── public/              # Static assets
```

---

## API Overview

### Public Endpoints

| Method | Endpoint | Description |
|--------|----------|------------|
| `GET` | `/api/forms/{id}` | Get form for display |
| `GET` | `/api/forms/{id}/meta` | Get form metadata |
| `POST` | `/api/forms/responses` | Submit a response |

### Admin Endpoints

| Method | Endpoint | Description |
|--------|----------|------------|
| `GET` | `/api/admin/forms/` | List forms (paginated, searchable) |
| `POST` | `/api/admin/forms/` | Create new form |
| `GET` | `/api/admin/forms/{id}` | Get form details |
| `PUT` | `/api/admin/forms/{id}` | Update form |
| `DELETE` | `/api/admin/forms/{id}` | Soft delete form |
| `GET` | `/api/admin/forms/{id}/responses` | List responses |
| `GET` | `/api/admin/forms/{id}/responses/export` | Export to Excel |
| `GET` | `/api/admin/forms/{id}/metrics` | Form metrics |
| `PATCH` | `/api/admin/forms/responses/{id}/status` | Update response status |
| `POST` | `/api/admin/forms/responses/{id}/archive` | Archive response |

Full API documentation is available at `/swagger` when the backend is running.

---

## Authentication & Authorization

- **Authentication** - Keycloak-based SSO via NextAuth.js on the frontend; Bearer token verification via external `super-skylab` service on the backend
- **Authorization** - Role-based access control with three levels:
  - **Owner** - Full control, collaborator management
  - **Editor** - Edit forms, view responses
  - **Viewer** - Read-only access

---

## Getting Started

### Prerequisites

- [Node.js](https://nodejs.org/) 18.17+
- [.NET 9.0 SDK](https://dotnet.microsoft.com/download/dotnet/9.0)
- [PostgreSQL](https://www.postgresql.org/download/)
- Access to a [Keycloak](https://www.keycloak.org/) instance

### Clone with Submodules

```bash
git clone --recurse-submodules https://github.com/skylab-kulubu/skylab-forms.git
cd skylab-forms

# If already cloned without submodules:
git submodule update --init --recursive
```

### Backend

```bash
cd backend

# Restore & run (migrations run automatically on startup)
dotnet restore src/Skylab.sln
dotnet run --project src/API/Skylab.Api
```

Swagger UI will be available at `http://localhost:8080/swagger`.

### Frontend

```bash
cd frontend

# Install dependencies
npm install

# Create .env.local with required variables (see below)
npm run dev
```

The app will be available at `http://localhost:3000`.

### Environment Variables

**Backend:**

| Variable | Description | Required |
|----------|------------|----------|
| `CONNECTION_STRING` | PostgreSQL connection string | Yes |
| `ALLOWED_ORIGIN` | CORS allowed origin | Yes |

**Frontend:**

| Variable | Description | Required |
|----------|------------|----------|
| `NEXT_PUBLIC_API_URL` | Backend API base URL | Yes |
| `NEXT_PUBLIC_KEYCLOAK_ISSUER` | Keycloak realm URL (public) | Yes |
| `KEYCLOAK_CLIENT_ID` | OAuth client ID | Yes |
| `KEYCLOAK_CLIENT_SECRET` | OAuth client secret | Yes |
| `KEYCLOAK_ISSUER` | Keycloak realm URL (server-side) | Yes |
| `AUTH_SECRET` | NextAuth encryption secret | Yes |

### Docker

```bash
# Backend only
docker build -f backend/src/Dockerfile -t skylab-forms-api ./backend
docker run -p 8080:8080 \
  -e CONNECTION_STRING="Host=db;Port=5432;Database=forms_db;Username=admin;Password=skylab" \
  -e ALLOWED_ORIGIN="http://localhost:3000" \
  skylab-forms-api
```

---

## Contributing

See submodules for project structure, architecture details, and contribution guidelines.