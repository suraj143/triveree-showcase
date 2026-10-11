# Soft Data HRMS — UAE Payroll & GCC Extensibility

## Overview

**Soft Data HRMS** includes a **country-aware, configuration-driven payroll engine** with UAE implemented first and GCC strategy stubs for Saudi Arabia, Qatar, Kuwait, Bahrain, and Oman.

Core rule: **no payroll calculations are hardcoded**. Formulas, EOS tiers, overtime multipliers, leave accrual, and WPS export templates live in the database.

## Architecture

```
PayrollEngine
 ├── UAEPayrollStrategy      (enabled)
 ├── SaudiPayrollStrategy    (stub)
 ├── QatarPayrollStrategy    (stub)
 ├── KuwaitPayrollStrategy   (stub)
 ├── BahrainPayrollStrategy  (stub)
 └── OmanPayrollStrategy    (stub)
```

- **Backend:** NestJS + Prisma + PostgreSQL  
- **Frontend:** Angular 20  
- **Tenancy:** `tenantId` on business tables; JWT carries tenant context  
- **PII:** Emirates ID, passport, visa, labour card, IBAN encrypted (AES-256-GCM)

## Demo tenant

| Field | Value |
|---|---|
| Tenant code | `SOFTDATA` |
| Company | Soft Data LLC (Dubai HQ · Abu Dhabi · Riyadh) |
| Password | `Password123!` |

### Demo users & responsibilities

| Email | Role | Persona |
|---|---|---|
| `alex.parker@softdata.demo` | Super Admin | User management, RBAC, audit, system config |
| `maya.reed@softdata.demo` | HR Manager | Employees, analytics, recruitment, reporting |
| `ethan.cole@softdata.demo` | Manager | Team leave/timesheets/performance |
| `noah.bennett@softdata.demo` | Employee | Self-service leave, documents, timesheets |
| `olivia.hart@softdata.demo` | Payroll Viewer | Payroll analytics and payslip review |
| `liam.foster@softdata.demo` | HR Manager | Candidate pipeline and hiring workflow |

## Docker Compose

```bash
cp .env.example .env
docker compose up --build -d db api web
# Web http://localhost:8080  ·  API http://localhost:3000/api/health
FORCE_SEED=true npx prisma db seed   # from backend/ when reseeding Soft Data personas
npm run test:e2e
```
