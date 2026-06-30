# ProfitLens Architecture

## Overview

ProfitLens is a Shopify Embedded SaaS platform focused on business intelligence.

Instead of managing store operations, ProfitLens analyzes operational data and transforms it into actionable financial insights.

Version 1 ships with a single analytics module:

* Return Intelligence

Future versions will extend the platform with additional analytics modules without changing the core architecture.

---

# Architecture Principles

The system follows these principles:

* Feature-first architecture
* Domain-driven organization
* Modular design
* Separation of concerns
* SOLID principles
* High cohesion
* Low coupling
* Scalable background processing
* Read-only analytics whenever possible

---

# Core Layers

The application is divided into independent layers.

## UI Layer

Responsibilities

* Polaris UI
* User interaction
* Forms
* Tables
* Charts
* Filters

Never performs business logic.

---

## Route Layer

Responsibilities

* HTTP requests
* Validation
* Authentication
* Calling services

Routes never access the database directly.

---

## Service Layer

Responsibilities

* Business logic
* Analytics calculations
* Coordinating repositories
* Validation
* Domain workflows

Services never know UI details.

---

## Repository Layer

Responsibilities

* Database access
* Prisma queries
* Data persistence

Repositories never contain business logic.

---

## Domain Layer

Contains

* Domain models
* Business rules
* Value objects
* Analytics calculations

No framework-specific code.

---

# Feature Modules

Every feature owns its own implementation.

Example:

returns/

* routes
* services
* repositories
* domain
* dto
* components
* jobs
* module.ts

Future modules follow the exact same structure.

---

# Shared Modules

Shared code belongs in:

core/

Contains

* logger
* configuration
* module registry
* errors
* permissions
* scheduler
* constants

No business logic.

---

# Background Workers

Heavy calculations never execute inside HTTP requests.

Workers are responsible for

* Store synchronization
* Metrics generation
* Daily aggregation
* Scheduled reports

Workers must be stateless.

---

# Module Registry

Every feature registers itself.

The application shell discovers modules dynamically.

Adding a new analytics module must not require modifying the dashboard shell.

---

# Event Flow

Shopify

↓

Webhook

↓

Queue

↓

Worker

↓

Database

↓

Analytics Engine

↓

Dashboard

---

# Analytics Philosophy

ProfitLens never changes Shopify data.

It observes.

Calculates.

Analyzes.

Reports.

The platform is read-only whenever possible.

---

# Scalability

Every analytics module should be deployable independently in the future.

Background processing must support horizontal scaling.

Database queries should be optimized for analytical workloads.

---

# Security

Least privilege.

Validate every webhook.

Verify Shopify signatures.

Never trust client input.

Encrypt sensitive configuration.

---

# Future Modules

* Profit Analytics
* Margin Alerts
* Dead Stock Intelligence
* Customer Profitability
* Inventory Intelligence

These modules must integrate without changing existing feature boundaries.

---

# Architecture Status

Version 1.0

Approved as the architectural foundation of ProfitLens.
