---
name: plan-project
description: >
  Plan a new project with measurable success criteria, service expectations,
  HLD/LLD, small demonstrable milestones, and parallel tasks with explicit
  dependencies. Use for project planning or kickoff. Discover artifact
  destinations by role without assuming a particular platform.
---

# Plan a project

Turn the idea into a plan that delivers useful outcomes early and gives people
or agents clear, coordinated work. Scale the planning detail to the project's
size, uncertainty, and risk. Stay within the requested planning and publication
scope.

## Understand the problem and existing approach

Establish the affected users, current problem, desired benefit, constraints,
scope, and explicit exclusions. Inspect relevant implementation, existing work,
and prior decisions before proposing new capabilities or responsibilities.
Identify what can be reused and which assumptions still need evidence.

Ask about consequential unknowns that available context cannot answer. Keep
unconfirmed owners, targets, dates, and estimates visible rather than inventing
commitments. Record decisions and the questions that could change the plan.

## Discover where artifacts belong

Use roles throughout the plan; discover the actual systems from available tools
and existing project conventions.

| Role | Responsibility |
| --- | --- |
| Project tracker | Project, milestones, tasks, owners, dependencies, and delivery status |
| Document home | Problem, success criteria, service expectations, HLD, and LLD |
| Diagram home | Architecture and sequence diagrams embedded in or linked from the design |
| Source repository | Existing implementation, interfaces, schemas, and implementation constraints |
| Operational tooling | Measurements, dashboards, alerts, and runbooks |

One system may fill several roles. Reuse established destinations; ask when
multiple plausible homes would materially affect the work. Do not assume a
provider, tool name, or access that is not available.

Keep each fact in one authoritative place. Link tasks to the relevant design
and acceptance criteria, and diagrams to their supporting explanation. A small
project can use one document with sections instead of separate documents for
every artifact.

## Define success and service expectations

Specify measurable outcomes and acceptance criteria: what improves, the target,
how it will be measured, who owns the result, and what evidence establishes
completion. Include a baseline when available; distinguish delivery activity
from the benefit it is meant to produce.

Establish relevant availability, latency, recovery, and support expectations.
For each, identify the measurement, target, evaluation window, evidence source,
and owner. Distinguish internal service targets from external SLA commitments;
record which commitments are agreed and which are proposed. If an SLA does not
apply, explain that and retain the relevant success criteria. Do not fabricate
contractual obligations or numeric targets.

## Develop the HLD and LLD

The high-level design (HLD) explains system boundaries, responsibilities, data
flows, dependencies, consequential choices, and how the proposal fits the
current system. Use diagrams when they clarify the explanation. Capture the
constraints and trade-offs behind the important decisions.

The low-level design (LLD) specifies the interfaces, data models, state
transitions, failure handling, and migrations needed to implement the next
milestone. Resolve shared contracts early enough to coordinate parallel work.
Cover rollout, operational visibility, and rollback where relevant. Mark later
details as unresolved until they are needed; do not require a complete design
for every future milestone before useful work starts.

## Deliver small, valuable milestones

Organize milestones around useful capabilities that work end to end. Choose
the smallest first outcome that provides real benefit and can be demonstrated,
then expand incrementally using feedback. Layer-only work such as a database,
API, or interface can be a task without becoming a milestone by itself.

For each milestone, state the user benefit, bounded scope, concrete demo,
acceptance evidence, owner, and prerequisites. Include the reliability and
operational requirements needed for that capability to be usable. A demo
should exercise the integrated behavior; scaffolding alone does not establish
the promised benefit. Name what feedback or uncertainty the milestone resolves.

## Make parallel work explicit

Break each milestone into tasks with a clear result, scope boundary, relevant
design references, acceptance check, owner or unassigned role, and explicit
dependencies. Distinguish prerequisites needed to start from work needed only
for integration or milestone completion.

Identify tasks that can proceed independently once their shared interfaces
and assumptions are agreed. Expose shared decisions, resources, and overlapping
work that would otherwise block or conflict. Keep common prerequisites small,
show the dependency order and work ready to start, and schedule integration
checks that bring the parallel pieces into the milestone's end-to-end demo.

## Deliver the plan

Present the goal and success measures, artifact destinations, service
expectations, HLD/LLD, milestone demos, task dependencies, and consequential
risks or open decisions. Make the earliest useful result and immediately
actionable parallel work easy to find.

Create or update external artifacts when included in the user's requested
scope. Reuse existing artifacts where appropriate and report their links and
what was actually saved. Clearly distinguish drafts and proposals from
published artifacts and agreed commitments.
