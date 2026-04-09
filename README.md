![A mechanical visual metaphor for protocol construction, stamping, and controlled runtime formation.](assets/images/protocol-machine.png)

# 🧭 Codex Shared Best Practice (CSBP)

A three-file protocol kit for maintaining a controlled, operator-approved shared best practice layer in Codex.

## 🌍 Publication Status

This repository is the public source surface for the CSBP protocol kit.

It publishes the protocol texts themselves:

- `CSBP-entry-point.txt`
- `shared-best-practice.txt`
- `shared-best-practice-compiler.txt`

It does not publish a software package, CLI, plugin, or hosted service.

## 📘 What this is

CSBP is not a plugin, package, or CLI tool. It is not a replacement for `AGENTS.md`.

It is a portable AI-only protocol kit for Codex environments that need a durable, reviewable best-practice layer without turning that layer into a second operating contract.

It is also not a system that turns session history into active guidance automatically.

In normal use, the operator does not sit down and manually maintain these files. The operator boots the system, Codex reads the active layers, and new rules are discussed only when there is something worth promoting.

## 🧩 Why it exists

In long-lived Codex setups, recurring guidance tends to collapse into one of two bad shapes:

- everything stays trapped in chat history
- everything gets promoted into `AGENTS.md`

A third failure mode is automatic persistence: raw session carryover, operator preference, and operational guidance all end up mixed in the same layer.

CSBP gives you a middle layer: small, explicit, operator-controlled, and separate from the host contract.

## 🔄 Example workflow

### ⚠️ Without CSBP

A recurring issue shows up during work.
It gets mentioned in chat, half-remembered in later sessions, or promoted too quickly into `AGENTS.md`.
Over time, useful guidance mixes with one-off observations, and the system becomes harder to trust.

### ✅ With CSBP

The operator notices a recurring pattern and raises it with Codex.
Codex checks whether the pattern is strong enough for CSBP: is it recurring, actionable, preventive, and broader than one project or one session?

If the answer is no, it stays out.
If the answer is yes, Codex proposes a candidate block, the operator reviews it, and only an approved practice reaches the runtime file.

### 🛠️ Candidate to promotion

Example flow:

1. During work, the operator notices a recurring mistake.
2. The operator asks Codex whether it fits CSBP.
3. Codex evaluates it with the compiler rules.
4. The candidate is refined, narrowed, split, or rejected.
5. If approved, the runtime block is added to `shared-best-practice.txt`.

### 🚫 What stays out

Not every useful observation becomes a practice.

Examples that stay out:

- one-off fixes
- project-local conventions
- rough session notes
- operator preferences that are not stable enough
- explanations that do not translate into an operational rule

This is one of the main points of CSBP: it is not just a place to keep good ideas, it is also a filter that keeps weak or local material out of the runtime layer.

## 📂 Files

### 🚪 `CSBP-entry-point.txt`

Bootstrap and routing layer. Read first, every session. Defines what CSBP is, authority order, the two usage modes, and write restrictions. Fixed. Do not modify, rewrite, or extend.

### 📌 `shared-best-practice.txt`

Runtime layer. Stores promoted practices only. Each practice uses a fixed block shape:

| Field | Purpose |
|---|---|
| `id` | Sequential identifier assigned at promotion (`PNNN`) |
| `status` | `active` or `deprecated` |
| `scope` | `global` or `environment-global` |
| `kind` | `environment`, `orientation`, `operation`, or `verification` |
| `applies_when` | Operational condition for relevance |
| `goal` | Intended operational effect |
| `do` | Primary correct default action |
| `avoid` | Recurring wrong pattern |

If `status` is `deprecated`, stop reading the block.

### 🧪 `shared-best-practice-compiler.txt`

Formation layer. Loaded only for practice formation work. Governs candidate intake, admission and rejection rules, normalization, proposal shape, and the full promotion lifecycle. Fixed. Do not modify, rewrite, or extend.

![A sci-fi visual metaphor for AGENTS, authority, policy, sessions, and environment linked through a controlled operational graph.](assets/images/authority-network.png)

## 🏛️ Authority order

```text
operator instruction > AGENTS.md > local project instructions or project-local guidance > CSBP
```

CSBP never overrides higher-authority instructions. Conflicting items are not applied.

## ⚙️ How it works

1. Codex reads `CSBP-entry-point.txt`. That activates CSBP for the session.
2. Codex loads `shared-best-practice.txt`. Active practices are applied when relevant.
3. Codex reads `shared-best-practice-compiler.txt` only when formation work is required.

The two modes are intentionally separated. Compiler logic does not bleed into runtime behavior.

That separation also shapes the workflow.

If the operator has a possible rule in mind, they discuss it with Codex against the current environment and the compiler rules. Codex proposes a candidate, the rule is refined or rejected, and only approved output reaches the runtime file.

The other common entry point is session review. After a block of work, the operator can ask Codex to examine the session and look for candidate rules that fit CSBP. Codex can propose and discuss. It does not write new runtime practice on its own.

If there are no active relevant practices in `shared-best-practice.txt`, CSBP continues without runtime guidance. If no candidate practice is under discussion, the compiler layer does not load into runtime behavior.

## ♻️ Practice lifecycle

```text
candidate  -> evaluate -> normalize -> promote | reject
active     -> revise
active     -> deprecate
deprecated -> remove (on direct operator instruction only)
```

No practice enters the runtime without operator approval. The operator assigns the final `PNNN` identifier at promotion, not during proposal.

## 🎯 Promotion criteria

A candidate is admitted when it is recurring across sessions, global or environment-global in scope, actionable during real work, preventive by default, and not already covered by `AGENTS.md` or a stronger local instruction.

A candidate is rejected when it is project-specific, session-specific, one-off, vague, diagnostic-only, recovery-only, duplicate, or reads like policy or explanation instead of operational instruction.

## 📝 Proposal shape

```text
candidate:      short description of the pattern
kind:           environment | orientation | operation | verification
scope:          global | environment-global
rationale:      one short operational sentence
proposed block: full block shape draft
```

ID is left unassigned until operator promotion.

## 🧱 Install

```text
.codex/
  memories/
    CSBP-entry-point.txt
    shared-best-practice.txt
    shared-best-practice-compiler.txt
```

Wire `CSBP-entry-point.txt` into your `.codex/AGENTS.md` so a fresh chat reads it before using the system. The exact bootstrap line depends on your host environment contract.

## 🔌 Compatibility

If CSBP is your persistent best-practice system, disable Codex memories. Running both at the same time creates two competing sources of guidance.

The same caution applies to other automatic note-taking or persistence layers that turn session carryover into behavior over time without explicit review and promotion.

## 🛎️ When to change what

Change `shared-best-practice.txt` when a practice has passed through the compiler path and the operator has approved the action.

Do not change, rewrite, or extend `CSBP-entry-point.txt` or `shared-best-practice-compiler.txt` during runtime use or practice formation work. Those are the fixed system-definition layers.

<details>
<summary><strong>🔬 Technical Protocol Details</strong></summary>

### 🧱 System model

CSBP is a layered protocol system, not a single instruction file and not a memory dump. Its mechanics depend on strict separation between bootstrap, runtime application, and practice formation.

| Layer | File | Operational role | Mutable at runtime | Read timing |
|---|---|---|---|---|
| Bootstrap layer | `CSBP-entry-point.txt` | Defines system identity, authority order, load order, mode routing, and write restrictions | No | First |
| Runtime layer | `shared-best-practice.txt` | Stores already promoted practices that may guide live work when relevant | Yes, but only through the compiler path and with operator approval | After bootstrap |
| Compiler layer | `shared-best-practice-compiler.txt` | Governs formation, evaluation, normalization, promotion, revision, deprecation, removal, and rejection of practices | No during normal runtime use | Only during practice formation work |

### ⚙️ Core mechanical principle

The protocol works by keeping three different jobs separate:

| Job | Description | Why separation matters |
|---|---|---|
| Define the system | Explain what CSBP is, how it is loaded, and what authority it has | Prevents the runtime layer from redefining the system |
| Apply promoted practices | Use only already-approved runtime instructions when they are relevant | Prevents speculative or draft guidance from becoming active behavior |
| Form or revise practices | Evaluate whether a recurring pattern deserves promotion into runtime | Prevents discussion and drafting logic from leaking into normal operation |

If those jobs are mixed together, CSBP stops being a controlled best-practice layer and starts behaving like an unstable second contract.

### 🏛️ Authority and conflict handling

CSBP is explicitly lower-authority than operator instruction, `AGENTS.md`, and local project rules.

```text
operator instruction > AGENTS.md > local project instructions or project-local guidance > CSBP
```

Operationally, that means:

1. CSBP may guide behavior only when no higher-authority rule overrides it.
2. A conflicting runtime practice is not applied.
3. CSBP does not create new policy authority for itself.
4. Runtime practices cannot redefine scope, policy, or authority.

### 🧭 Load order and execution path

The protocol has a fixed read order:

1. Read `CSBP-entry-point.txt`.
2. Activate runtime mode.
3. Read `shared-best-practice.txt`.
4. Apply only active and relevant practices.
5. Read `shared-best-practice-compiler.txt` only if the current task is practice formation work.

This order matters because CSBP should never infer its own behavior from filenames alone and should never treat the compiler as always-on runtime guidance.

### 🔀 Mode separation

CSBP has two distinct operating modes.

| Mode | Primary file | Purpose | What it can do | What it must not do |
|---|---|---|---|---|
| Runtime mode | `shared-best-practice.txt` | Apply already-promoted practices during live work | Guide decisions when `applies_when` matches | Invent new practices, revise practices silently, or use compiler logic as active behavior |
| Practice formation mode | `shared-best-practice-compiler.txt` | Evaluate and shape candidate practices | Propose promotion, revision, deprecation, rejection, or removal | Auto-promote without approval or behave as if draft logic were active runtime policy |

This is one of the most important design constraints in the protocol. The compiler is intentionally powerful during formation work and intentionally inactive during ordinary runtime use.

### 🧾 Inputs and discussion transformations

Practice formation does not start from one rigid input type. The compiler layer can work from several kinds of operator-supplied material.

| Input type | How it enters the protocol |
|---|---|
| Operator observation | A direct report of a recurring issue or missed opportunity |
| Free-text description | An informal statement of a pattern or concern |
| Conversation fragment | A slice of prior discussion used as raw formation material |
| Rough hypothesis | An unconfirmed pattern that still needs evaluation |
| Reported recurring mistake or omission | A concrete repeated failure mode observed during real work |

During discussion, the protocol may also transform the candidate before any promotion decision:

| Discussion move | Purpose |
|---|---|
| Reinterpret the issue | Find the underlying recurring pattern rather than the surface wording |
| Split multiple patterns | Keep one practice focused on one recurring operational problem |
| Merge similar patterns | Avoid redundant runtime entries |
| Narrow or broaden the formulation | Adjust the candidate to the right operational scope |
| Reclassify the issue | Decide whether it is operational, policy-level, project-specific, or too weak for promotion |

### 📌 Runtime practice mechanics

The runtime file is not a notes file. It is a structured store of promoted instructions. Each block uses a fixed shape:

| Field | Meaning | Runtime function |
|---|---|---|
| `id` | Sequential identifier in `PNNN` form | Stable reference for review and maintenance |
| `status` | `active` or `deprecated` | Determines whether the block can guide live work |
| `scope` | `global` or `environment-global` | Signals how broadly the practice is meant to hold |
| `kind` | `environment`, `orientation`, `operation`, or `verification` | Aids interpretation of the practice's role |
| `applies_when` | Operational condition | Decides whether the practice is relevant now |
| `goal` | Intended effect | Explains what the practice is trying to prevent or achieve |
| `do` | Preferred default action | The main instruction to apply |
| `avoid` | Competing failure pattern | The pattern the protocol is trying to keep out |

Runtime reading order is also fixed:

1. Check `status`.
2. If `status` is `deprecated`, stop reading the block.
3. Check `applies_when`.
4. Use `goal` to interpret intent.
5. Apply `do` as the default move.
6. Use `avoid` as a guard against regression into the recurring bad pattern.

Deprecated entries are excluded from runtime use, but they remain available for review and maintenance history.

### 🗂️ Scope and kind classification

Two fields do most of the structural classification work: `scope` and `kind`.

| Field | Value | Operational meaning |
|---|---|---|
| `scope` | `global` | Stable across work contexts |
| `scope` | `environment-global` | Stable across the current working environment across sessions |
| `kind` | `environment` | Stable condition, constraint, or affordance of the environment |
| `kind` | `orientation` | Setup, framing, or initial approach |
| `kind` | `operation` | Action or working method during live work |
| `kind` | `verification` | Check, confirmation, or closure behavior |

These fields do not just label a block. They help keep the runtime layer interpretable and prevent vague practices from entering the store.

### 🧪 Practice formation pipeline

CSBP treats practice creation as a controlled pipeline, not as a casual note-taking exercise.

```text
candidate -> evaluate -> normalize -> promote | reject
active    -> revise
active    -> deprecate
deprecated -> remove
```

Each stage has a different function:

| Stage | Function |
|---|---|
| Candidate | A reported recurring issue, omission, or rough hypothesis enters discussion |
| Evaluate | The candidate is tested against recurrence, scope, usefulness, and overlap with stronger rules |
| Normalize | The wording is rewritten into a short, preventive, runtime-ready instruction |
| Promote | The candidate becomes an active runtime practice |
| Reject | The candidate is kept out of runtime because it is weak, local, duplicate, unstable, or otherwise unsuitable |
| Revise | An active practice is improved without changing the basic need it covers |
| Deprecate | An active practice is retained for reference but removed from live runtime use |
| Remove | A deprecated or invalid practice is deleted, but only on direct operator instruction |

### 🎯 Admission and rejection logic

The compiler path is designed to be selective.

| Admit when the candidate is... | Reject when the candidate is... |
|---|---|
| Recurring across sessions | Project-specific |
| Global or environment-global | Session-specific |
| Actionable during live work | One-off |
| Preventive by default | Vague |
| Expressible as a short operational instruction | Diagnostic-only |
| Stable across work contexts | Recovery-only or post-failure only |
| Not already covered by stronger rules | Duplicate of existing practice or `AGENTS.md` |
| Likely to reduce waste or recurring error | Too broad, too narrow, or dependent on hidden context |

The normalization step is what turns a useful observation into a practice. It removes narrative, removes local detail, and rewrites the idea as a stable operational default.

### 🌐 External check policy

The compiler layer may optionally suggest a lightweight web check when outside recurrence could materially clarify a decision.

| Rule | Meaning |
|---|---|
| Optional, not default | External checks are allowed but not required for every candidate |
| Corroboration, not proof | The check supports practical judgment rather than replacing it |
| Prefer primary practical sources | Sources should be close to the real environment, workflow, or tool |
| Avoid broad research | The protocol does not call for unnecessary internet research when local evidence is already enough |

### 📏 Block quality rules

The compiler does not only decide whether a practice is useful. It also enforces structural quality before promotion.

| Rule | Why it matters |
|---|---|
| One practice = one recurring pattern | Prevents overloaded blocks that try to solve multiple issues at once |
| Split if more than one main action or wrong pattern is present | Keeps runtime guidance precise and reusable |
| Rewrite or reject if too broad | Avoids vague rules that match too many situations |
| Rewrite or reject if too narrow | Avoids rules that only fit one isolated case |
| Rewrite or reject if overly explanatory | Keeps the runtime layer operational rather than essay-like |
| Re-evaluate split fragments independently | Prevents weak sub-patterns from being promoted automatically |

### 🔒 Mutability rules

Not all files in CSBP have the same status.

| File | Status | Edit rule |
|---|---|---|
| `CSBP-entry-point.txt` | Fixed system-definition layer | Do not modify during runtime use or formation work |
| `shared-best-practice-compiler.txt` | Fixed system-definition layer | Do not modify during runtime use or formation work |
| `shared-best-practice.txt` | Mutable runtime layer | Modify only through the compiler path and only with operator approval |

This is a key control mechanism. If the fixed layers become casually editable, the system can silently change its own operating semantics.

### ✅ Approval mechanics

CSBP is operator-controlled by design.

| Action | Allowed automatically | Requires operator approval |
|---|---|---|
| Apply an already-promoted relevant practice | Yes | No |
| Read runtime or bootstrap layers | Yes | No |
| Propose a candidate practice | Yes | No |
| Promote a candidate into runtime | No | Yes |
| Revise an active runtime practice | No | Yes |
| Deprecate a runtime practice | No | Yes |
| Remove a deprecated or invalid practice | No | Yes, and only on direct instruction |

The operator also assigns the final runtime `PNNN` identifier at promotion. IDs are not finalized during proposal.

### 🚫 What CSBP explicitly is not

The protocol defines several non-goals to keep scope from drifting.

| CSBP is not... | Why that boundary exists |
|---|---|
| A replacement for `AGENTS.md` | It must remain below the host contract in authority |
| A project-local instruction store | Its target is shared, recurring practice, not repo-specific rules |
| A session log | Raw conversation carryover is not runtime guidance |
| A policy or style manual | It stores operational instructions, not broad doctrine |
| A recovery cookbook | It prefers preventive defaults over after-the-fact rescue logic |
| An automatic memory system | Promotion requires explicit review and approval |

### 🛡️ Failure modes the protocol is designed to prevent

CSBP exists to guard against a specific class of operational drift:

| Failure mode | Protocol countermeasure |
|---|---|
| Useful guidance stays trapped in chat history | Promote stable recurring practices into a dedicated runtime layer |
| Everything gets pushed into `AGENTS.md` | Keep CSBP explicitly lower-authority and narrower in scope |
| Draft logic becomes live behavior | Separate runtime mode from compiler mode |
| One-off local fixes become permanent rules | Require recurrence, scope, and normalization checks |
| Fixed system rules get rewritten casually | Lock the bootstrap and compiler layers |
| Automatic persistence mutates behavior without review | Require operator approval before runtime promotion |

### 🧩 Why the three-file architecture matters

The protocol depends on the fact that each file does one main job and does not absorb the others.

- If the entry-point file starts carrying runtime practices, bootstrap and application collapse together.
- If the runtime file starts carrying compiler logic, live behavior becomes unstable and over-explanatory.
- If the compiler file starts acting like always-on guidance, formation logic bleeds into normal execution.

The three-file split is therefore not cosmetic. It is the main mechanism that makes CSBP reviewable, bounded, and safe to reuse across sessions.

</details>

## 🤖 AI Assistance

This work was developed with AI assistance under direct operator guidance and review.

The system design, framing decisions, and publication choices were shaped through human-AI collaboration rather than generated as an unreviewed autonomous output.

CSBP is published as a reviewed public artifact, with AI used as a drafting, structuring, and refinement partner during development.

## 📜 License

This repository is licensed under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).
