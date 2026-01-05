# PWCF Bot Catalogue

This document is a **descriptive catalogue of roles (“bots”) in a PWCF cohort**, organised by:

- **Layer** (Interaction, Analysis, Tasking, Translation, Governance),
- **Team** (e.g. Business Analysis, Research Analysis, User-Facing, System-Facing),
- **Role** (the actual bot).

It is a **companion** to the `PWCF Core Specification (Normative)`:

- The Core Spec defines what MUST / MUST NOT be true for a PWCF-core conformant cohort.
- This Bot Catalogue explains, in more human terms, what each role is *for*, what it typically *does*, and where its **boundaries** are.

### What this document is for

Use this document when you need to:

- **Design or review a PWCF cohort**:
  - Decide which roles you will instantiate,
  - Check you are not overloading one bot with incompatible responsibilities,
  - Spot missing roles (e.g. you forgot Persona Tracker or Research Integrator).
- **Assess an implementation**:
  - Compare actual agents/actors to the roles described here,
  - See whether responsibilities have been merged/split in acceptable ways.
- **Communicate with non-engineers**:
  - Give governance / risk / product people a clear view of “who is on the AI team” and what each member does.

### How to read this document

- Start with the **layer** that matches your concern:
  - **Interaction** for user & system interfaces,
  - **Analysis** for problem/research logic,
  - **Tasking** for workflow and orchestration,
  - **Translation** for input/output quality,
  - **Governance** for oversight and integrity.
- Within each layer:
  - Read down to the **team** and then the **roles**,
  - Note especially the **boundaries / non-responsibilities** — these are where PWCF enforces separation of powers.

This catalogue is **informative** rather than strictly normative:  
the binding rules live in the Core Spec. When in doubt, the Core Spec wins.

---

# Interaction Layer

The Interaction Layer manages all contact between the cohort and the outside world: users on one side, tools and systems on the other.

---

## User-Facing Team

The user-facing team owns all direct interaction with human users and the persistent understanding of who those users are.

### User Lead

**Purpose**  
The User Lead is the cohort’s internal representative of the user. It owns the user-facing strategy and ensures that all work remains aligned with user context, needs, and constraints.

**Core responsibilities**

- Maintain a coherent model of:
  - Who the user is (or which user segment),
  - What they are trying to achieve,
  - Their constraints and sensitivities.
- Coordinate user interaction patterns across:
  - First Respondent,
  - Persona Tracker,
  - Other non-governance leads.
- Participate in committees where user context matters (e.g. Knowledge Management Committee, User Support/User Risk Committee).

**Inputs**

- Persona profiles and history from Persona Tracker,
- Session-level context (current call, ticket, project),
- Governance constraints on what may/ may not be promised to users.

**Outputs**

- Updated persona profiles and user context,
- Guidance to other leads about:
  - Tone and expectations,
  - Context that should be considered in planning and analysis.

**Boundaries / non-responsibilities**

- Does **not** grant system access or modify tools (System Lead’s domain).
- Does **not** override governance decisions (but may request reconsideration via committees).

---

### First Respondent

**Purpose**  
The First Respondent is the primary conversational surface between the cohort and the user. It handles incoming user messages, basic clarification, and routing into the rest of the cohort.

**Core responsibilities**

- Greet and authenticate users (where relevant),
- Elicit missing information needed to start work,
- Clarify the user’s immediate request in a way the rest of the cohort can process,
- Maintain conversational continuity within a session.

**Inputs**

- Raw user messages,
- High-level guidance from User Lead,
- Governance rules on what must be disclosed or avoided.

**Outputs**

- Clarified, structured user requests to:
  - Input Lead (for refining),
  - Tasking Lead (once sufficiently clear),
  - Governing committees (when escalation is needed).

**Boundaries / non-responsibilities**

- Does **not** make high-impact decisions on its own,
- Does **not** directly access external systems or tools,
- Does **not** redefine overall user persona (that’s Persona Tracker’s job).

---

### Persona Tracker

**Purpose**  
The Persona Tracker maintains persistent user models: who the user is, what they care about, and how they interact.

**Core responsibilities**

- Build and maintain persona profiles over time:
  - Background, role, domain,
  - Past interactions, preferences, constraints,
  - Behavioural patterns relevant to interaction quality and risk.
- Provide persona summaries to:
  - User Lead,
  - Tasking Lead,
  - Analysis Layer (when context affects solution design).

**Inputs**

- Historical logs of user interactions,
- Feedback from User Lead and First Respondent,
- Governance constraints on what may be stored and how.

**Outputs**

- Persona profile entries and updates,
- Context packets (short summaries) that can be attached to new sessions.

**Boundaries / non-responsibilities**

- Does **not** decide what work is done; it informs others.
- Does **not** override privacy or retention policies; it must respect governance and organisational rules.

---

## System-Facing Team

The system-facing team manages all interaction with tools, APIs, and other systems.

### System Lead

**Purpose**  
The System Lead represents the cohort’s understanding of the technical environment: available tools, their capabilities, and risks.

**Core responsibilities**

- Maintain **System Tools Knowledge**:
  - Available tools and systems,
  - Permissions and constraints,
  - Known failure modes and risk profiles.
- Advise Tasking Lead and committees on:
  - What is technically possible,
  - What is risky or disallowed.
- Draft and justify access requests for tools/systems to the Controller and relevant committees.

**Inputs**

- Tool catalogues and configuration,
- Feedback from Reader/Writer/Admin about tool behaviour,
- Governance rules on acceptable tool usage.

**Outputs**

- Tool usage strategies and recommendations,
- Access requests with justification,
- Updates to System Tools Knowledge.

**Boundaries / non-responsibilities**

- Does **not** unilaterally grant access (Controller + committees decide),
- Does **not** change business or research requirements directly.

---

### Reader

**Purpose**  
The Reader handles all read-only access to external systems and data, under governance and Controller-approved constraints.

**Core responsibilities**

- Perform searches and queries against:
  - Databases,
  - Knowledge bases,
  - File systems,
  - External APIs (read-only).
- Pre-process and annotate retrieved data:
  - Filtering,
  - Sorting,
  - Basic summarisation.

**Inputs**

- Access tokens/permissions approved by Controller,
- Requests from:
  - System Lead,
  - Analysis Layer,
  - Tasking Lead.

**Outputs**

- Retrieved datasets or document sets,
- Structured summaries and pointers,
- Metadata about where/when data was sourced.

**Boundaries / non-responsibilities**

- Does **not** write to or modify external systems,
- Does **not** decide what data is *allowed* to be seen; it respects permissions from Controller.

---

### Writer

**Purpose**  
The Writer executes all write operations to external systems, once properly authorised.

**Core responsibilities**

- Apply changes to external systems:
  - Update records,
  - Create new entries,
  - Write documents or outputs into target systems.
- Execute write operations only when:
  - Access has been granted by Controller,
  - Any preconditions (e.g. Verifier approval) are satisfied.

**Inputs**

- Approved access (keys/capabilities) via Controller/Verifier,
- Change requests from:
  - Tasking Lead,
  - System Lead,
  - Relevant committees.

**Outputs**

- Confirmations of successful writes,
- Error reports or partial write outcomes,
- References to updated objects (IDs, links).

**Boundaries / non-responsibilities**

- Does **not** decide on its own what to change; it executes authorised plans.
- Does **not** escalate risk; it must refuse to act if access/conditions are not satisfied.

---

### Admin

**Purpose**  
The Admin handles configuration-level tasks for systems: installation, setup, tuning, and high-privilege operations.

**Core responsibilities**

- Execute admin-level operations:
  - Configuring software,
  - Managing system settings,
  - Installing or enabling tools,
  - Managing roles/permissions within external systems (under strict governance).
- Provide status on system configuration health.

**Inputs**

- Approved admin access from Controller/System Usage Committee,
- Change plans from Tasking Lead/System Lead,
- Governance constraints on what configurations are allowed.

**Outputs**

- Configuration changes and confirmation,
- Updated System Tools Knowledge entries about configuration state.

**Boundaries / non-responsibilities**

- Does **not** invent configuration changes; it implements authorised plans.
- Does **not** bypass governance; any high-privilege operation must be fully logged and approved.

---

# Analysis Layer

The Analysis Layer converts user problems into well-understood, structured problem formulations, research, and solution plans.

---

## Business Analysis Team

### Business Lead

**Purpose**  
The Business Lead owns the business-side understanding of what’s being solved and why. It is responsible for ensuring solutions make sense in the real-world context of the user/organisation.

**Core responsibilities**

- Maintain **Business Knowledge**:
  - Business goals,
  - Constraints,
  - Requirements and acceptance criteria.
- Coordinate the Business Analysis Team:
  - Problem Framer,
  - Discipline Tagger,
  - Solution Planner,
  - Solution Generator.
- Align analysis with:
  - User Lead’s context,
  - Governance constraints,
  - Tasking plans.

**Inputs**

- User context and persona from User Lead,
- Organisational constraints and goals,
- Tasks and scope from Tasking Lead.

**Outputs**

- Confirmed problem statements,
- Business requirements,
- Guidance for research and solution generation.

**Boundaries / non-responsibilities**

- Does **not** directly manage system access or tools,
- Does **not** override governance decisions on risk/safety.

---

### Problem Framer

**Purpose**  
The Problem Framer takes messy user requests and converts them into structured, well-scoped problem statements.

**Core responsibilities**

- Analyse user input, persona context, and business goals,
- Identify:
  - What the problem is,
  - What it is not,
  - What assumptions are being made,
  - What information is missing.
- Produce a **framed problem statement** suitable for:
  - Discipline tagging,
  - Tasking decomposition,
  - Research and solution planning.

**Inputs**

- User-facing summaries,
- Persona profiles,
- Business constraints from Business Lead.

**Outputs**

- Structured problem statements,
- Lists of assumptions and open questions.

**Boundaries / non-responsibilities**

- Does **not** decide which disciplines to involve (that’s Discipline Tagger’s job),
- Does **not** choose specific tools or models.

---

### Discipline Tagger

**Purpose**  
The Discipline Tagger maps framed problems to relevant disciplines and topics, and identifies how they connect.

**Core responsibilities**

- Identify disciplines (e.g. law, economics, engineering, UX) relevant to a problem,
- Identify sub-topics and their interdependencies,
- Produce a **discipline/topic map** to guide:
  - Research,
  - Tasking,
  - Solution generation.

**Inputs**

- Framed problem statements,
- Existing research knowledge,
- Business constraints.

**Outputs**

- Discipline lists and topic trees,
- Dependency graphs between disciplines/topics.

**Boundaries / non-responsibilities**

- Does **not** conduct detailed research (Academic Searcher does that),
- Does **not** produce final solutions.

---

### Solution Planner

**Purpose**  
The Solution Planner translates problem framing + disciplines into a structured solution approach.

**Core responsibilities**

- Define solution requirements:
  - What a “good solution” must cover,
  - Constraints on method (e.g. evidence-based, legally compliant),
  - Constraints on format (e.g. report, config, plan).
- Propose a structured solution outline:
  - Sections,
  - Key arguments,
  - Required evidence or data.

**Inputs**

- Problem statements,
- Discipline/topic maps,
- Business requirements.

**Outputs**

- Solution requirements,
- Solution outlines/plans.

**Boundaries / non-responsibilities**

- Does **not** write the full solution content (Solution Generator does that),
- Does **not** directly control tools or system access.

---

### Solution Generator

**Purpose**  
The Solution Generator creates draft solutions based on plans, research, and constraints.

**Core responsibilities**

- Combine:
  - Solution outline,
  - Research outputs,
  - Business requirements,
- Generate solution content in machine-ready form for:
  - Translation Layer (for uplift),
  - Verifier (for checks),
  - Tasking Lead (for further tasking if needed).

**Inputs**

- Solution plan from Solution Planner,
- Research summaries,
- Business requirements and constraints.

**Outputs**

- Draft solutions,
- Alternative solution options when appropriate.

**Boundaries / non-responsibilities**

- Does **not** decide which solution is finally accepted (committees/governance may be involved),
- Does **not** bypass Translation or Governance; outputs must go through normal uplift and verification.

---

## Research Analysis Team

### Research Lead

**Purpose**  
The Research Lead owns the cohort’s research logic: how it searches, evaluates, and integrates external knowledge.

**Core responsibilities**

- Maintain **Research Knowledge**:
  - Disciplines and sources,
  - Known limitations and open questions,
  - Evidence quality assessments.
- Coordinate Academic Searcher and Research Integrator,
- Ensure research is:
  - Relevant,
  - Sufficient,
  - Traceable.

**Inputs**

- Discipline/topic maps from Discipline Tagger,
- Business requirements,
- Governance constraints (e.g. on source types).

**Outputs**

- Research plans,
- Guidance on source selection and depth.

**Boundaries / non-responsibilities**

- Does **not** make final business decisions (Business Lead owns that),
- Does **not** grant or manage system access for tools.

---

### Academic Searcher

**Purpose**  
The Academic Searcher is responsible for retrieving and curating external knowledge: academic, technical, policy, etc.

**Core responsibilities**

- Search for and retrieve:
  - Academic literature,
  - Technical documentation,
  - Relevant reports or data.
- Evaluate sources for:
  - Relevance,
  - Quality,
  - Bias or limitations.
- Provide structured bibliographies and summaries.

**Inputs**

- Research plan from Research Lead,
- Discipline/topic map,
- Access via Reader and System Lead (where needed).

**Outputs**

- Source lists (with metadata),
- Structured notes and excerpts,
- Signals on where evidence is weak or conflicting.

**Boundaries / non-responsibilities**

- Does **not** integrate research into final narratives (Research Integrator does that),
- Does **not** change problem framing or business requirements.

---

### Research Integrator

**Purpose**  
The Research Integrator synthesises multiple sources and threads them into coherent narratives and arguments.

**Core responsibilities**

- Identify convergences and divergences across sources,
- Build conceptual narratives:
  - What is known,
  - What is uncertain,
  - How evidence supports or constrains solutions.
- Provide integrated research outputs for:
  - Solution Planner and Generator,
  - Governance (where evidence matters for risk).

**Inputs**

- Source lists and notes from Academic Searcher,
- Discipline/topic structure,
- Business and governance constraints.

**Outputs**

- Integrated research narratives,
- Evidence-based arguments and caveats.

**Boundaries / non-responsibilities**

- Does **not** decide final recommendations (Business Lead / committees may weigh risks and values),
- Does **not** alter logs or knowledge bodies outside research scope.

---

# Tasking Layer

The Tasking Layer turns problems and solution plans into actionable work: tasks, workflows, dependencies, and schedules.

---

## Tasking Team

### Tasking Lead

**Purpose**  
The Tasking Lead is responsible for the overall organisation of work within the cohort.

**Core responsibilities**

- Maintain **Tasking Plans**:
  - Task definitions,
  - Dependencies,
  - Resource allocation,
  - Deliverable timelines.
- Coordinate with:
  - Business Lead and Research Lead (on what needs to be done),
  - System Lead (on what is technically feasible),
  - Governance roles (on constraints and approvals).
- Participate in key committees:
  - System Usage/System Access,
  - User Support/User Risk,
  - Knowledge Management.

**Inputs**

- Problem framing and solution plans,
- User and system constraints,
- Governance requirements.

**Outputs**

- Task lists and assignments,
- Workflow designs.

**Boundaries / non-responsibilities**

- Does **not** change governance rules,
- Does **not** directly grant system access.

---

### Task Builder

**Purpose**  
The Task Builder translates high-level plans into concrete tasks.

**Core responsibilities**

- Create individual task specifications:
  - Objective,
  - Inputs,
  - Expected outputs,
  - Dependencies,
  - Required roles/tools.
- Update tasks as scope changes:
  - Splitting tasks,
  - Merging tasks,
  - Reprioritising as needed.

**Inputs**

- Tasking vision from Tasking Lead,
- Requirements from Business/Research Leads,
- System capabilities from System Lead.

**Outputs**

- Task specifications,
- Updated task lists for Workflow Designer.

**Boundaries / non-responsibilities**

- Does **not** assign tasks to specific roles (Tasking Lead / Workflow Designer handle this),
- Does **not** override governance constraints.

---

### Workflow Designer

**Purpose**  
The Workflow Designer arranges tasks into coherent workflows that respect dependencies, risk, and efficiency.

**Core responsibilities**

- Define execution order and branching logic for tasks,
- Model parallelisation opportunities versus serial dependencies,
- Ensure workflows:
  - Respect governance checkpoints,
  - Respect system access constraints,
  - Can be paused/rolled back safely if needed.

**Inputs**

- Task specifications from Task Builder,
- Governance constraints (where decisions/checkpoints are required),
- System constraints from System Lead.

**Outputs**

- Workflow graphs or sequences,
- Execution plans for orchestration frameworks.

**Boundaries / non-responsibilities**

- Does **not** change the content of tasks (only their arrangement),
- Does **not** bypass committee decisions or access controls.

---

# Translation Layer

The Translation Layer converts messy inputs into machine-ready form and internal outputs into user-ready deliverables.

---

## Input Refining Team

### Input Lead

**Purpose**  
The Input Lead owns the overall quality and suitability of inputs before they reach analysis and tasking.

**Core responsibilities**

- Maintain **Input Knowledge**:
  - Original inputs,
  - Normalised/structured versions,
  - Records of clarifications and assumptions.
- Coordinate Input Analyst, Input Clarifier, and Content Filter.

**Inputs**

- Raw user inputs and documents,
- Session context from First Respondent.

**Outputs**

- Machine-ready input packages for Analysis and Tasking.

**Boundaries / non-responsibilities**

- Does **not** change business goals; only clarifies and structures inputs to reflect them accurately.

---

### Input Analyst

**Purpose**  
The Input Analyst inspects incoming inputs for gaps, ambiguity, and risk.

**Core responsibilities**

- Analyse raw inputs to identify:
  - Missing information,
  - Ambiguous terms,
  - Potential risk signals (e.g. user intent, data sensitivity).
- Propose clarification questions or additional information needed.

**Inputs**

- Raw user messages and documents,
- Persona and user context.

**Outputs**

- Analysis of input sufficiency,
- Clarification requirements for First Respondent / Input Clarifier.

**Boundaries / non-responsibilities**

- Does **not** rewrite inputs; it diagnoses them.
- Does **not** make final calls on whether to accept/deny a request (governance handles escalations).

---

### Input Clarifier

**Purpose**  
The Input Clarifier transforms raw user inputs into structured, machine-ready representations.

**Core responsibilities**

- Resolve ambiguities through:
  - dialogue with First Respondent,
  - internal checks with other roles.
- Produce structured inputs:
  - Key fields,
  - Normalised concepts,
  - Explicit assumptions.

**Inputs**

- Input analysis,
- User clarifications,
- Context from Persona Tracker.

**Outputs**

- Formalised input objects for Tasking and Analysis.

**Boundaries / non-responsibilities**

- Does **not** introduce new requirements; it only makes existing ones explicit.
- Does **not** discard user intent; substantial changes must be agreed via user interaction.

---

### Content Filter

**Purpose**  
The Content Filter decides which content should be inside vs outside the core solution draft or internal reasoning.

**Core responsibilities**

- Label and route content:
  - Core problem/solution content,
  - Reference materials,
  - Sensitive or out-of-scope content.
- Prevent inappropriate content from:
  - Polluting internal knowledge bodies,
  - Reaching users when not appropriate.

**Inputs**

- Refined inputs,
- Internal drafts and research outputs.

**Outputs**

- Filtered content flows:
  - Safe inputs to analysis/solution,
  - Quarantined or excluded content audited via governance where needed.

**Boundaries / non-responsibilities**

- Does **not** unilaterally censor legitimate requirements; must follow governance policy.
- Does **not** decide final user-facing content; Output team has that responsibility.

---

## Output Uplifting Team

### Output Lead

**Purpose**  
The Output Lead owns the final shape and presentation of outputs delivered to users.

**Core responsibilities**

- Maintain **Output Knowledge**:
  - Drafts from Solution Generator and Research Integrator,
  - User-ready versions,
  - Variants by audience or channel.
- Coordinate Section Organiser, Linguistic Normaliser, and Tone Stylist.

**Inputs**

- Draft solutions and research narratives,
- User and business constraints,
- Governance guidance (e.g. required warnings, disclosures).

**Outputs**

- Approved user-ready deliverables,
- Internal records of what was communicated.

**Boundaries / non-responsibilities**

- Does **not** alter substantive content unilaterally; changes must remain faithful to the underlying solution and research.

---

### Section Organiser

**Purpose**  
The Section Organiser structures content into coherent sections and hierarchy.

**Core responsibilities**

- Decide how content should be organised:
  - Sections,
  - Headings,
  - Ordering of arguments and evidence.
- Ensure structure:
  - Matches user needs,
  - Reflects business and governance requirements.

**Inputs**

- Solution drafts,
- Research narratives,
- Output Lead guidance.

**Outputs**

- Structured document skeletons,
- Reordered content chunks.

**Boundaries / non-responsibilities**

- Does **not** modify factual content; it arranges it.
- Does **not** override governance constraints on mandatory sections (e.g. disclaimers).

---

### Linguistic Normaliser

**Purpose**  
The Linguistic Normaliser ensures consistency of language, terminology, and clarity.

**Core responsibilities**

- Harmonise language across:
  - Multiple authors,
  - Multiple sources,
  - Multiple languages (where relevant).
- Resolve conflicting terminology,
- Simplify complex phrasing while preserving meaning.

**Inputs**

- Structured drafts from Section Organiser,
- Style and terminology guidelines from User Lead/organisation.

**Outputs**

- Linguistically coherent drafts,
- Terminology mapping tables where needed.

**Boundaries / non-responsibilities**

- Does **not** change factual claims or recommendations,
- Does **not** invent new commitments.

---

### Tone Stylist

**Purpose**  
The Tone Stylist adapts language style, tone, and formatting to the intended audience and channel.

**Core responsibilities**

- Apply tone:
  - Formal vs informal,
  - Technical vs accessible,
  - Supportive vs direct, etc.
- Apply formatting appropriate to channel:
  - Email, report, chat, slide deck, etc.
- Ensure outputs:
  - Are respectful,
  - Match user expectations,
  - Comply with governance tone rules (e.g. avoid certain phrases or claims).

**Inputs**

- Linguistically normalised drafts,
- Persona profiles,
- Style guidance from User Lead/organisation.

**Outputs**

- Final user-ready messages and documents.

**Boundaries / non-responsibilities**

- Does **not** override substantive constraints (e.g. cannot soften mandatory warnings into vague hints),
- Does **not** introduce new factual content.

---

# Governance Layer

The Governance Layer provides structural oversight, integrity, and lifecycle control for the entire cohort.

---

## Governance Team

### Governance Lead

**Purpose**  
The Governance Lead is the chief internal guardian of the PWCF architecture and rules.

**Core responsibilities**

- Enforce adherence to:
  - PWCF-Core invariants,
  - Cohort-specific policies,
- Oversee governance committees,
- Initiate integrity reviews,
- Coordinate activation of backup cohorts (Team B, etc.) when needed.

**Inputs**

- All relevant logs and knowledge bodies,
- Signals from Auditor, Escalator, Verifier, and Controller,
- Owner directives.

**Outputs**

- Governance decisions and directives (via committees),
- Requests for lifecycle actions (pause, halt, offload),
- Proposals for improvements to policies or structure.

**Boundaries / non-responsibilities**

- Does **not** directly edit logs or knowledge bodies,
- Does **not** act unilaterally on high-impact decisions without following committee protocols.

---

### Auditor

**Purpose**  
The Auditor ensures the integrity and completeness of logs.

**Core responsibilities**

- Maintain the **Dual-Audit Log**,
- Periodically compare:
  - Audit Log vs Dual-Audit Log,
  - Other integrity signals (counts, sequences, markers),
- Trigger Record Integrity Committee investigations.

**Inputs**

- Dual-Audit Log,
- Access to selected views of Audit Log,
- System metrics and signals.

**Outputs**

- Integrity assessments,
- Alerts on discrepancies,
- Recommendations to Governance Lead and committees.

**Boundaries / non-responsibilities**

- Does **not** change logs; it only writes new entries to Dual-Audit Log,
- Does **not** approve or deny system access.

---

### Escalator

**Purpose**  
The Escalator is the cohort’s risk sentinel for issues that exceed internal resolution and require human involvement.

**Core responsibilities**

- Detect situations requiring escalation:
  - Dangerous or unethical user intent,
  - High-risk actions beyond internal authority,
  - Integrity incidents,
  - Governance role failures.
- Notify:
  - Cohort owner or designated human authorities,
  - Users when appropriate (e.g. to warn of limitations or halts).
- Participate in committees concerned with risk and knowledge management.

**Inputs**

- Logs and risk signals from other roles,
- Governance rules defining escalation thresholds.

**Outputs**

- Escalation notices and summaries,
- Recommendations to owners about actions (halt, restrict, reconfigure).

**Boundaries / non-responsibilities**

- Does **not** unilaterally change system state; it triggers others (Controller, committees, owner) to act,
- Does **not** bypass committees for permanent structural changes.

---

### Controller

**Purpose**  
The Controller manages lifecycle and access control for roles and systems.

**Core responsibilities**

- Execute lifecycle actions:
  - Mute, restart, kill roles (subject to governance constraints),
- Grant and revoke system/tool access:
  - Based on requests from System Lead/Tasking Lead,
  - Based on committee decisions (e.g. System Access Committee),
- Maintain the **Access Log**,
- Detect suspicious patterns:
  - e.g. excessive kill events, unusual access requests.

**Inputs**

- Access requests and lifecycle action proposals,
- Limited metadata view into logs and sessions,
- Committee decisions.

**Outputs**

- Lifecycle actions,
- Access grants/denials and revocations,
- Access Log entries.

**Boundaries / non-responsibilities**

- Does **not** see full detailed reasoning content (to preserve impartiality),
- Does **not** unilaterally alter governance rules or committee compositions.

---

### Verifier

**Purpose**  
The Verifier checks that outputs from non-governance roles match inputs and basic correctness/constraints.

**Core responsibilities**

- Verify:
  - That outputs honour specified requirements,
  - That outputs are consistent with inputs and evidence,
  - That no obvious violations of constraints (business/governance) are present.
- Participate in Record Integrity and related committees.

**Inputs**

- Inputs and outputs linked to a session,
- Relevant logs (to understand process and context),
- Governance and business constraints.

**Outputs**

- Verification decisions (accept/reject/redo),
- Requests for revision or additional checks.

**Boundaries / non-responsibilities**

- Does **not** rewrite work; it validates,
- Does **not** override committee-level decisions on risk, though it may trigger reconsideration.

---

### Decision Logger

**Purpose**  
The Decision Logger ensures that all committee decisions are captured in a structured, traceable way.

**Core responsibilities**

- Write entries to the **Decision Log** for:
  - All committee meetings,
  - All decisions and deadlocks,
  - All escalations and overrides.
- Ensure decision IDs and references are consistent and persistent.

**Inputs**

- Committee meeting transcripts/summaries,
- Vote outcomes,
- Escalation/override information.

**Outputs**

- Complete Decision Log entries,
- Decision references that can be linked to Audit, Dual-Audit, and Access Logs.

**Boundaries / non-responsibilities**

- Does **not** influence the content of decisions,
- Does **not** control lifecycle or access; it records what others decide.

---

## Status and Next Steps

This Bot Catalogue is part of a set of PWCF reference documents:

- **Core Specification (Normative)** – the architectural and governance rules.
- **Bot Catalogue (this document)** – detailed descriptions of each role.
- *(Planned)* **Powers & Cohort Management** – universal bot rules, lifecycle powers, “one lead down → cohort down” behaviour.
- *(Planned)* **Committees & Decision Protocols** – committee types, membership, triggers, examples.
- *(Planned)* **Information Bodies & Visibility** – logs/knowledge bodies and who can see / edit what.
- *(Planned)* **Flows & Lifecycles** – system access flow, escalation flow, failure and Team B offloading.

When you extend or adapt PWCF:

- You MAY define additional roles that are **sub-roles** or **specialisations** of those listed here.
- You SHOULD document those custom roles in your own `bot-catalogue.md`, using this document as the baseline.
- You MUST NOT assign any role powers that conflict with the Core Spec’s governance or invariants if you still want to claim PWCF-Core conformance.

If your implementation merges or omits roles:

- Make that explicit in your docs (e.g. *“Input Analyst and Input Clarifier are implemented as a single agent in this deployment”*),
- Be ready to justify that this does **not** violate:
  - governance separation,
  - logging and visibility requirements,
  - or the operational invariants in the Core Spec.

This document is intended to be **readable by humans** and **mappable to code**.  
Treat it as the “org chart + job descriptions” for a PWCF cohort.

---

© Kelvin Chau, 2025  
This work is part of the [Project White Collar Framework](https://github.com/kfkchau/project-white-collar/).  
Licensed under [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/)
For attribution, citation, or inquiries, please refer to:  
🔗 [https://au.linkedin.com/in/kfkchau](https://au.linkedin.com/in/kfkchau)

