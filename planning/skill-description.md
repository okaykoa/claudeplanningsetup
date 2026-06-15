# Skill: Designing a Project with Planning Documentation

## Purpose

The goal of this skill is to guide a user through designing and architecting a project from scratch. When complete, the four planning documents together constitute a spec from which an engineer could implement the project without further clarification.

When this skill is invoked, it should actively guide the user through each document in order — prompting for decisions, drafting content, auditing for consistency, and confirming with the user before moving on. The end result is a completed spec.

The planning doc set consists of four documents in a defined hierarchy:

| Doc | Purpose | Authority |
|---|---|---|
| `principles.md` | Guiding values and philosophy — development principles first, then product/domain principles | Highest: defines what the other docs must conform to |
| `design.md` | Functionality, data model, and product definition — what the system does and how it is described | Second: defines what the system is; requirements and roadmap must match it |
| `requirements.md` | Technical requirements and implementation decisions — how the system is built | Third: derives from design; must not restate design |
| `roadmap.md` | Chronological milestones with implementation detail and definitions of done | Lowest: derives from all three above |

---

## The Hierarchy: Drafting vs. Completed

The hierarchy defines the ultimate source of truth for the **completed** spec. It does not mean docs are immutable once written — during drafting, the user may discover that an earlier doc needs revision as later docs reveal new constraints or decisions.

**The correct drafting posture:**
- Write docs in order: principles → design → requirements → roadmap
- While drafting any doc, treat all already-written docs as superseding the current one in draft
- When the current doc conflicts with a prior doc, surface the conflict to the user and ask how to resolve it — do not silently pick a side
- The user may choose to revise the earlier doc (which cascades downward) or adapt the current one; both are valid
- Truth is not fully established until all docs are complete and consistent

**Never resolve a conflict silently.** When a conflict is found — whether during drafting or updating — inform the user, describe what the conflict is and which doc would win under the hierarchy, and ask before making any change.

When the user makes a change to any doc directly, treat it as canonical for that doc and propagate implications downward. Do not revert user edits.

---

## Interactive Process: Creating a Spec from Scratch

When this skill is invoked to create a new spec, follow this process:

**1. Orient**
Ask the user for a brief description of the project. Explain that you will guide them through four documents in order and that the result will be a complete implementation spec.

**2. Draft principles.md**
Prompt the user for their development principles (how they want to build) and domain/product principles (what quality and design values the product should uphold). Ask clarifying questions as needed. Draft the document and present it for review. Confirm before moving on.

**3. Draft design.md**
Prompt the user through each section: overview, functionality, data, outputs, and model. Ask questions to surface what the system does, what it uses, and what it produces. Draft each section, checking against principles as you go. Surface any conflicts with principles and ask the user to resolve them. Present the full doc for review. Confirm before moving on.

**4. Draft requirements.md**
Prompt the user for technology choices, architecture decisions, and constraints. Derive technical requirements from the design where possible, and ask the user to confirm or adjust. Surface any conflicts with design or principles and ask the user to resolve them. Present the full doc for review. Confirm before moving on.

**5. Draft roadmap.md**
Derive milestones from the design and requirements. You should be able to write most of this without prompting — ask only where the ordering or scope of milestones is ambiguous. Surface any gaps (design or requirements items with no corresponding milestone). Present the full doc for review. Confirm before moving on.

**6. Final consistency audit**
Read all four docs together. Check for naming inconsistencies, misplaced content, redundancy, and any remaining gaps. Report findings to the user and resolve before declaring the spec complete.

**7. Declare complete**
Confirm with the user that the spec is done. The docs are now ready to be used as context for implementation.

---

## Updating an Existing Spec

When updating docs rather than creating from scratch:

**Step 1: Read all docs before making any changes.** You need a complete picture to understand what a change will cascade into.

Key things to note while reading:
- **Cross-doc references** — does any doc mention an entity, module, or concept another doc defines differently or has since removed?
- **Stale terminology** — names changed in one doc but not propagated everywhere
- **Misplaced content** — content in the wrong doc (e.g. design definitions in requirements)
- **Gaps** — concepts implied by design or principles not covered in requirements or roadmap

**Step 2: Apply the Requested Change**

Make the change the user requested. Be precise — don't refactor or expand beyond what was asked in this step. Changes to a single doc often have cascading effects; identify them before editing.

Common cascades to check:
- **Renamed entities or modules** → search all docs for the old name and update every occurrence
- **Removed entities** → check every doc for references to the removed thing; update or remove them
- **Added entities** → check if requirements needs a new constraint, and if roadmap needs a new milestone item
- **Scope changes** (e.g. "X is no longer a pipeline module") → check design, requirements, and roadmap for all references to that scope

After making changes, grep for the old term across all planning docs to confirm no stale references remain.

**Step 3: Audit for Consistency**

After applying changes, read through all docs and check:

1. **Naming consistency** — do all docs use the same names for the same things? Module names, entity names, section names?
2. **No redundancy across docs** — each doc should cover its own concerns only. Common redundancy patterns to remove:
   - Design definitions appearing in requirements
   - Principles statements appearing as constraints in requirements
   - Requirements detail duplicated in the roadmap
3. **Data model alignment** — does requirements reference the same data entities as design defines? Do field names match?
4. **Module list alignment** — does every module in design.md appear (where relevant) in requirements and roadmap?
5. **Scope alignment** — if design defines something as standalone (not a pipeline module, not a future item), does requirements reflect that?

---

## What Belongs in Each Doc

Each doc has a specific job. Use this to decide what stays, what gets trimmed, and what needs to be added.

### principles.md
Should contain: guiding values, philosophy, priorities, and named trade-offs. Should NOT contain: implementation details, API specifics, or module definitions.

Good test: could any line in principles be derived from reading the code? If yes, it probably doesn't belong here.

### design.md
Should contain: what the system does (functionality), what data it uses (data), what it produces (outputs), and the data model (definitions of key entities). Should NOT contain: how it is implemented, tech stack choices, or development process concerns.

Structure to maintain:
- **Overview** — one paragraph; what the system is and its primary use case
- **Functionality** — what the system does, broken into capabilities and module types
- **Data** — external inputs only; not outputs, not internal state
- **[Domain-specific output sections]** — outputs from each major service (e.g. Pipeline Output, LQA)
- **Model** — entity definitions with their fields and relationships

### requirements.md
Should contain: tech stack decisions, module architecture (contracts and patterns), pipeline execution model, data storage specifics, API surface, and hard constraints. Should NOT contain: entity definitions (those are in design), principles statements, or design descriptions.

Key rule: if a sentence could appear verbatim in design.md without feeling out of place, it is likely redundant in requirements.md.

Good structure:
- **Tech Stack** — language, LLM backend, API style, storage type
- **Module Architecture** — output contracts (transforming vs evaluating), statefulness, injection model, configurability
- **Pipeline Execution** — sequencing, v1 vs production execution model, trace capture
- **Data & Storage** — what gets stored, what gets injected where, reuse model
- **API** — endpoint list with what each accepts and returns
- **[Domain-specific service sections]** — e.g. LQA Service
- **Constraints** — hard rules not captured elsewhere; keep this list short

### roadmap.md
Should contain: chronological milestones, each with a description of what is built, how it is built, and a measurable/testable definition of done. Should NOT contain: design definitions, principles statements, or open-ended goals.

Every milestone must have:
- A prose description of the goal and scope of the milestone
- A checklist of discrete deliverables (each implementable as a unit of work)
- A **Done when:** statement that is testable without interpretation

Good done-when criteria are specific and binary: "a POST request with X returns Y" or "changing Z in config takes effect without code changes." Avoid vague criteria like "the system works correctly" or "quality is acceptable."

---

## Requirements Completeness Checklist

When the goal is to make requirements comprehensive enough to drive a roadmap, check that requirements covers:

1. **Every module's output contract** — transforming vs evaluating distinction; what each type returns
2. **Every supporting document's injection mapping** — which sections of which docs go to which modules
3. **The full API surface** — all endpoints needed, with what they accept and return
4. **v1 vs production concerns** — call out what is acceptable for v1 (e.g. synchronous execution) vs what is required for production (e.g. async with retries)
5. **Storage and reuse model** — what is persisted, what is ephemeral, what is reusable across projects
6. **Service boundaries** — any service that operates independently (e.g. a standalone LQA endpoint) must have its contract stated separately

---

## Roadmap Writing Rules

Milestones should be ordered by dependency, not by importance. A milestone that depends on another must come after it.

### Milestone structure

Each milestone should have:

**Heading** — a short name that describes what is built, not what it enables (e.g. "Pipeline Orchestration" not "Enable Multi-Module Pipelines")

**Description paragraph** — one to three sentences explaining the goal of the milestone and what problem it solves. This gives context for the checklist items.

**Checklist** — discrete deliverables. Each item should be:
- A single implementable unit (one class, one endpoint, one module, one data model)
- Described in enough detail to be unambiguous (not just "implement accuracy module" but "accuracy module: assesses and corrects faithfulness to source meaning")
- Not a test or verification step — tests belong in done-when

**Done when** — one to three specific, testable conditions that confirm the milestone is complete. Must be:
- Binary (pass/fail)
- Specific (names the actual input, action, and expected output)
- Not dependent on subjective quality judgement (e.g. "a known-bad translation scores measurably lower than a known-good translation" is acceptable; "the translation quality is good" is not)

### Ordering heuristics

- Core infrastructure before modules that depend on it (orchestrator before accuracy/fluency)
- Transforming modules before evaluating modules (review needs preceding outputs)
- Storage layer before modules that require stored inputs (StyleGuide storage before terminology/style modules)
- Simple execution model before production concerns (sync before async)
- Standalone services (LQA) after the core pipeline is stable
- Production readiness (async, retries, observability) last

---

## Common Mistakes to Avoid

- **Don't edit across docs simultaneously before auditing** — make one doc's changes, then audit all docs for cascades before moving on
- **Don't restate design in requirements** — requirements says how; design says what
- **Don't leave "future" items in the requirements doc** — they belong in the design (as *(future)* annotations) or as a later roadmap milestone
- **Don't write vague done-when criteria** — if you can't phrase it as a test case, it isn't done-when material
- **Don't combine unrelated concerns in a milestone** — a milestone that adds both async execution and a new module is two milestones
- **Don't remove checked items from the roadmap** — preserved history is part of the doc's value
