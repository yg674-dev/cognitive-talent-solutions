# SmartMatch — AI-Powered Mentor & Cross-Team Collaboration Workflows

**Product Proposal for Cognitive Talent Solutions (CTS)**
Built on Cognitive Network Analyzer™ + ONA by CTS & ServiceNow
**Date:** April 5, 2026

---

## Executive Summary

CTS already has a proven foundation: ONA data that maps informal organizational networks, a certified ServiceNow integration, and a patent-pending Modular AI Agent Framework. What's missing is a closed-loop, action-oriented recommendation system that takes those network insights and automatically surfaces the *right mentor* and the *right cross-team collaborator* for each employee — with HR workflows to act on those recommendations in ServiceNow without manual analysis.

This proposal outlines **SmartMatch**, a two-module recommendation workflow layer built on top of ONA by CTS & ServiceNow — turning insight dashboards into automated HR actions.

---

## Problem Statement

| Pain Point | Current State | Impact |
|---|---|---|
| Mentor matching is manual | HR assigns mentors based on org chart proximity or manager nominations | Poor fit, low engagement, mentees left unmatched |
| Cross-team collaboration is accidental | Employees discover cross-functional peers through hallway conversations or Slack luck | Siloed knowledge, duplicated work, missed innovation |
| ONA insights stay in dashboards | Analysts generate reports, but recommendations don't flow into HR workflows | Insight-to-action gap; data doesn't change behavior |

---

## Solution: SmartMatch — Two Workflow Modules

### Module 1: MentorMatch — Intelligent Mentor Pairing

**What it does:** Automatically recommends the top 3 mentor candidates for each employee, ranked by network compatibility, skill complementarity, and availability signals.

**Data inputs (combining Active + Passive ONA):**

- **Network graph data** — who the mentee is already connected to (to avoid redundant pairings) and who they are *not* connected to but are adjacent to (bridging opportunities)
- **Skill gap signals** — from ServiceNow Growth & Development module (current skills vs. target role skills)
- **Engagement signals** — passive ONA collaboration frequency, email/meeting patterns indicating mentor bandwidth
- **Past mentorship outcomes** — historical match quality scores and completion rates fed back as training signal

**Recommendation scoring:**

```
MentorScore = α · NetworkBridgeScore
            + β · SkillComplementarityScore
            + γ · AvailabilityScore
            + δ · DiversityScore (dept, tenure, geography)
```

Weights (α, β, γ, δ) are tunable per organization's goals — e.g., a company in M&A mode weights `NetworkBridgeScore` higher to break acquired-company silos.

**ServiceNow workflow:**

1. **Trigger:** New hire starts onboarding in Employee Journey Management, or employee enrolls in a Growth & Development plan
2. SmartMatch agent queries ONA graph → generates ranked mentor list with plain-language explanations
3. HR receives a ServiceNow task with ranked list, a "Why this mentor?" card, and approve/swap actions
4. Upon approval, ServiceNow auto-sends introduction messages, schedules first meeting, and creates a mentorship record
5. Check-in nudges at 30/60/90 days with a feedback loop that retrains the model over time

**Expected outcomes** (building on existing CTS benchmarks):
- Extend the current 40% time-to-productivity improvement to sustained development tracks, not just onboarding
- Reduce mentor-mismatch churn (estimated 15–25% of mentorships abandoned in first 60 days industry-wide)

---

### Module 2: TeamBridge — Cross-Team Collaboration Recommendations

**What it does:** Proactively surfaces employees in other teams who share relevant project contexts, complementary expertise, or are structural bridges — turning ONA's structural hole detection into actionable "People You Should Know" suggestions.

**Data inputs:**

- **Structural hole detection** — ONA identifies employees sitting between disconnected clusters (Burt structural holes); these are your highest-value bridge candidates
- **Project/ticket metadata** — ServiceNow project records, incident categories, and skills tags to find thematic overlap across departments
- **Collaboration velocity** — passive ONA signals showing which teams are collaborating less than network structure would predict (cold connections worth warming)
- **Employee-stated interests** — from Active ONA surveys and Growth & Development goals

**Two recommendation modes:**

| Mode | Use Case | Algorithm |
|---|---|---|
| **Serendipity Mode** | Innovation, knowledge sharing | Collaborative filtering on project tag overlap + network distance |
| **Targeted Mode** | Specific skill gap or project need | Content-based filtering on skill profiles + structural hole bridging |

Inspired by proven recommendation system architectures (Netflix/LinkedIn-style), adapted for organizational graphs:
- **Item** = potential collaborator profile
- **User** = employee seeking connection
- **Signal matrix** = co-participation in projects, shared skills, mutual connections

**ServiceNow workflow:**

1. **Trigger:** Weekly digest (configurable), or event-driven (new project started, role change, onboarding milestone)
2. SmartMatch agent queries ONA graph → generates "People You Should Know" cards with context ("Alex in Engineering solved a similar problem in Q3 — you're both working on customer data pipelines")
3. Delivered via **ServiceNow Employee Center** as a widget or push notification
4. Employee can one-click request a warm intro (HR or shared manager gets notified), or save for later
5. Opt-in office hours scheduling: if recommended collaborator has flagged availability, direct calendar booking enabled

---

## Technical Architecture

```
┌─────────────────────────────────────────────────────┐
│                  ServiceNow Now Platform              │
│  Employee Journey │ Manager Hub │ Growth & Dev │ EC   │
└────────────┬──────────────────────────────────┬──────┘
             │  Triggers & Actions               │ UI Widgets
             ▼                                   ▼
┌─────────────────────────────────────────────────────┐
│           SmartMatch Workflow Layer (NEW)            │
│  ┌──────────────────┐    ┌───────────────────────┐  │
│  │   MentorMatch    │    │      TeamBridge        │  │
│  │  Ranking Engine  │    │  Rec Engine (CF+CB)    │  │
│  └────────┬─────────┘    └───────────┬───────────┘  │
└───────────┼──────────────────────────┼──────────────┘
            ▼                          ▼
┌─────────────────────────────────────────────────────┐
│         Cognitive Network Analyzer™ (CTS ONA)        │
│   Active ONA (surveys) + Passive ONA (signals)       │
│   Graph DB: nodes=employees, edges=collaboration     │
└─────────────────────────────────────────────────────┘
```

Built using CTS's existing **Modular AI Agent Framework** — each module is an independent agent that can be activated, tuned, or replaced without disrupting the other.

---

## Privacy & Ethical Design

Given that passive ONA involves behavioral signal collection, three guardrails are essential:

1. **Aggregate-only passive signals** — individual email/meeting data is never exposed; only graph-level scores surface in recommendations
2. **Explainable recommendations** — every suggestion shows a plain-language reason ("Based on your skills gap in cloud architecture and Maria's expertise flagged in your growth plan")
3. **Employee opt-out** — employees can opt out of cross-team discovery mode; mentorship matching remains HR-driven

---

## Business Case

| Metric | Benchmark | SmartMatch Target |
|---|---|---|
| Time-to-productivity (new hires) | 40% improvement (existing CTS data) | Maintain + extend to 6-month trajectory |
| Attrition reduction | 20% (existing CTS data) | Add mentor-mismatch reduction layer |
| Cross-team project success rate | Industry avg: 54% | Target 70%+ via pre-seeded network bridges |
| HR hours spent on mentor matching | ~4–8 hrs/cohort (manual) | < 30 min/cohort (review + approve) |

---

## Phased Rollout

| Phase | Timeline | Scope |
|---|---|---|
| **Phase 1: MentorMatch MVP** | Months 1–3 | Onboarding flow only; HR-approved matching; ServiceNow EJM integration |
| **Phase 2: MentorMatch Full** | Months 4–6 | Extend to Growth & Development plans; feedback loop + model retraining |
| **Phase 3: TeamBridge MVP** | Months 7–9 | Serendipity Mode digest; Employee Center widget |
| **Phase 4: TeamBridge Full** | Months 10–12 | Targeted Mode; calendar booking; M&A/restructuring use case |

---

## Why CTS is Uniquely Positioned

- **ONA data moat** — no competitor has the same network graph fidelity; this is the core input to the recommendation engine
- **ServiceNow certified app** — already in the Store; SmartMatch is an extension, not a new product to sell
- **Modular AI Agent Framework** — patent-pending architecture maps directly to a two-module agent design
- **Proven enterprise client base** (Medtronic, Coca-Cola, PwC, Thermo Fisher) — ready pilots with measurable baseline data

---

## Recommended Next Steps

1. **Identify a pilot client** from existing base with active ServiceNow + ONA deployment (PwC or Medtronic are strong candidates given their onboarding scale)
2. **Define the recommendation model schema** — map ONA graph fields to mentor/collaborator scoring dimensions
3. **Design the HR approval UX** in ServiceNow Employee Center — keep it a one-screen, three-click flow
4. **Set feedback loop instrumentation from day one** — mentorship outcomes, collaboration initiated, opt-out rates

---

## Strategic Positioning

SmartMatch is CTS's natural next product evolution: from **insight platform** to **action platform** — closing the gap between knowing who should connect and actually making it happen.

---

*Confidential · Cognitive Talent Solutions · April 2026*
*Built on Cognitive Network Analyzer™ + ONA by CTS & ServiceNow*
