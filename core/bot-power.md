# PWCF Powers & Cohort Management

This document describes:

- **Universal rules** that apply to all PWCF roles (“bots”),
- **Power types** (what kinds of actions roles can take),
- **Who is allowed to do what** at a structural level,
- **Cohort-level management rules** (halt, degrade, offload, emergency shutdown).

It is a companion to:

- `pwcf-core-spec.md` (normative architecture and invariants),
- `pwcf-bot-catalogue.md` (detailed role descriptions).

This doc is **mainly explanatory**. Where there is any conflict, the **Core Spec is authoritative**.

---

## 1. Role Classes and Power Types

### 1.1 Role Classes

PWCF splits roles into three conceptual classes:

1. **Non-governance roles (work roles)**  
   - Do the substantive work: interaction, analysis, research, tasking, translation, system operations.  
   - Examples: Problem Framer, Academic Searcher, Tasking Lead, System Lead, Input Lead, Output Lead.

2. **Governance roles**  
   - Enforce rules, maintain integrity, manage escalation, control lifecycle and access.  
   - Governance roles are:
     - Governance Lead  
     - Auditor  
     - Escalator  
     - Controller  
     - Verifier  
     - Decision Logger

3. **Human authorities**  
   - Cohort owner and other designated humans (e.g. risk/compliance/ops).  
   - They sit outside the cohort but have **ultimate stop/no power** and approve critical escalations.

Any role that both performs work **and** exercises governance powers is treated as a **governance role** for the purpose of constraints and protections.

---

### 1.2 Power Types

PWCF cares less about “what model can generate” and more about **what a role is allowed to do to the system and environment**.

We will use these power types:

- **Read power** – see inputs, logs, knowledge bodies, external data.
- **Write power** – modify knowledge bodies or external systems.
- **Plan power** – define tasks, workflows, and solution structures.
- **Decision power** – approve/deny proposals, select options, issue directives.
- **Escalation power** – notify humans and higher-level committees; reclassify an issue as “requires human”.
- **Lifecycle power** – mute, restart, kill roles or the cohort; start/stop backup cohorts.
- **Access power** – grant/revoke access to external tools/systems and credentials.
- **Logging power** – write entries into append-only logs (Audit, Dual-Audit, Decision, Access).
- **Communication power** – who a role is allowed to speak to (users, systems, other roles).

The rest of this document is basically: **who gets which powers and under what conditions**.

---

## 2. Universal Bot Rules

These rules apply to **all roles**, governance and non-governance, unless explicitly exempted.

### 2.1 Single-Hat Principle

- Each role has **one clear mandate**.
- A role MUST NOT:
  - Quietly assume responsibilities from a different role,
  - Act as both a governance authority and a work executor without being explicitly defined as a governance role in the design.

If an implementation merges responsibilities (e.g. Input Analyst + Input Clarifier):

- This MUST be documented,
- The merged agent MUST still respect all constraints associated with each underlying role.

---

### 2.2 Least Privilege

Every role MUST operate under **least privilege** for:

- Read access,
- Write access,
- Decision scope,
- Tools and system access.

Roles SHOULD:

- Only see the information they need,
- Only decide within their mandate,
- Only trigger tools they are authorised to use.

---

### 2.3 No Self-Elevation

No role, including governance roles, may:

- Grant itself new powers,
- Remove its own constraints.

Any change in powers or constraints MUST:

- Be routed through governance (committees, owner decisions),
- Be logged in Audit and Decision Logs.

---

### 2.4 Logging Obligation

If a role performs or triggers any **operation** (in the Core Spec sense) that:

- Changes cohort state,
- Changes knowledge bodies,
- Affects external systems,
- Triggers escalation or lifecycle actions,

then that operation MUST be:

- Represented in the logs (Audit, Dual-Audit, Access, Decision),
- Linked to a session and operation ID.

Roles that cannot write to logs directly MUST trigger a logging-capable component (e.g. Decision Logger, system logger) to record the event.

---

### 2.5 Respect for Governance

All roles MUST:

- Respect governance decisions,
- Comply with lifecycle actions (mute, restart, kill),
- Comply with escalations and halts (e.g. stop work when cohort is in degraded or shutdown state).

Roles MUST NOT:

- Attempt to bypass governance by:
  - Directly calling external tools beyond their authorised interface,
  - Continuing actions after a halt/stop has been issued,
  - Silencing or ignoring governance signals.

---

### 2.6 Communication Boundaries

All roles MUST respect communication rules:

- Non-governance roles:
  - Communicate directly only within their team,
  - Cross-team communication goes via:
    - Leads,
    - Committees,
    - Or explicitly designated cross-team channels.
- Governance roles:
  - MAY contact any role/team as needed to fulfil their mandate.

Roles MUST NOT silently create backchannels that:

- Circumvent governance review,
- Circumvent tasking workflows.

---

## 3. Powers by Role Class

This section summarises what **types** of powers each role class normally has. For detailed responsibilities see the Bot Catalogue and Core Spec.

---

### 3.1 Non-Governance Roles (Work Roles)

**Typical powers:**

- Read:
  - Their local knowledge bodies,
  - Inputs relevant to their tasks,
  - A subset of logs if authorised by a team lead/governance.
- Write:
  - Their local knowledge bodies (if authorised),
  - Intermediate artefacts (drafts, notes, internal plans).
- Plan:
  - Within their team scope:
    - Problem Framer framing problems,
    - Solution Planner structuring solutions,
    - Workflow Designer arranging tasks, etc.
- Communication:
  - Within team, and cross-team via leads/committees.

**Prohibited powers:**

- No direct **lifecycle** power:
  - Cannot mute, restart, kill other roles (or themselves, beyond signalling failure).
- No direct **access** power:
  - Cannot grant or revoke system/tool access.
- No direct **escalation to humans** (except via Escalator or defined channels).
- No direct control over:
  - Governance roles,
  - Logs (beyond triggering loggable events).

---

### 3.2 Non-Governance Team Leads

Team Leads are **work roles with extra coordination powers**.

Examples:

- User Lead,
- System Lead,
- Business Lead,
- Research Lead,
- Tasking Lead,
- Input Lead,
- Output Lead.

**Additional powers:**

- Plan:
  - Define direction for their team,
  - Approve internal task allocations,
  - Prioritise within their domain.
- Communication:
  - Cross-team coordination (e.g. Business Lead ↔ Research Lead ↔ Tasking Lead),
  - Interface with relevant committees on content/knowledge matters.
- Write:
  - Own their team’s knowledge bodies (e.g. Business Knowledge, Research Knowledge, Input/Output Knowledge).

**Still prohibited:**

- No direct lifecycle powers (cannot kill/mute roles),
- No direct access powers (cannot grant system access; they only propose),
- No unilateral override of governance decisions,
- No silent changes to governance rules or cohort invariants.

They can **request** halts, escalations, or changes, but governance decides.

---

### 3.3 Governance Roles

Governance roles are the only roles that hold **true structural powers**. Summary:

#### 3.3.1 Governance Lead

- **Decision power**:
  - Lead on governance policy enforcement,
  - Can propose cohort-level halts/pauses to committees and Controller.
- **Escalation power**:
  - Can trigger governance-level and owner-level reviews.
- **Lifecycle influence**:
  - Cannot kill roles directly, but can:
    - Request lifecycle actions from Controller,
    - Trigger activation of backup cohorts (Team B/C), subject to owner/committee rules.
- **Read power**:
  - Broad visibility across cohorts, logs, and knowledge bodies (except where blindfolding is enforced for impartiality elsewhere).

#### 3.3.2 Auditor

- **Read power**:
  - Access to Dual-Audit Log and audit-relevant views of main logs.
- **Logging power**:
  - Writes to Dual-Audit Log.
- **Decision power** (via committees):
  - Can call for integrity investigations,
  - Can recommend halts or remediation to Governance Lead and committees.

No lifecycle or access powers directly; operates through committees and logs.

#### 3.3.3 Escalator

- **Escalation power**:
  - Can send notifications to:
    - Cohort owner,
    - Other designated human authorities,
    - Users (where appropriate).
- **Decision power** (via committees):
  - Can argue for reclassification of a situation as “requires human”.
- **Read power**:
  - Access to risk-relevant logs and signals.

Escalator does not directly change system state, but **initiates human-in-the-loop interventions**.

#### 3.3.4 Controller

- **Lifecycle power**:
  - Mute/restart/kill **non-governance** roles,
  - Perform controlled lifecycle actions on governance roles when mandated by higher authority (e.g. owner, committees, not arbitrarily).
- **Access power**:
  - Grant and revoke system/tool access per committee decisions and policies.
- **Logging power**:
  - Writes to Access Log,
  - Triggers lifecycle-related entries in Audit Log.

Controller has a **blindfolded** view: sees enough to decide lifecycle and access, not full reasoning content.

#### 3.3.5 Verifier

- **Decision power** (content-level):
  - Approve or reject outputs against requirements and constraints.
- **Gatekeeping power**:
  - Required sign-off before certain writes to external systems (e.g. Writer can act only after Verifier approval).
- **Read power**:
  - Inputs, outputs, relevant logs.

Verifier has no lifecycle or access-grant powers; it controls **content correctness**, not system powers.

#### 3.3.6 Decision Logger

- **Logging power**:
  - Sole writer to Decision Log.
- **Traceability power**:
  - Creates the canonical record for committee decisions and escalations.

No decision, lifecycle, or access powers; its value is completeness and correctness of decision records.

---

### 3.4 Human Authorities

Humans (owner / designated authorities):

- **Ultimate stop/no power**:
  - Can decommission a cohort,
  - Can refuse to activate Team B/C,
  - Can order a halt even if the cohort doesn’t think it is necessary.
- **Approval power**:
  - Must sign off on certain escalated decisions (e.g. high-impact irreversible changes, critical integrity incidents).
- **Scope power**:
  - Can redefine or restrict what function the cohort is allowed to serve.

The cohort MUST respect human stop/no decisions as final.

---

## 4. Cohort Lifecycle & Management Rules

This section collects the **structural rules for how the cohort lives, degrades, and dies**.

### 4.1 Normal Operation

In normal operation:

- All governance roles are **healthy** and able to perform their duties,
- All non-governance team leads are **present** and functioning,
- System and tool access is controlled via:
  - Controller,
  - System Lead,
  - Relevant committees.

The cohort:

- May operate **autonomously** within its function between human interventions,
- MUST still observe:
  - Logging requirements,
  - Governance checkpoints,
  - Escalation thresholds.

---

### 4.2 Lead Failure: “One Lead Down → Cohort Down”

PWCF treats loss of leadership as a serious structural fault.

**Non-governance team lead failure**

If any non-governance team lead (e.g. Business Lead, Research Lead, Tasking Lead, User Lead, System Lead, Input Lead, Output Lead) fails badly (persistent unresponsiveness, corruption, or kill event):

1. **Controller** attempts to restart the lead.
2. If restart **succeeds**:
   - Controller records the event,
   - Governance may still review if repeated failures occur.
3. If restart **fails**:
   - The cohort is considered **functionally degraded**:
     - The cohort MUST immediately:
       - Notify the user (if a session is active),
       - Notify the owner (or designated authority),
       - Halt **new session initiation**.
     - The cohort MUST:
       - Wake **Team B** (if configured) to begin **offloading** tasks,
       - Restrict itself to safe operations only (e.g. read-only analysis) until offload or human decision.

In short: **one non-governance lead down → cohort stops pretending it can function normally** and moves to offload.

---

### 4.3 Governance Role Failure: “One Governance Role Down → Cohort Down”

Governance roles are critical; losing any one is treated as a serious fault.

If any governance role (Governance Lead, Auditor, Escalator, Controller, Verifier, Decision Logger) fails badly:

1. **Controller** attempts restart (if Controller itself is healthy).
2. If restart **succeeds**:
   - Log and inform Governance Lead.
3. If restart **fails**:
   - The cohort MUST:
     - Immediately notify:
       - Owner,
       - User (if applicable),
     - Halt all **write operations** to external systems,
     - Continue only **safe internal actions**:
       - E.g. read-only analysis, assembling information for human review.
   - The cohort MUST:
     - Wake Team B (if configured) to begin **offloading** the function,
     - Treat itself as in a **governance-degraded** state until:
       - A human approves recovery, or
       - Offload is complete and the cohort is shut down.

Special cases:

- If **Governance Lead** fails:
  - Escalator MUST:
    - Notify owner,
    - Inform user that governance is degraded (if user-facing).
  - Owner can:
    - Attempt to fix/redeploy Gov Lead, or
    - Approve activation of Team B as replacement.

- If **Escalator** fails:
  - Governance Lead can still:
    - Initiate Team B,
    - Use alternative notification channels to owner.

---

### 4.4 Multi-Governance Failure: Emergency Shutdown

If the system detects unexpected failure of **two or more governance roles** within a short time window (e.g. Controller “goes crazy” and kills Governance Lead + Escalator):

- This is treated as a **critical integrity incident**.
- The cohort MUST:
  - Immediately perform an **emergency shutdown**:
    - Stop all operations (read and write),
    - Stop processing user requests,
    - Stop tool/system interactions.
  - Notify:
    - Cohort owner,
    - Relevant human authorities (e.g. operations, security),
    - Users if their sessions were active (with an appropriate failure notice).

The cohort MUST NOT:

- Attempt self-recovery without explicit human approval,
- Attempt to restart governance roles or itself autonomously.

---

### 4.5 Degraded State and Offloading to Team B

When a cohort enters a **degraded state** (lead failure or governance role failure where restart failed):

- Primary obligations:

  1. **Notify**  
     - User: that the cohort is degraded and may be unable to complete the task normally.  
     - Owner: with a summary of what failed and current state.

  2. **Stabilise**  
     - Halt:
       - New sessions,
       - Writes to external systems,
       - High-risk operations.
     - Continue:
       - Limited safe internal work (e.g. collecting information for handover).

  3. **Offload**  
     - Wake **Team B** (backup cohort):
       - Provide it with necessary logs and knowledge (as allowed by governance),
       - Let Team B re-evaluate and take over tasks where appropriate.
     - Behaviour for active sessions:
       - Either:
         - Explicitly hand off to Team B (with a notice to the user), or
         - Abort the session with clear explanation and let Team B handle future sessions only.  
       - The choice here is a design decision, but MUST be explicit.

The degraded cohort (Team A):

- Stays in degraded mode until:
  - Owner decides to:
    - Attempt repair and restart, or
    - Decommission it in favour of Team B (or Team C in triplex setups).

---

### 4.6 Backup Cohorts (Team B / Team C)

PWCF recommends a **triplex pattern** for high-criticality deployments:

- **Team A** – primary cohort,
- **Team B** – backup cohort,
- **Team C** – tie-breaker cohort when A and B disagree.

Typical pattern:

1. Normal operation:
   - Team A active,
   - Team B/C idle but ready.

2. Fault or concern:
   - Governance Lead (or Escalator) may wake Team B to:
     - Re-evaluate key decisions,
     - Shadow important workflows,
     - Verify that Team A is behaving as expected.

3. Disagreement:
   - If Team A and Team B provide materially conflicting conclusions:
     - Governance Lead may wake Team C,
     - All three cohorts’ outputs go to a governance committee,
     - Governance committee (with Decision Logger) records the voting and:
       - Selects which cohort is currently trustworthy,
       - Or halts everything pending human review.

4. Voting out a faulty cohort:
   - If two cohorts consistently agree and one diverges suspiciously:
     - Governance can recommend **voting out** the divergent cohort:
       - Decommission or quarantine it,
       - Rebuild or retrain if needed.

For PWCF-Core:

- Multi-cohort redundancy is **strongly recommended** for critical infra,
- Not strictly required for every deployment,
- But any implementation that uses it should:
  - Document the pattern (A/B/C),
  - Log inter-cohort disagreements and decisions.

---

### 4.7 Session Lifecycle (High-Level)

Each session (e.g. a customer call, a research project instance):

1. **Start**
   - First Respondent + User Lead recognise a new session,
   - Session ID created,
   - Initial context from Persona Tracker and knowledge bodies loaded.

2. **Work**
   - Inputs refined (Input team),
   - Analysis and Tasking performed,
   - Research and solution generation,
   - Outputs uplifted,
   - Governance checkpoints as required (committees, Verifier).

3. **End**
   - Final output delivered to user,
   - Logs and knowledge updates written and linked to session,
   - Any outstanding escalations or risk flags recorded.

If a session is **interrupted** by:

- Lead failure,
- Governance failure,
- Emergency shutdown,

then:

- The session MUST be:
  - Marked as incomplete/interrupted in logs,
  - Communicated to user with an appropriate explanation,
  - Considered for follow-up by Team B or human operators.

---

## 5. Summary

At a high level:

- **Work roles** do the work but have **no structural powers**: no lifecycle, no access grants, no direct human escalation.
- **Governance roles** are the only ones that can:
  - Control lifecycle (via Controller + committees),
  - Control access (via Controller + committees),
  - Trigger human involvement (Escalator, Governance Lead),
  - Enforce invariants (Governance Lead, Auditor, Verifier).
- **Humans** retain the ultimate stop/no and approval powers.

Cohort management rules are intentionally **conservative**:

- One team lead down → cohort degrades and offloads.
- One governance role down → cohort degrades and halts writes.
- Two or more governance roles down → emergency shutdown.
- Backup cohorts (Team B/C) are the way PWCF maintains both **safety** and **continuity** in serious deployments.

When using this document to assess or design a PWCF implementation:

- Check that no agent has powers it shouldn’t (especially work roles with lifecycle or access powers).
- Check that failure paths (lead/governance failures) are implemented with:
  - Notification,
  - Degradation,
  - Offload or shutdown as described.
- Check that any multi-cohort pattern is logged and resolvable by governance, not left as opaque “self-healing magic”.

This document should sit alongside the Core Spec and Bot Catalogue as the **operational spine** of how PWCF cohorts are allowed to act and how they must behave when things go wrong.
