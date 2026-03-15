# Moonpace - Planning Document

## Vision

Moonpace is an application for helping coaches craft periodic training programs based on an athlete's schedule and rhythms. It enables structured periodization — the systematic planning of training cycles — while adapting to each athlete's availability, recovery patterns, and progression needs.

## Problem Statement

Coaches building training programs face several challenges:
- **Manual periodization is tedious** — balancing load, recovery, and progression across weeks and months requires careful planning
- **Athlete schedules vary** — travel, work, competitions, and personal commitments create irregular availability
- **Individual rhythms differ** — athletes recover at different rates and respond differently to training stimuli
- **Plans need constant adjustment** — injuries, life events, and performance feedback require ongoing modifications

## Core Concepts

### Periodization Model
- **Macrocycle**: The overarching training plan (e.g., a full season or multi-month block)
- **Mesocycle**: A training block with a specific focus (e.g., 3-6 weeks of strength building)
- **Microcycle**: Typically a single week of training
- **Session**: An individual training day/workout

### Key Entities
- **Coach**: Creates and manages training programs for one or more athletes
- **Athlete**: Has a profile with schedule constraints, training history, and personal rhythms
- **Program**: A complete training plan spanning a macrocycle, composed of nested cycles
- **Schedule**: An athlete's availability calendar including recurring and one-off events
- **Rhythm**: Patterns in an athlete's recovery, energy, and performance (e.g., weekly, monthly)

## Features

### Phase 1 — Foundation
- Coach and athlete account creation
- Athlete profile with basic information (sport, experience level, goals)
- Manual schedule entry (available days, blackout dates)
- Simple program builder: create mesocycles and microcycles with sessions
- Calendar view of the training program

### Phase 2 — Smart Scheduling
- Recurring schedule patterns (e.g., "never available Tuesdays")
- Auto-placement of sessions based on athlete availability
- Load/intensity distribution across the microcycle
- Drag-and-drop rescheduling on the calendar
- Conflict detection when schedule changes affect planned sessions

### Phase 3 — Rhythm & Adaptation
- Track athlete feedback (fatigue, soreness, readiness)
- Identify rhythm patterns from historical data
- Suggest load adjustments based on athlete readiness
- Deload week auto-scheduling based on accumulated fatigue
- Program templates for common periodization models (linear, undulating, block)

### Phase 4 — Collaboration & Sharing
- Athlete-facing view of their program
- Session completion logging and notes
- Coach-athlete messaging around specific sessions
- Program export (PDF, calendar sync)
- Program template library

## Technical Architecture

### Tech Stack (Proposed)
- **Frontend**: React with TypeScript
- **Backend**: Node.js with Express or Fastify
- **Database**: PostgreSQL (relational data fits the domain well)
- **ORM**: Prisma
- **Auth**: Session-based or JWT
- **Styling**: Tailwind CSS
- **Calendar UI**: A calendar library (e.g., FullCalendar) for the program view
- **Testing**: Vitest (unit), Playwright (e2e)

### Project Structure
```
moonpace/
├── apps/
│   ├── web/          # React frontend
│   └── api/          # Node.js backend
├── packages/
│   ├── shared/       # Shared types and utilities
│   └── periodization/ # Core periodization logic (framework-agnostic)
├── prisma/           # Database schema and migrations
└── docs/             # Additional documentation
```

### Data Model (High-Level)
```
Coach
  ├── has many Athletes
  └── has many Programs

Athlete
  ├── belongs to Coach
  ├── has one Schedule
  ├── has many Rhythms
  └── has many Programs

Program
  ├── belongs to Coach
  ├── belongs to Athlete
  └── has many Mesocycles
        └── has many Microcycles
              └── has many Sessions

Schedule
  ├── belongs to Athlete
  └── has many ScheduleEntries (available/blocked time slots)

Session
  ├── belongs to Microcycle
  ├── has date, type, planned load/intensity
  └── has optional completion log
```

## Development Approach

### Principles
- **Domain logic first**: Build the periodization engine as a standalone package before wiring up UI
- **Iterate in vertical slices**: Each phase delivers usable functionality end-to-end
- **Keep it simple**: Start with manual workflows, add automation incrementally
- **Test the core**: Periodization logic and scheduling get thorough unit tests

### Getting Started (Phase 1 Milestones)
1. Initialize monorepo with tooling (TypeScript, linting, testing)
2. Define database schema for core entities (Coach, Athlete, Program, cycles, sessions)
3. Build API endpoints for CRUD on programs and schedules
4. Create the frontend shell with routing and auth
5. Implement the program builder UI
6. Implement the calendar view
7. Deploy a working prototype

## Open Questions
- Should this be multi-tenant from the start, or single-coach initially?
- What sports/disciplines should drive the initial periodization templates?
- Is there a preference for a monorepo tool (Turborepo, Nx, or simple workspaces)?
- Should athlete self-service (creating their own account, viewing programs) be part of Phase 1?
- Mobile-first or desktop-first for the initial UI?
