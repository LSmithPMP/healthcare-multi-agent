# Product Requirements Document
## Healthcare Multi-Agent System

| Field | Detail |
|-------|--------|
| Project Name | Healthcare Multi-Agent System |
| Document Type | Product Requirements Document (PRD) |
| Version | 1.0 |
| Date | March 2026 |
| Program | Applied Agentic AI |
| Institution | Interview Kickstart |
| Week | Week 3 — Multi-Agent Orchestration |
| Architecture | Coordinator-Worker · Multi-Intent Routing |
| Primary Language | JavaScript (n8n) · GPT-4o · GPT-4o-mini |
| Status | Production — Deployed |

---

## 1. Executive Summary

The Healthcare Multi-Agent System is a conversational AI platform built in n8n that orchestrates three specialist healthcare agents through a central Coordinator Agent. Users interact through a single chat interface — the coordinator classifies intent and routes to the appropriate specialist, or dispatches multiple agents simultaneously for multi-intent queries.

The system is backed by five live Google Sheets datasets covering 3,401 hospitals, 28 doctors across 12 medical specialties, appointment availability, diagnostic test preparation, and health package information. Email delivery via Gmail integration enables direct-to-patient communication of test preparation instructions.

---

## 2. Problem Statement

Healthcare information is fragmented across disconnected systems — appointment scheduling, hospital quality data, and diagnostic preparation exist in separate silos requiring manual navigation. Patients face friction finding the right specialist, the highest-rated hospital, or preparation instructions for an upcoming test.

Existing solutions address one domain at a time. The Healthcare Multi-Agent System solves this by deploying three specialist agents under a central coordinator that can handle complex, multi-domain queries in a single conversational turn — while maintaining context across a 10-turn session window.

---

## 3. Objectives

- Route healthcare queries to the correct specialist agent based on intent classification
- Support multi-intent routing — single queries spanning multiple domains dispatch to multiple agents simultaneously
- Provide appointment availability and booking across 28 doctors and 12 medical specialties
- Surface hospital quality ratings, ER locations, and ambulance availability across 3,401 nationwide hospitals
- Deliver diagnostic test preparation instructions and health package information with optional Gmail delivery
- Maintain seamless user experience — internal agent architecture never exposed to the user

---

## 4. User Stories

| ID | User Story | Acceptance Criteria | Priority |
|----|-----------|-------------------|----------|
| US-01 | As a patient, I want to find a cardiologist available this week so I can schedule an appointment. | Doctor Booking Agent returns available cardiologists with open slots; user can confirm booking. | Must Have |
| US-02 | As a user, I want to find the highest-rated hospital near my ZIP code. | Hospital & ER Agent returns hospitals ranked by CMS star rating for the requested area. | Must Have |
| US-03 | As a patient, I want test preparation instructions emailed to me before my MRI. | Diagnostics Agent retrieves MRI prep instructions and sends via Gmail to patient email. | Must Have |
| US-04 | As a user, I want to ask about a cardiologist and a 5-star hospital in one message. | Coordinator routes to both agents simultaneously; synthesizes unified response. | Must Have |
| US-05 | As a user, I want a seamless experience without knowing there are multiple agents. | Internal agent names never exposed; coordinator presents unified response. | Must Have |
| US-06 | As a user, I want the system to remember what I said earlier in our conversation. | Window Buffer Memory maintains 10-turn context on coordinator. | Should Have |

---

## 5. Agent Architecture

| Agent | Model | Capabilities | Data Coverage |
|-------|-------|-------------|--------------|
| Healthcare Coordinator | GPT-4o | Intent classification, multi-intent routing, session memory (10 turns) | Primary user interface — never answers directly, always delegates |
| Doctor Booking Agent | GPT-4o-mini | Find doctors by specialty, check availability, book appointments | 28 doctors across 12 specialties; appointment slots March 1–7 |
| Hospital & ER Agent | GPT-4o-mini | Find hospitals, compare quality ratings, locate ERs by ZIP | 3,401 hospitals nationwide; 32 ER locations across 13 metros |
| Diagnostics Agent | GPT-4o-mini | Test prep instructions, health packages, email delivery via Gmail | 8 diagnostic test types; 6 health packages; Gmail integration |

---

## 6. Datasets (5 Google Sheets)

| Dataset | Records | Coverage |
|---------|---------|----------|
| Doctor Info | 28 doctors | 12 medical specialties |
| Doctor Availability | Appointment slots | March 1–7, 30-min intervals; is_available flag |
| Hospital General Info | 3,401 hospitals | Nationwide — CMS quality ratings (1–5 stars) |
| Hospital Emergency Data | 32 ER locations | 13 major metro ZIP codes with ambulance availability |
| Hospital Lab Tests | 8 test types | Prep instructions + 6 health packages |

---

## 7. System Design

### 7.1 Routing Logic
The Healthcare Coordinator Agent classifies every user message into one or more routing paths:
- **Doctor Booking Agent** — specialties, availability, appointments, doctor contact information
- **Hospital & ER Agent** — hospitals by location/name, quality ratings, emergency rooms, ambulance availability
- **Diagnostics Agent** — lab tests, prep instructions, health packages, email delivery

### 7.2 Multi-Intent Handling
When a query spans multiple domains, the coordinator dispatches to both relevant agents concurrently and synthesizes a unified, coherent response. The user receives a single answer — not separate agent outputs.

### 7.3 Memory Architecture
Each agent maintains its own Simple Memory Buffer. The coordinator maintains a Window Buffer Memory with a 10-turn context window, enabling coherent multi-turn conversations across routing boundaries.

### 7.4 Fallback Behavior
Queries completely unrelated to healthcare trigger a polite fallback response. The system does not attempt to answer off-topic queries.

---

## 8. Security Notes

- All Google Sheets credentials managed via n8n credential store — never hardcoded
- Gmail integration uses OAuth2 — no plaintext passwords or API keys in workflow nodes
- No real patient data — all datasets contain synthetic/demo data for academic demonstration

---

## 9. Sample Queries

- "Find me a cardiologist available Monday morning"
- "Which hospitals near ZIP 60601 have 5-star ratings and ambulance availability?"
- "What do I need to do to prepare for an MRI?"
- "Book an appointment with a pediatrician and email me the confirmation"
- "Compare the top-rated hospitals in Michigan"

---

## 10. Academic Context

| Field | Detail |
|-------|--------|
| Program | Applied Agentic AI |
| Institution | Interview Kickstart |
| Week | Week 3 — Multi-Agent Orchestration |
| Semester | Spring 2026 |
| Author | Lamonte Smith |
| GitHub | github.com/LSmithPMP/healthcare-multi-agent |

---

*Lamonte Smith · Interview Kickstart — Applied Agentic AI · March 2026 · Confidential*
