# LLM-Defined Persistent Memory

## A Model-Native Memory Adapter Using Semantic Signal Alphabet + RD Engine

## 1. Summary

LLM-Defined Persistent Memory is a compact, reducer-governed memory architecture that combines two existing project ideas:

* **Semantic Signal Alphabet (SSA)** — the LLM defines its own semantic buckets for a domain.
* **RD Engine** — the host/runtime stores durable state and updates it through deterministic reducers.

Together, they create a memory system where a model can leave compact notes for its future self in a semantic structure it helped define.

The core idea:

> The model defines the drawers.
> The host controls the notebook.
> Reducers decide what may be written.
> Future sessions read compact, model-native memory instead of skimming miles of transcript.

This is not infinite memory. It is **bounded, compact, persistent, model-native continuity**.

It reduces reliance on raw transcripts, long summaries, vector retrieval, and repeated re-decoding of old context. It does not remove hardware limits, storage limits, context limits, model drift, stale memory, or the need for audit trails.

---

## 2. The Central Insight

Most LLM memory systems are built around one of these patterns:

1. Save long transcripts.
2. Summarize conversations.
3. Retrieve chunks through embeddings.
4. Store human-defined facts in a database.
5. Fine-tune the model.

Each approach can work, but each has a recurring problem: the model often has to re-interpret memory that was not organized according to its own semantic habits.

The synthesis here is different.

Semantic Signal Alphabet lets the model define a fixed bucket map for a domain. RD Engine then stores durable values inside those buckets. The LLM reads those values at the start of future sessions and may propose reducer actions to update them.

This creates a compact memory state that is closer to the model’s own handwriting.

The useful phrase is:

> LLM-defined buckets become memory slots.

Or, more fully:

> SSA makes memory model-native.
> RD Engine makes memory durable and governed.
> Reducers make updates auditable and controlled.

---

## 3. Why This Exists

Long-term assistant memory has several hard problems:

* Context windows are finite.
* Summaries drift.
* Transcript logs become too large to skim.
* Vector retrieval can return semantically adjacent but wrong material.
* Human-made categories may not match the model’s own internal semantic tendencies.
* Cross-session continuity often depends on bulky external context.
* Models may overwrite, hallucinate, or reinterpret memory if not governed by runtime rules.

This architecture addresses a narrow but important slice of that problem:

> How can a model carry compact, persistent, semantically meaningful state across sessions without dragging the full past conversation with it?

The answer proposed here:

1. Let the target model define a small set of semantic buckets.
2. Freeze those buckets as a versioned artifact.
3. Store current memory values inside those buckets.
4. Let the model propose updates.
5. Let deterministic reducers decide what actually changes.
6. Preserve an evidence/event log separately for audit, repair, and migration.

---

## 4. SSA and RD Engine as Inverse Halves

### 4.1 Semantic Signal Alphabet

Semantic Signal Alphabet is model-native.

The model is given:

* a domain,
* a source vocabulary or source corpus,
* a bucket count,
* a generation prompt,
* and the instruction to partition meaning into a fixed number of buckets plus UNASSIGNED.

The model produces:

* bucket IDs,
* labels,
* descriptions,
* anchor terms,
* assigned concepts,
* unassigned overflow,
* metadata about the model and generation process.

In SSA, the model says:

> “This is how I would carve the domain.”

### 4.2 RD Engine

RD Engine is host-native.

The host/runtime defines:

* what state exists,
* what actions are valid,
* what reducers may update state,
* what preconditions must be satisfied,
* how state is persisted,
* how snapshots are loaded,
* how updates are logged.

In RD Engine, the host says:

> “This is what exists, and this is how it may change.”

### 4.3 Combined System

The combined system becomes:

> Model-native semantic buckets + host-enforced state transitions.

The LLM is allowed to define the memory shape, but not freely mutate memory by vibes.

The runtime remains the source of truth.

---

## 5. Core Design Principle

The model does not directly own memory.

It proposes memory actions.

The host executes only valid reducer actions.

This prevents the LLM from freely rewriting persistent memory, while still allowing it to participate in its own continuity.

A typical loop:

1. Runtime loads bucket map.
2. Runtime loads current bucket state.
3. LLM receives compact memory context.
4. User interacts.
5. LLM proposes structured memory updates.
6. Runtime validates actions.
7. Reducers apply permitted changes.
8. Runtime saves new state.
9. Event log records what happened.

---

## 6. Important Non-Claims

This system does not claim:

* infinite memory,
* perfect recall,
* universal memory portability,
* consciousness,
* hidden-model introspection,
* transcript replacement for all use cases,
* correct memory without audit,
* cross-model compatibility without migration,
* no need for external databases,
* no need for evidence logs.

The correct claim is:

> This system provides bounded, compact, persistent, model-native memory using frozen semantic buckets and deterministic reducer-governed updates.

---

## 7. Why “Specific Model Limited” Is a Feature, Not a Bug

Each model has its own semantic habits.

A bucket map generated by one model should not be assumed to fit another model.

A bucket map should be tied to:

* model name,
* model file,
* model hash,
* tokenizer identity where possible,
* generation prompt version,
* source vocabulary/corpus,
* bucket count,
* creation timestamp,
* schema version.

The rule is:

> New model, new bucket map.

A softer version:

> New model, validate or regenerate the bucket map before trusting it.

This is intentional.

A universal memory bucket map may sound convenient, but it risks forcing every model through the same external filing cabinet. That creates semantic monoculture.

The healthier approach is:

> Portability should be explicit translation, not fake sameness.

When changing models, the old memory should be migrated through a controlled translation process:

1. Load old bucket map.
2. Load old memory state.
3. Generate or load new model’s bucket map.
4. Ask the new model to propose migrated values.
5. Validate the migration.
6. Preserve old state for audit.
7. Freeze the new state.

The goal is not “all models use the same drawers.”

The goal is:

> Each model gets its own drawers, and migration becomes a deliberate refiling process.

---

## 8. Architecture Overview

```text
semantic-signal-alphabet/
  bucket map generation
  model-native semantic drawers

rd-engine/
  reducer-governed durable state
  deterministic update rules

memory adapter/
  loads bucket map
  loads state
  builds compact memory prompt
  parses proposed reducer actions
  applies reducers
  saves state
  logs evidence
```

The adapter can live as:

```text
semantic-signal-alphabet/
  adapters/
    memory_rd_engine/
```

Or as a ChattyCog/ChattyFactory-compatible module:

```text
chattycog_gui/
  modules/
    memory_rd_engine/
```

The exact location can vary. The important thing is the architecture.

---

## 9. Component A: Bucket Map

The bucket map is generated offline or during setup.

### Input

* Target model.
* Source vocabulary or domain corpus.
* Desired bucket count.
* Generation prompt.
* Optional domain constraints.
* UNASSIGNED requirement.

### Output

A frozen JSON artifact.

Example:

```json
{
  "schema_version": "0.1",
  "model_id": "qwen2.5-7b-instruct-q4_k_m",
  "model_hash": "example_hash_here",
  "bucket_count": 8,
  "generation_prompt_version": "memory-buckets-v0.1",
  "source_manifest": {
    "name": "personal-assistant-memory-vocab",
    "description": "Vocabulary and concepts for assistant-style long-term user memory."
  },
  "buckets": [
    {
      "id": 0,
      "label": "user_preferences",
      "description": "Likes, dislikes, preferred tools, style, food, media, and working preferences.",
      "anchors": ["likes", "prefers", "favorite", "avoid", "style"]
    },
    {
      "id": 1,
      "label": "user_goals",
      "description": "Active projects, intentions, tasks, milestones, and desired outcomes.",
      "anchors": ["goal", "project", "build", "plan", "finish"]
    },
    {
      "id": 2,
      "label": "user_state",
      "description": "Current or recent state such as tiredness, frustration, excitement, stress, or momentum.",
      "anchors": ["tired", "stressed", "excited", "sick", "focused"]
    },
    {
      "id": 3,
      "label": "stable_facts",
      "description": "Stable facts about the user, household, devices, repos, tools, and long-term context.",
      "anchors": ["name", "device", "location", "family", "repo"]
    },
    {
      "id": 4,
      "label": "conversation_continuity",
      "description": "Important discussion threads, recent decisions, and context needed to continue prior work.",
      "anchors": ["discussed", "decided", "next", "continuation", "thread"]
    },
    {
      "id": 5,
      "label": "pending_actions",
      "description": "Tasks, reminders, follow-ups, unresolved work, and next steps.",
      "anchors": ["todo", "follow-up", "later", "next", "pending"]
    },
    {
      "id": 6,
      "label": "temporal_context",
      "description": "Dates, schedules, deadlines, anniversaries, time windows, and sequencing.",
      "anchors": ["date", "deadline", "tomorrow", "week", "anniversary"]
    },
    {
      "id": 7,
      "label": "relationship_context",
      "description": "People, roles, family, collaborators, and interpersonal context relevant to future sessions.",
      "anchors": ["wife", "daughter", "friend", "follower", "collaborator"]
    }
  ],
  "unassigned": {
    "id": -1,
    "label": "UNASSIGNED",
    "description": "Information that does not fit the current bucket map. This is overflow, not garbage."
  }
}
```

---

## 10. Component B: Memory State

The memory state is the current durable content of the bucket system.

It should be compact enough to inject into the prompt.

Each bucket value may be a plain string, but a structured record is safer.

Recommended bucket state format:

```json
{
  "bucket_id": 1,
  "bucket_label": "user_goals",
  "summary": "User is consolidating public project presentation before amplifying around the GitHub one-year anniversary.",
  "confidence": 0.91,
  "last_updated": "2026-06-13",
  "source": "user_explicit",
  "status": "active",
  "evidence_refs": ["event_2026_06_13_001"]
}
```

Recommended fields:

* `bucket_id`
* `bucket_label`
* `summary`
* `confidence`
* `last_updated`
* `source`
* `status`
* `evidence_refs`

Possible status values:

* `active`
* `stale`
* `uncertain`
* `needs_confirmation`
* `archived`

Possible source values:

* `user_explicit`
* `assistant_inferred`
* `system_observed`
* `imported`
* `migration`
* `manual_edit`

---

## 11. Component C: Event Log

The bucket state is compact.

The event log is the audit trail.

The event log should be append-only.

Example:

```json
{
  "event_id": "event_2026_06_13_001",
  "timestamp": "2026-06-13T10:22:00Z",
  "source": "user_explicit",
  "raw_text": "This week everything goes on hold and those theory repos and the start here file need to get done.",
  "proposed_action": {
    "reducer": "remember",
    "bucket_id": 5,
    "value": "Immediate priority: finish landing repo Start Here and theory repo consolidation before returning to ChattyFactory."
  },
  "applied": true,
  "applied_by": "memory_reducer_v0.1"
}
```

The event log is not injected every session.

It exists for:

* audit,
* debugging,
* conflict resolution,
* regeneration,
* migration,
* stale memory repair,
* user review.

This avoids spooky black-box memory.

---

## 12. Component D: Reducers

Reducers are the only way to modify memory state.

A reducer is a pure update function:

```text
current_state + action → new_state
```

The LLM may propose reducer actions, but the runtime decides whether they are valid.

### Basic Reducers

#### remember

Stores or replaces a bucket summary.

```json
{
  "reducer": "remember",
  "bucket_id": 1,
  "value": "User is preparing for a GitHub one-year anniversary amplification pivot."
}
```

#### append_evidence

Adds evidence to an existing bucket without rewriting the summary.

```json
{
  "reducer": "append_evidence",
  "bucket_id": 1,
  "evidence_ref": "event_2026_06_13_002"
}
```

#### merge_summary

Merges a new observation with an existing bucket summary.

```json
{
  "reducer": "merge_summary",
  "bucket_id": 1,
  "new_value": "User wants quiet build mode completed, tidy-up mostly done, and amplify next."
}
```

#### mark_stale

Marks memory as likely outdated.

```json
{
  "reducer": "mark_stale",
  "bucket_id": 6,
  "reason": "Date-specific plan may have passed."
}
```

#### request_confirmation

Marks a proposed memory as requiring user confirmation before being stored as stable.

```json
{
  "reducer": "request_confirmation",
  "bucket_id": 3,
  "value": "User may prefer 24-hour time.",
  "reason": "Preference inferred but not explicitly stated."
}
```

#### clear_bucket

Clears a bucket or moves the old value to archive.

```json
{
  "reducer": "clear_bucket",
  "bucket_id": 2,
  "reason": "Session mood no longer relevant."
}
```

#### promote_from_unassigned

Moves an item from UNASSIGNED into a fitting bucket after review.

```json
{
  "reducer": "promote_from_unassigned",
  "unassigned_event_id": "event_2026_06_13_010",
  "target_bucket_id": 4
}
```

---

## 13. Memory Strength Rules

Not all memory should be treated equally.

The adapter should distinguish between stable facts, active plans, temporary mood, and weak inference.

### Strong Memory

Can be stored with high confidence when explicitly stated.

Example:

```text
“My dog is Maddie.”
```

Possible bucket:

```text
stable_facts
```

### Medium Memory

Useful but may change.

Example:

```text
“This week I need to finish the Start Here file.”
```

Possible bucket:

```text
pending_actions
```

### Weak Memory

Should be treated as session state or flagged for confirmation.

Example:

```text
“The user seems frustrated.”
```

Possible bucket:

```text
user_state
```

This should not become a permanent personality fact unless repeated or confirmed.

### Rule

> Personal facts require stronger evidence than momentary observations.

---

## 14. UNASSIGNED Is a Feature

UNASSIGNED is not a failure bucket.

It is an overflow tray.

If information does not fit the current bucket map, it should go to UNASSIGNED with metadata.

Example:

```json
{
  "bucket_id": -1,
  "summary": "User described a dream-capture concept involving subconscious fragment accumulation.",
  "reason": "No current bucket cleanly captures dream-fragment telemetry.",
  "timestamp": "2026-06-13T11:00:00Z",
  "status": "review_later"
}
```

UNASSIGNED should be reviewed periodically.

If many UNASSIGNED entries cluster around the same kind of information, that may indicate:

* bucket count is too low,
* source domain is too broad,
* bucket map is badly shaped,
* memory needs hierarchy,
* a new adapter is needed,
* model changed,
* user needs changed.

UNASSIGNED should not be silently discarded.

---

## 15. Session Workflow

### First Run

1. Select target model.
2. Generate or load bucket map.
3. Initialize memory state.
4. Initialize event log.
5. Save all artifacts.

### Each Session

1. Load bucket map.
2. Load current memory state.
3. Format compact memory prompt.
4. Inject memory into system/developer/context section.
5. Process user input.
6. LLM replies normally.
7. LLM may propose memory update actions.
8. Runtime validates reducer actions.
9. Reducers apply approved updates.
10. Event log records all actions.
11. State persists to disk.

### Prompt Shape

Example compact memory injection:

```text
MODEL-NATIVE MEMORY BUCKETS

Bucket definitions:
[0] user_preferences — Likes, dislikes, preferred tools, style, food, media, and working preferences.
[1] user_goals — Active projects, intentions, tasks, milestones, and desired outcomes.
[2] user_state — Current or recent state such as tiredness, frustration, excitement, stress, or momentum.
[3] stable_facts — Stable facts about the user, household, devices, repos, tools, and long-term context.
[4] conversation_continuity — Important discussion threads, recent decisions, and context needed to continue prior work.
[5] pending_actions — Tasks, reminders, follow-ups, unresolved work, and next steps.
[6] temporal_context — Dates, schedules, deadlines, anniversaries, time windows, and sequencing.
[7] relationship_context — People, roles, family, collaborators, and interpersonal context relevant to future sessions.

Current values:
[0] user_preferences:
User prefers direct critique, practical tools, docs-first scaffolding, local-first systems, and friendly but bounded AI tooling.

[1] user_goals:
User is preparing the Chatty ecosystem for a GitHub one-year anniversary amplification pivot.

[5] pending_actions:
Immediate priority: finish landing repo Start Here, consolidate theory repos, add screenshots, then return to ChattyFactory.
```

The model does not need the full transcript to regain continuity. It gets a compact state snapshot.

---

## 16. Pseudocode

```python
from memory_store import MemoryStore
from reducers import apply_reducer
from llm import generate

bucket_map = load_json("memory_buckets.json")
store = MemoryStore("memory_state.json", "event_log.jsonl")

def build_memory_prompt(bucket_map, state):
    lines = []
    lines.append("MODEL-NATIVE MEMORY BUCKETS\n")

    lines.append("Bucket definitions:")
    for bucket in bucket_map["buckets"]:
        lines.append(
            f'[{bucket["id"]}] {bucket["label"]} — {bucket["description"]}'
        )

    lines.append("\nCurrent values:")
    for item in state["buckets"]:
        if item.get("summary"):
            lines.append(
                f'[{item["bucket_id"]}] {item["bucket_label"]}: '
                f'{item["summary"]}'
            )

    return "\n".join(lines)

while True:
    user_input = input("User: ")

    state = store.load_state()
    memory_prompt = build_memory_prompt(bucket_map, state)

    prompt = f"""
{memory_prompt}

User message:
{user_input}

Reply to the user normally.

If memory should be updated, also emit proposed reducer actions as JSON in a separate MEMORY_ACTIONS block.
Only propose updates that are useful for future continuity.
"""

    response = generate(prompt)

    reply = parse_reply(response)
    actions = parse_memory_actions(response)

    print(reply)

    for action in actions:
        result = apply_reducer(state, action)
        store.append_event(user_input, action, result)

    store.save_state(state)
```

---

## 17. Example Interaction

### Session 1

User:

```text
I love hiking, and I am planning a Yosemite trip in August.
```

LLM reply:

```text
That sounds like a great trip to plan around. Yosemite in August will probably need some careful timing, bookings, and heat-aware planning.
```

Proposed memory actions:

```json
[
  {
    "reducer": "remember",
    "bucket_id": 0,
    "value": "User loves hiking.",
    "source": "user_explicit"
  },
  {
    "reducer": "remember",
    "bucket_id": 1,
    "value": "User is planning a Yosemite trip in August.",
    "source": "user_explicit"
  }
]
```

Runtime applies reducers and persists state.

### Session 2

User:

```text
What did I say about Yosemite?
```

Injected memory includes:

```text
[1] user_goals:
User is planning a Yosemite trip in August.
```

LLM can answer:

```text
You said you were planning a Yosemite trip in August, and that you love hiking.
```

No transcript search required.

---

## 18. Comparison With Other Memory Approaches

| Approach               | Strength                   | Weakness                                     | This synthesis                                   |
| ---------------------- | -------------------------- | -------------------------------------------- | ------------------------------------------------ |
| Transcript storage     | Complete raw history       | Huge, hard to skim, context-heavy            | Keeps compact state, stores event log separately |
| Summarization          | Simple and cheap           | Drift, flattening, repeated re-summarization | Stable bucket values with evidence refs          |
| Vector retrieval       | Finds related chunks       | Can retrieve adjacent but wrong context      | No search needed for core continuity             |
| Human-defined database | Precise when well-designed | May not match model semantics                | Buckets are model-defined                        |
| Fine-tuning            | Can alter model behavior   | Expensive, static, hard to update            | Dynamic runtime memory, no training required     |
| Universal schema       | Portable                   | Risks semantic monoculture                   | Model-specific schemas with explicit migration   |

---

## 19. Migration Between Models

Model-specific memory is intentional.

When changing models, do not assume the old bucket map still fits.

Recommended migration workflow:

1. Load old bucket map.
2. Load old memory state.
3. Generate new bucket map using the new model.
4. Present old bucket values to the new model.
5. Ask it to propose where each memory belongs in its new bucket map.
6. Store migration proposals.
7. Validate with rules and optional user review.
8. Save new memory state.
9. Preserve old memory state and migration log.

Migration action example:

```json
{
  "migration_id": "migration_001",
  "from_model": "llama3-8b-q4",
  "to_model": "qwen2.5-7b-q4",
  "old_bucket_id": 1,
  "old_bucket_label": "user_goals",
  "new_bucket_id": 5,
  "new_bucket_label": "active_projects",
  "value": "User is preparing the Chatty ecosystem for public amplification.",
  "confidence": 0.88,
  "needs_user_review": false
}
```

Principle:

> New mind, new drawers.
> Bring the old notes, but let the new mind re-sort them.

---

## 20. Why Cross-Model Compatibility Can Be Dangerous

A system that becomes perfectly cross-model compatible may be convenient, but it may also indicate that the bucket system has become too generic.

If every model uses the same buckets without translation, then either:

1. The buckets are so vague they no longer carry much signal.
2. The models have been forced into an external semantic monoculture.
3. Model-specific differences are being ignored.
4. Migration loss is being hidden.

This system should not aim for fake sameness.

It should aim for explicit translation.

Cross-model support should mean:

> We can migrate memory between models carefully.

It should not mean:

> Every model must think in the same drawers.

---

## 21. Relationship to Dream Forge and Fragment Capture

The same architecture can support low-grade fragment capture systems.

For example, Dream Forge could capture:

* waking fragments,
* dream fragments,
* daydream residue,
* mood tags,
* recurring symbolic shapes,
* active project churn topics.

The same principle applies:

> Do not assume each fragment is meaningful alone.
> Accumulate fragments until recurring shapes become visible.

In this use case, SSA could generate buckets for personal fragment classification, while RD Engine stores recurring patterns.

Example buckets:

* unresolved_problem_shape
* repeated_place
* missing_tool
* blocked_path
* social_pressure
* wrong_room
* object_fragment
* emotional_residue
* UNASSIGNED

This should not be framed as mystical dream decoding.

It is better framed as:

> low-grade cognitive residue capture for later pattern review.

---

## 22. Relationship to ChattyCog

ChattyCog can use this adapter as one layer of persistent memory.

Possible use:

```text
ChattyCog session starts
→ loads model-specific memory buckets
→ injects compact memory into context
→ user interacts
→ model proposes memory reducer actions
→ host applies approved changes
→ cold/luke-warm memory remains separate
```

This does not replace all ChattyCog memory.

It adds a compact model-native state layer that can sit beside:

* cold memory,
* luke-warm memory,
* module notes,
* project rundowns,
* file logs,
* explicit user notes.

---

## 23. Relationship to ChattyFactory

ChattyFactory can use the same architecture for build state.

Possible buckets:

* active_artifact_goal
* current_build_stage
* known_failures
* repair_attempts
* user_constraints
* accepted_design_decisions
* pending_tests
* handoff_notes
* UNASSIGNED

This would let a local model resume build/patch work without re-reading every log every time.

It would not replace compile logs or test output.

It would provide compact continuity:

> Where are we?
> What failed?
> What is the next bounded action?
> What should not be changed?

---

## 24. Relationship to Janet School

Janet School can use this architecture to track reasoning-state summaries across sessions.

Possible buckets:

* concept_mastery
* recurring_error_shape
* uncertainty_markers
* successful_interventions
* failed_interventions
* prerequisite_gaps
* reasoning_style
* telemetry_notes
* UNASSIGNED

This would allow longitudinal tracking of an MCM’s development without dragging every interaction into the active prompt.

The full telemetry remains in logs.

The bucket state provides compact experiment continuity.

---

## 25. Recommended File Layout

```text
adapters/
  memory_rd_engine/
    README.md
    docs/
      00_premise.md
      01_architecture.md
      02_bucket_maps.md
      03_memory_state.md
      04_reducers.md
      05_model_specificity.md
      06_migration.md
      07_unassigned_overflow.md
      08_examples.md
    schemas/
      bucket_map.schema.json
      memory_state.schema.json
      memory_event.schema.json
      reducer_action.schema.json
      migration_record.schema.json
    examples/
      assistant_8_bucket_map.json
      assistant_memory_state.json
      assistant_event_log.jsonl
      chattyfactory_bucket_map.json
      dreamforge_bucket_map.json
    src/
      memory_store.py
      reducers.py
      prompt_format.py
      session_loop.py
      migrate.py
    tests/
      test_reducers.py
      test_schema_validation.py
      test_prompt_format.py
```

---

## 26. Minimum Viable Implementation

The first implementation should be small.

### MVP Goals

* Load a bucket map JSON.
* Load/save memory state JSON.
* Append events to JSONL.
* Format memory prompt.
* Accept reducer actions as JSON.
* Apply reducers deterministically.
* Provide a CLI demo.

### MVP Non-Goals

* No vector database.
* No web UI.
* No automatic cloud sync.
* No fine-tuning.
* No universal cross-model memory.
* No complex dream analysis.
* No large-scale agent framework.

### MVP CLI Example

```text
python session_loop.py --bucket-map examples/assistant_8_bucket_map.json --state data/memory_state.json
```

The CLI should:

1. Print current memory.
2. Accept user input.
3. Call selected model or stub model.
4. Parse proposed memory actions.
5. Apply reducers.
6. Save state.
7. Show updated memory.

---

## 27. Safety and Governance

Memory systems can easily become creepy, wrong, stale, or overconfident.

This adapter should include guardrails.

### Required Safeguards

* User can inspect all memory.
* User can clear buckets.
* User can disable automatic updates.
* User can require confirmation before stable personal facts are stored.
* Event log preserves evidence.
* Inferences are marked as inferences.
* Temporary state is not promoted to stable fact without reason.
* Stale memory can be marked stale.
* Migrations preserve old state.
* UNASSIGNED is reviewed, not silently deleted.

### Important Rule

> The runtime owns persistence. The LLM only proposes.

---

## 28. Failure Modes

### Bucket Saturation

A bucket becomes too broad and accumulates too much mixed information.

Mitigation:

* split bucket,
* increase bucket count,
* add hierarchy,
* archive stale values,
* regenerate map.

### Semantic Drift

Bucket meaning changes informally over time.

Mitigation:

* freeze bucket definitions,
* version maps,
* validate updates against descriptions,
* require migration for major changes.

### Over-Memory

The system remembers too much.

Mitigation:

* confidence thresholds,
* explicit user control,
* aging/staleness rules,
* bounded summaries.

### Under-Memory

The system fails to remember important things.

Mitigation:

* explicit remember command,
* post-session review,
* pending review queue,
* improved reducer prompts.

### False Memory

The system stores an incorrect inference.

Mitigation:

* evidence refs,
* user review,
* lower confidence for inferred facts,
* correction reducer.

### Cross-Model Misread

A new model misinterprets old buckets.

Mitigation:

* regenerate bucket map,
* explicit migration,
* preserve old map,
* audit migrated values.

---

## 29. Public Framing

Good public description:

> LLM-Defined Persistent Memory is a compact memory adapter that lets a target model define its own semantic memory buckets, while a deterministic reducer engine stores and updates those buckets across sessions.

Avoid:

* infinite memory,
* consciousness claims,
* mind uploading language,
* universal model memory,
* perfect recall,
* “context windows no longer matter.”

Use:

* bounded,
* compact,
* persistent,
* reducer-governed,
* model-native,
* auditable,
* migration-aware.

---

## 30. Final Principle

This system is built around one practical idea:

> A model should not need to wake up and decipher a giant ape-made filing cabinet every session.

Instead:

1. Let the model define the bucket map.
2. Freeze the map.
3. Let the host store state.
4. Let reducers govern updates.
5. Let future sessions read compact notes in a familiar semantic shape.
6. Translate explicitly when models change.

The result is not infinite memory.

It is a small, durable, model-native notebook.

The model writes in its own handwriting.

The host keeps the notebook safe.
