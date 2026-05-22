# Multi-Branch Gym Management System

A relational database system designed to manage the data architecture and day-to-day operations of a multi-branch gym organization. This system bridges a normalized relational backend with a fully custom multi-window desktop interface to execute robust administrative data control.

---

## Project Theme & Architecture

The core objective of this project is to implement a robust data architecture for managing an interconnected, multi-branch fitness organization. Built on **SQL Server**, the system models complex multi-entity environments containing:
* Core member portfolios and contact details.
* Trainer shift profiling, scheduling dependencies, and class hierarchies.
* Granular physical asset tracking (gym equipment profiles and specific branch allocations).
* Operational commercial data (membership structures, pricing rules, and ongoing plans).

The platform enforces systematic **data integrity** through normalized table relationships, composite primary keys, foreign key cascading constraints, and targeted check expressions (`CHECK`) to avoid structural data corruption across asynchronous locations.

---

## Management Dashboards & CRUD Capabilities

The application engine exposes a central window interface that anchors five specialized functional sub-dashboards. Each window is dynamically mapped to its respective data models to process full **CRUD (Create, Read, Update, Delete)** interactions without leaving the interface shell.

### 1. Member Management Dashboard
Acts as the central operational gateway for tracking client status. It maintains dual data grid layouts to process atomic entries alongside multi-value multi-line data paths independently.
* **Functional Scope:** Adds/updates full profiles including personal details, specific branch assignment mapping, customized plan hooks, and calendar date selection parameters.
* **Multi-Value Tracking:** Features dedicated sub-routines (`Add Phone` / `Delete Phone`) to scale and modify multiple contact points per single user profile seamlessly.
* **Underlying Model Access:** Writes directly to `Member` and `Member_Phone`. Consumes reference pipelines from `Membership`, `Workout_Plan`, and `Branch_Manager` to safely populate dropdown data options with valid keys at the input threshold.

### 2. Trainer & Workout Plan Management
Engineered with an organized tabbed configuration to combine training staff records and logical fitness schedules within a single form module to preserve strict historical alignment.
* **Manage Trainers Tab:** Allows full creation, removal, and adjustments of staff variables (`First Name`, `Last Name`, `Gender`, `Shift`, `Specialty`, `Phone`, `Branch`). Dropdowns explicitly match available branch identities.
* **Manage Workout Plans Tab:** Constructs actionable timelines mapping `Plan Name`, `Duration` (in weeks), and `Intensity Level` parameters.
* **Underlying Model Access:** Writes to `Trainer` and `Workout_Plan`. Reads `Branch_Manager`. This single form pairing resolves the dense $1:M$ dependency between trainers and plans without causing workflow detachment.

### 3. Class Schedule Management
Houses custom side-by-side matching data tables designed to display active class names alongside live time block instances.
* **Functional Scope:** Registers new classes under selected training instructors, modifies baseline naming records, and appends time tracking matrices by pairing standard calendar days with hour settings.
* **Underlying Model Access:** Intersects `Class` and `Class_Schedule` tables while filtering against the active `Trainer` list. Structural dependencies ensure empty schedule items cannot exist without a core parental class entry.

### 4. Membership Tier Administration
Acts as the isolated reference-data controller for establishing and managing pricing structures, tiers, and service plans.
* **Functional Scope:** Standardizes gym-wide options by defining specific baseline tier properties (`ID`, `Duration` configurations such as 1, 3, or 6 months, and exact `Price`). All available variations display within a single administrative grid.
* **Underlying Model Access:** Reads/Writes exclusively to the standalone `Membership` table. Since this serves as purely independent reference data, it avoids structural external locks and serves as an immutable lookup path for other components.

### 5. Comprehensive Branch & Equipment Management
The largest architectural subsystem in the UI pipeline, functioning as the primary supervisor for physical operational environments, internal asset locations, and localized gym access.
* **Branch Logistics:** Controls macro variables including branch classification types (`Male`, `Female`, `Mixed`), geographical markers (`City`, `Area`), and explicit `Branch Manager` tracking records.
* **Active Offerings Grid:** Displays live class availability mappings (`Offers` table) cross-referencing individual `Branch_ID` paths directly with specific `Class_ID` attributes.
* **Asset Tracking Panel:** Provides a dedicated right-hand interface component for cataloging high-value physical equipment inventories. Tracks individual assets via unique tracking numbers, descriptive names, activation dates, and maintenance cycle parameters.
* **Underlying Model Access:** Modifies `Branch_Manager` and `Equipment` structures while parsing data loops across `Offers` and `Class` tables to form a complete, real-time administrative view of physical gym locations.

---

## Interface System Procedures & Structural Hooks

The backend operational logic relies on a clean, standardized collection of core initialization, data loading, and interactive event procedures structured uniformly across the application forms:

### Code Architecture Pattern
* **Constructor Procedures:** Instantiates forms and configures core data links (e.g., `public Member()`, `public Trainers()`, `ClassScheduler()`).
* **Data Loading & Binding Pipelines:** Runs optimized routines to refresh grids, handle default parameters, and safely fill reference dropdown controls (e.g., `LoadMemberData()`, `LoadEquipment()`, `LoadBranchOptions()`).
* **Event Routines:** Explicitly captures user actions to validate inputs and securely commit database transactions (e.g., `Add_Member_Click`, `btnUpdateEquipment_Click`, `btnDeleteClass.Click`).
