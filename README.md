🚀 Email Cadence System

Temporal + NestJS + Next.js Monorepo

A full-stack email cadence orchestration system built using:

Next.js (Frontend)

NestJS (REST API)

Temporal Cloud (Workflow engine)

Temporal Worker (Background execution)

This system supports:

Creating multi-step cadences

Enrolling contacts

Executing long-running workflows

Updating cadence steps while workflows are running

Real-time workflow state tracking


Architecture Overview
Frontend (Next.js)
        │
        ▼
API (NestJS)
        │
        ▼
Temporal Cloud
        │
        ▼
Worker (Node.js)

Flow:

Frontend calls API

API starts a Temporal workflow

Worker polls EMAIL_TASK_QUEUE

Workflow executes:

SEND_EMAIL activity (mocked)

WAIT step (Temporal sleep)

Workflow state is queryable via API

Cadence can be updated mid-execution via workflow signal


🔐 Environment Variables

Create a .env file in the root:

# Temporal Cloud
TEMPORAL_ADDRESS=ap-northeast-1.aws.api.temporal.io:7233
TEMPORAL_NAMESPACE=your-namespace
TEMPORAL_TASK_QUEUE=EMAIL_TASK_QUEUE
TEMPORAL_API_KEY=your-api-key

# API
PORT_API=3001

.env is ignored via .gitignore.


Installation

From root:
npm install

npm run dev


🧠 Core Concepts Implemented
1️⃣ Workflow State Management

Each workflow tracks:

currentStepIndex

status

steps

stepsVersion

This allows:

Resume after crash (Temporal durability)

Mid-flight updates

Accurate progress tracking

2️⃣ Dynamic Cadence Updates

Workflows support in-flight updates via Temporal signals.

When /enrollments/:id/update-cadence is called:

New steps are sent as a signal

stepsVersion increments

Workflow continues from currentStepIndex

New steps are respected

This demonstrates safe long-running workflow mutation.

3️⃣ Activities

SEND_EMAIL → Mocked email send (console log)

WAIT → Uses Temporal sleep (durable timer)





🧪 How to Test

Run all services

Open http://localhost:3000

Create cadence

Enroll contact

Watch worker logs execute steps

Update cadence mid-run

Observe updated execution

You can also verify workflow execution in Temporal Cloud UI.