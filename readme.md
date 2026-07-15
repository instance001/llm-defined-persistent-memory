# LLM-Defined Persistent Memory

A model-native persistent memory adapter built from:

- [Semantic Signal Alphabet](https://github.com/instance001/semantic-signal-alphabet)
- [Chatty Quest / RD Engine](https://github.com/instance001/chatty-quest)

This project explores a compact memory architecture where an LLM defines its own semantic memory buckets, while a deterministic runtime stores and updates those buckets through governed reducer actions.

The examples in this repository use the surrounding Chatty ecosystem because the pattern was developed to solve real continuity and context-routing problems across many related local-first AI projects.

The goal is not "infinite memory."

The goal is:

> bounded, compact, persistent, model-native continuity.

---

## Core Idea

Most assistant memory systems rely on one or more of the following:

- long transcript logs
- summarization
- vector search
- human-defined databases
- external retrieval layers
- fine-tuning

Those can work, but they often make the model repeatedly burn context and attention re-decoding old material.

This adapter takes a different approach:

1. A target model generates a fixed semantic bucket map using the Semantic Signal Alphabet process.
2. Those buckets become persistent memory slots.
3. The RD Engine stores the current values of those slots.
4. Reducers control all memory updates.
5. The model reads a compact memory snapshot at the start of a session.
6. Entity/project triggers can inject specific "hot context" cards only when relevant.
7. Cold logs remain available as audit history, not active working memory.

In plain terms:

> The model defines the drawers.
>
> The host controls the notebook.
>
> Future sessions read notes written in a semantic shape the model already understands.

---

## Why This Exists

A large project ecosystem can produce enormous cold logs.

Cold logs are useful as archives, but they are inefficient as active memory.

Without a routing layer, a model may need to skim large text logs just to rediscover basic orientation:

- What is Chatty-Cog?
- What is ChattyFactory?
- How does Semantic Signal Alphabet relate to RD Engine?
- Which old repo is archived?
- Which project is active?
- What context is adjacent to this name?

That burns tokens on searching and deciphering instead of reasoning.

This project turns important memories into compact, structured records:

```text
User says: "Chatty-Cog"

System loads:
- Chatty-Cog routing packet
- brief current role
- active status
- related buckets
- adjacent systems worth checking
```

The same token budget becomes useful context instead of a raw search fragment.

---

## Relationship To Semantic Signal Alphabet

[Semantic Signal Alphabet](https://github.com/instance001/semantic-signal-alphabet) provides the model-native bucket layer.

A target model is given:

- a domain
- a source vocabulary or source corpus
- a bucket count
- and an instruction to divide meaning into buckets plus `UNASSIGNED`

The output is a frozen bucket map.

Example:

```json
{
  "model_id": "qwen2.5-7b-instruct-q4_k_m",
  "bucket_count": 8,
  "buckets": [
    {
      "id": 0,
      "label": "user_preferences",
      "description": "Likes, dislikes, style preferences, tools, working habits."
    },
    {
      "id": 1,
      "label": "active_projects",
      "description": "Current builds, repos, plans, milestones, and next actions."
    }
  ],
  "unassigned": {
    "id": -1,
    "label": "UNASSIGNED",
    "description": "Information that does not cleanly fit the current bucket map."
  }
}
```

The important principle:

> New model, new bucket map.

A bucket map is specific to the model that generated it. A different GGUF or cloud model may organize the same domain differently.

This is intentional.

Cross-model portability should happen through explicit migration, not fake sameness.

---

## Relationship To Chatty Quest / RD Engine

[Chatty Quest](https://github.com/instance001/chatty-quest) contains the RD Engine pattern: deterministic state, reducer-governed updates, and host-controlled truth.

This adapter uses that idea for memory.

The LLM does not directly own persistent memory.

Instead:

1. The LLM proposes memory actions.
2. The runtime validates those actions.
3. Reducers apply permitted changes.
4. State is saved.
5. Events are logged.

Example reducer action:

```json
{
  "reducer": "remember",
  "bucket_id": 1,
  "value": "User is preparing the Chatty ecosystem for a one-year GitHub anniversary amplification pivot.",
  "source": "user_explicit"
}
```

The host remains the source of truth.

The model may suggest.

The reducer decides.

---

## Memory Layers

This adapter uses several memory layers.

### 1. Standing Bucket Memory

Always-available compact memory.

Example:

```text
[active_projects]
User is consolidating public project presentation before amplifying around the GitHub one-year anniversary.

[working_style]
User prefers direct critique, docs-first scaffolding, bounded LLM roles, and local-first tooling.
```

This gives the model general orientation.

### 2. Hot Context Cards

Specific memory records injected only when relevant.

Example:

```text
User says:
"Had to take Maddie to the vet today."

Trigger:
Maddie

Injected hot context:
Maddie is the user's red heeler dog, 11 years old, storm-anxious, and important household context.
```

The model does not need to carry every detail all the time.

The host hands it the right card when a known thing is mentioned.

### 3. Context Routing Packets

Hot context cards can also act as routing packets.

A routing packet does not just say what something is.

It also says:

- current role
- current status
- related projects
- related buckets
- adjacent records
- where to look next

Example:

```text
Chatty-Cog:
Central local AI exoskeleton/module host.

Likely adjacent context:
- Semantic Signal Alphabet
- RD Engine
- ChattyFactory
- LLM Tweaker
- Janet School
```

This turns memory into context routing, not just recall.

### 4. Cold Logs

Cold logs remain important.

They are the audit trail.

They preserve:

- raw history
- evidence
- decisions
- old context
- mistakes
- superseded plans
- migration sources

But they should not be the first active memory surface.

Cold logs are the basement archive.

Packets and buckets are the working desk.

---

## Architecture

```text
User input
  ->
Trigger validator
  ->
Matched hot context / routing packets
  ->
Standing bucket memory loaded
  ->
Prompt built
  ->
LLM responds
  ->
LLM proposes memory actions
  ->
Reducers validate and apply updates
  ->
Memory state persists
  ->
Event log records evidence
```

---

## Example: Project Memory

For a project-heavy ecosystem, names become deterministic recall handles.

Example trigger:

```text
chatty-cog
```

Primary packet:

```json
{
  "packet_id": "project_chatty_cog",
  "canonical_name": "Chatty-Cog",
  "aliases": ["chatty-cog", "chatty cog", "chattycog"],
  "summary": "Chatty-Cog is the local AI exoskeleton and module host for the Chatty ecosystem.",
  "current_role": "Core infrastructure, memory host, module shell.",
  "primary_buckets": [
    "core_infrastructure",
    "memory_and_context",
    "module_hosting"
  ],
  "adjacent_packets": [
    {
      "packet_id": "semantic_signal_alphabet",
      "reason": "Provides model-native bucket maps for memory."
    },
    {
      "packet_id": "rd_engine",
      "reason": "Provides reducer-governed durable state."
    },
    {
      "packet_id": "chattyfactory",
      "reason": "May use Chatty-Cog as a host, module target, or handoff surface."
    }
  ]
}
```

When the user says:

```text
Chatty-Cog needs this memory adapter.
```

The model receives a useful routing packet instead of a raw log fragment.

---

## Why Not Just Use Search?

Search can find a mention.

A routing packet explains the mention.

Search result:

```text
Line 18472: "Chatty-Cog should host modules."
```

Routing packet:

```text
Chatty-Cog is the local AI exoskeleton/module host.
It manages modules, memory layers, state notes, and local model orchestration.
This question likely touches Semantic Signal Alphabet, RD Engine, and ChattyFactory.
```

The first gives a fragment.

The second gives orientation.

---

## Model-Specific Memory

This system intentionally does not aim for universal model memory.

A model-generated bucket map belongs to the model that generated it.

A different model may need a different map.

That is a tradeoff, but a useful one.

Universal schemas can create semantic monoculture: every model forced through the same external filing cabinet.

This adapter prefers:

> model-specific drawers, explicit migration.

Migration workflow:

1. Load old bucket map.
2. Load old memory state.
3. Generate new bucket map with new model.
4. Ask new model to refile old memory into its own buckets.
5. Validate.
6. Preserve old state for audit.
7. Save new state.

Principle:

> New mind, new drawers.
>
> Bring the old notes, but let the new mind re-sort them.

---

## `UNASSIGNED` Is Not Failure

`UNASSIGNED` is an overflow tray.

It catches information that does not fit the current bucket map.

This is useful.

If `UNASSIGNED` fills with recurring patterns, that may indicate:

- bucket count is too low
- the domain is too broad
- a new bucket is needed
- the model changed
- the memory system needs migration
- or a separate adapter should exist

Do not silently delete `UNASSIGNED`.

Review it.

---

## Reducer Examples

### `remember`

```json
{
  "reducer": "remember",
  "bucket_id": 1,
  "value": "User is consolidating project presentation before public amplification.",
  "source": "user_explicit"
}
```

### `mark_stale`

```json
{
  "reducer": "mark_stale",
  "bucket_id": 6,
  "reason": "Date-specific plan may have passed."
}
```

### `register_trigger`

```json
{
  "reducer": "register_trigger",
  "canonical_name": "Chatty-Cog",
  "aliases": ["chatty-cog", "chatty cog", "chattycog"],
  "packet_id": "project_chatty_cog",
  "source": "user_explicit"
}
```

### `add_adjacency`

```json
{
  "reducer": "add_adjacency",
  "from_packet": "project_chatty_cog",
  "to_packet": "semantic_signal_alphabet",
  "relation": "shares_memory_layer",
  "reason": "SSA provides model-native bucket maps for the memory adapter."
}
```

---

## Suggested File Layout

```text
adapters/
  memory_rd_engine/
    README.md
    docs/
      00_premise.md
      01_model_defined_buckets.md
      02_rd_engine_memory_state.md
      03_hot_context_injection.md
      04_context_routing_packets.md
      05_model_specificity_and_migration.md
      06_unassigned_overflow.md
      07_safety_and_user_control.md
    schemas/
      bucket_map.schema.json
      memory_state.schema.json
      reducer_action.schema.json
      trigger_registry.schema.json
      hot_context_record.schema.json
      context_packet.schema.json
      memory_event.schema.json
      migration_record.schema.json
    examples/
      assistant_8_bucket_map.json
      assistant_memory_state.json
      maddie_hot_context.json
      chatty_cog_packet.json
      chattyfactory_packet.json
      semantic_signal_alphabet_packet.json
    src/
      memory_store.py
      reducers.py
      trigger_validator.py
      hot_context_injector.py
      packet_store.py
      packet_ranker.py
      prompt_format.py
      session_loop.py
      migrate.py
    tests/
      test_reducers.py
      test_trigger_matching.py
      test_packet_routing.py
      test_schema_validation.py
      test_migration.py
```

---

## MVP Scope

The first version should be deliberately small.

### MVP Goals

- Load a frozen bucket map.
- Load and save memory state.
- Store compact bucket values.
- Apply reducer actions.
- Append event logs.
- Register simple exact-match triggers.
- Inject hot context cards.
- Inject one primary context routing packet.
- Show adjacent packet names and reasons.
- Provide a CLI demo.

### MVP Non-Goals

- No embeddings.
- No fuzzy semantic search.
- No autonomous agent framework.
- No hidden memory mutation.
- No automatic sensitive memory injection.
- No universal cross-model memory.
- No giant graph traversal.
- No claim of infinite memory.

---

## Safety And User Control

Persistent memory should be inspectable and controllable.

Required principles:

- The user can inspect all memory.
- The user can delete or disable triggers.
- The user can clear buckets.
- The user can mark memory stale.
- The user can export memory.
- The user can disable automatic updates.
- Sensitive information requires stricter handling.
- Inferences should be marked as inferences.
- Stable facts should require explicit evidence.
- The event log should explain why memory exists.

Important rule:

> The runtime owns persistence.
>
> The LLM only proposes.

---

## What This Is Not

This is not:

- infinite memory
- mind reading
- consciousness
- a universal memory standard
- a replacement for all retrieval systems
- a replacement for all logs
- a vector database
- a fine-tuning method
- a guarantee of perfect recall

It is:

- compact
- bounded
- persistent
- model-native
- reducer-governed
- auditable
- migration-aware
- trigger-routable memory

---

## Public Framing

> LLM-Defined Persistent Memory lets a target model define its own semantic memory buckets, while a deterministic reducer engine stores, updates, and routes those memories across sessions.

---

## Related Docs

- [GLOSSARY.md](GLOSSARY.md)

---

## License

This project is licensed under the **GNU Affero General Public License v3.0**.

See `LICENSE` for the full AGPLv3 license text.

The intention is that improvements, hosted variants, network-accessible versions, adapters, and derivative systems remain available to the commons under the same license terms.

---

## Related Projects

- [Semantic Signal Alphabet](https://github.com/instance001/semantic-signal-alphabet)
  Provides the model-native bucket generation process.

- [Chatty Quest / RD Engine](https://github.com/instance001/chatty-quest)
  Provides the deterministic reducer/state pattern that inspired the persistence layer.

---

## Status

Early design / adapter specification.

This README describes the intended architecture and MVP direction. Implementation should begin with the smallest deterministic version:

```text
bucket map
+ reducer store
+ trigger registry
+ hot context card
+ context routing packet
+ JSONL event log
```

Keep it boring first.

Make it useful before making it clever.
