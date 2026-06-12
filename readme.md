\# LLM-Defined Persistent Memory



A model-native persistent memory adapter built from:



\* \[Semantic Signal Alphabet](https://github.com/instance001/semantic-signal-alphabet)

\* \[Chatty Quest / RD Engine](https://github.com/instance001/chatty-quest)



This project explores a compact memory architecture where an LLM defines its own semantic memory buckets, while a deterministic runtime stores and updates those buckets through governed reducer actions.

The examples in this repository use the surrounding Chatty ecosystem because the pattern was developed to solve real continuity and context-routing problems across many related local-first AI projects.



The goal is not “infinite memory.”



The goal is:



> bounded, compact, persistent, model-native continuity.



\---



\## Core Idea



Most assistant memory systems rely on one or more of the following:



\* long transcript logs,

\* summarization,

\* vector search,

\* human-defined databases,

\* external retrieval layers,

\* fine-tuning.



Those can work, but they often make the model repeatedly burn context and attention re-decoding old material.



This adapter takes a different approach:



1\. A target model generates a fixed semantic bucket map using the Semantic Signal Alphabet process.

2\. Those buckets become persistent memory slots.

3\. The RD Engine stores the current values of those slots.

4\. Reducers control all memory updates.

5\. The model reads a compact memory snapshot at the start of a session.

6\. Entity/project triggers can inject specific “hot context” cards only when relevant.

7\. Cold logs remain available as audit history, not active working memory.



In plain terms:



> The model defines the drawers.

> The host controls the notebook.

> Future sessions read notes written in a semantic shape the model already understands.



\---



\## Why This Exists



A large project ecosystem can produce enormous cold logs.



Cold logs are useful as archives, but they are inefficient as active memory.



Without a routing layer, a model may need to skim large text logs just to rediscover basic orientation:



\* What is Chatty-Cog?

\* What is ChattyFactory?

\* How does Semantic Signal Alphabet relate to RD Engine?

\* Which old repo is archived?

\* Which project is active?

\* What context is adjacent to this name?



That burns tokens on searching and deciphering instead of reasoning.



This project turns important memories into compact, structured records:



```text

User says: "Chatty-Cog"



System loads:

\- Chatty-Cog routing packet

\- brief current role

\- active status

\- related buckets

\- adjacent systems worth checking

```



The same token budget becomes useful context instead of a raw search fragment.



\---



\## Relationship to Semantic Signal Alphabet



\[Semantic Signal Alphabet](https://github.com/instance001/semantic-signal-alphabet) provides the model-native bucket layer.



A target model is given:



\* a domain,

\* a source vocabulary or source corpus,

\* a bucket count,

\* and an instruction to divide meaning into buckets plus `UNASSIGNED`.



The output is a frozen bucket map.



Example:



```json

{

&#x20; "model\_id": "qwen2.5-7b-instruct-q4\_k\_m",

&#x20; "bucket\_count": 8,

&#x20; "buckets": \[

&#x20;   {

&#x20;     "id": 0,

&#x20;     "label": "user\_preferences",

&#x20;     "description": "Likes, dislikes, style preferences, tools, working habits."

&#x20;   },

&#x20;   {

&#x20;     "id": 1,

&#x20;     "label": "active\_projects",

&#x20;     "description": "Current builds, repos, plans, milestones, and next actions."

&#x20;   }

&#x20; ],

&#x20; "unassigned": {

&#x20;   "id": -1,

&#x20;   "label": "UNASSIGNED",

&#x20;   "description": "Information that does not cleanly fit the current bucket map."

&#x20; }

}

```



The important principle:



> New model, new bucket map.



A bucket map is specific to the model that generated it. A different GGUF or cloud model may organize the same domain differently.



This is intentional.



Cross-model portability should happen through explicit migration, not fake sameness.



\---



\## Relationship to Chatty Quest / RD Engine



\[Chatty Quest](https://github.com/instance001/chatty-quest) contains the RD Engine pattern: deterministic state, reducer-governed updates, and host-controlled truth.



This adapter uses that idea for memory.



The LLM does not directly own persistent memory.



Instead:



1\. The LLM proposes memory actions.

2\. The runtime validates those actions.

3\. Reducers apply permitted changes.

4\. State is saved.

5\. Events are logged.



Example reducer action:



```json

{

&#x20; "reducer": "remember",

&#x20; "bucket\_id": 1,

&#x20; "value": "User is preparing the Chatty ecosystem for a one-year GitHub anniversary amplification pivot.",

&#x20; "source": "user\_explicit"

}

```



The host remains the source of truth.



The model may suggest.



The reducer decides.



\---



\## Memory Layers



This adapter uses several memory layers.



\### 1. Standing Bucket Memory



Always-available compact memory.



Example:



```text

\[active\_projects]

User is consolidating public project presentation before amplifying around the GitHub one-year anniversary.



\[working\_style]

User prefers direct critique, docs-first scaffolding, bounded LLM roles, and local-first tooling.

```



This gives the model general orientation.



\---



\### 2. Hot Context Cards



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



\---



\### 3. Context Routing Packets



Hot context cards can also act as routing packets.



A routing packet does not just say what something is.



It also says:



\* current role,

\* current status,

\* related projects,

\* related buckets,

\* adjacent records,

\* where to look next.



Example:



```text

Chatty-Cog:

Central local AI exoskeleton/module host.



Likely adjacent context:

\- Semantic Signal Alphabet

\- RD Engine

\- ChattyFactory

\- LLM Tweaker

\- Janet School

```



This turns memory into context routing, not just recall.



\---



\### 4. Cold Logs



Cold logs remain important.



They are the audit trail.



They preserve:



\* raw history,

\* evidence,

\* decisions,

\* old context,

\* mistakes,

\* superseded plans,

\* migration sources.



But they should not be the first active memory surface.



Cold logs are the basement archive.



Packets and buckets are the working desk.



\---



\## Architecture



```text

User input

&#x20; ↓

Trigger validator

&#x20; ↓

Matched hot context / routing packets

&#x20; ↓

Standing bucket memory loaded

&#x20; ↓

Prompt built

&#x20; ↓

LLM responds

&#x20; ↓

LLM proposes memory actions

&#x20; ↓

Reducers validate and apply updates

&#x20; ↓

Memory state persists

&#x20; ↓

Event log records evidence

```



\---



\## Example: Project Memory



For a project-heavy ecosystem, names become deterministic recall handles.



Example trigger:



```text

chatty-cog

```



Primary packet:



```json

{

&#x20; "packet\_id": "project\_chatty\_cog",

&#x20; "canonical\_name": "Chatty-Cog",

&#x20; "aliases": \["chatty-cog", "chatty cog", "chattycog"],

&#x20; "summary": "Chatty-Cog is the local AI exoskeleton and module host for the Chatty ecosystem.",

&#x20; "current\_role": "Core infrastructure, memory host, module shell.",

&#x20; "primary\_buckets": \[

&#x20;   "core\_infrastructure",

&#x20;   "memory\_and\_context",

&#x20;   "module\_hosting"

&#x20; ],

&#x20; "adjacent\_packets": \[

&#x20;   {

&#x20;     "packet\_id": "semantic\_signal\_alphabet",

&#x20;     "reason": "Provides model-native bucket maps for memory."

&#x20;   },

&#x20;   {

&#x20;     "packet\_id": "rd\_engine",

&#x20;     "reason": "Provides reducer-governed durable state."

&#x20;   },

&#x20;   {

&#x20;     "packet\_id": "chattyfactory",

&#x20;     "reason": "May use Chatty-Cog as a host, module target, or handoff surface."

&#x20;   }

&#x20; ]

}

```



When the user says:



```text

Chatty-Cog needs this memory adapter.

```



The model receives a useful routing packet instead of a raw log fragment.



\---



\## Why Not Just Use Search?



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



\---



\## Model-Specific Memory



This system intentionally does not aim for universal model memory.



A model-generated bucket map belongs to the model that generated it.



A different model may need a different map.



That is a tradeoff, but a useful one.



Universal schemas can create semantic monoculture: every model forced through the same external filing cabinet.



This adapter prefers:



> model-specific drawers, explicit migration.



Migration workflow:



1\. Load old bucket map.

2\. Load old memory state.

3\. Generate new bucket map with new model.

4\. Ask new model to refile old memory into its own buckets.

5\. Validate.

6\. Preserve old state for audit.

7\. Save new state.



Principle:



> New mind, new drawers.

> Bring the old notes, but let the new mind re-sort them.



\---



\## UNASSIGNED Is Not Failure



`UNASSIGNED` is an overflow tray.



It catches information that does not fit the current bucket map.



This is useful.



If `UNASSIGNED` fills with recurring patterns, that may indicate:



\* bucket count is too low,

\* the domain is too broad,

\* a new bucket is needed,

\* the model changed,

\* the memory system needs migration,

\* or a separate adapter should exist.



Do not silently delete `UNASSIGNED`.



Review it.



\---



\## Reducer Examples



\### remember



```json

{

&#x20; "reducer": "remember",

&#x20; "bucket\_id": 1,

&#x20; "value": "User is consolidating project presentation before public amplification.",

&#x20; "source": "user\_explicit"

}

```



\### mark\_stale



```json

{

&#x20; "reducer": "mark\_stale",

&#x20; "bucket\_id": 6,

&#x20; "reason": "Date-specific plan may have passed."

}

```



\### register\_trigger



```json

{

&#x20; "reducer": "register\_trigger",

&#x20; "canonical\_name": "Chatty-Cog",

&#x20; "aliases": \["chatty-cog", "chatty cog", "chattycog"],

&#x20; "packet\_id": "project\_chatty\_cog",

&#x20; "source": "user\_explicit"

}

```



\### add\_adjacency



```json

{

&#x20; "reducer": "add\_adjacency",

&#x20; "from\_packet": "project\_chatty\_cog",

&#x20; "to\_packet": "semantic\_signal\_alphabet",

&#x20; "relation": "shares\_memory\_layer",

&#x20; "reason": "SSA provides model-native bucket maps for the memory adapter."

}

```



\---



\## Suggested File Layout



```text

adapters/

&#x20; memory\_rd\_engine/

&#x20;   README.md

&#x20;   docs/

&#x20;     00\_premise.md

&#x20;     01\_model\_defined\_buckets.md

&#x20;     02\_rd\_engine\_memory\_state.md

&#x20;     03\_hot\_context\_injection.md

&#x20;     04\_context\_routing\_packets.md

&#x20;     05\_model\_specificity\_and\_migration.md

&#x20;     06\_unassigned\_overflow.md

&#x20;     07\_safety\_and\_user\_control.md

&#x20;   schemas/

&#x20;     bucket\_map.schema.json

&#x20;     memory\_state.schema.json

&#x20;     reducer\_action.schema.json

&#x20;     trigger\_registry.schema.json

&#x20;     hot\_context\_record.schema.json

&#x20;     context\_packet.schema.json

&#x20;     memory\_event.schema.json

&#x20;     migration\_record.schema.json

&#x20;   examples/

&#x20;     assistant\_8\_bucket\_map.json

&#x20;     assistant\_memory\_state.json

&#x20;     maddie\_hot\_context.json

&#x20;     chatty\_cog\_packet.json

&#x20;     chattyfactory\_packet.json

&#x20;     semantic\_signal\_alphabet\_packet.json

&#x20;   src/

&#x20;     memory\_store.py

&#x20;     reducers.py

&#x20;     trigger\_validator.py

&#x20;     hot\_context\_injector.py

&#x20;     packet\_store.py

&#x20;     packet\_ranker.py

&#x20;     prompt\_format.py

&#x20;     session\_loop.py

&#x20;     migrate.py

&#x20;   tests/

&#x20;     test\_reducers.py

&#x20;     test\_trigger\_matching.py

&#x20;     test\_packet\_routing.py

&#x20;     test\_schema\_validation.py

&#x20;     test\_migration.py

```



\---



\## MVP Scope



The first version should be deliberately small.



\### MVP Goals



\* Load a frozen bucket map.

\* Load and save memory state.

\* Store compact bucket values.

\* Apply reducer actions.

\* Append event logs.

\* Register simple exact-match triggers.

\* Inject hot context cards.

\* Inject one primary context routing packet.

\* Show adjacent packet names and reasons.

\* Provide a CLI demo.



\### MVP Non-Goals



\* No embeddings.

\* No fuzzy semantic search.

\* No autonomous agent framework.

\* No hidden memory mutation.

\* No automatic sensitive memory injection.

\* No universal cross-model memory.

\* No giant graph traversal.

\* No claim of infinite memory.



\---



\## Safety and User Control



Persistent memory should be inspectable and controllable.



Required principles:



\* The user can inspect all memory.

\* The user can delete or disable triggers.

\* The user can clear buckets.

\* The user can mark memory stale.

\* The user can export memory.

\* The user can disable automatic updates.

\* Sensitive information requires stricter handling.

\* Inferences should be marked as inferences.

\* Stable facts should require explicit evidence.

\* The event log should explain why memory exists.



Important rule:



> The runtime owns persistence.

> The LLM only proposes.



\---



\## What This Is Not



This is not:



\* infinite memory,

\* mind reading,

\* consciousness,

\* a universal memory standard,

\* a replacement for all retrieval systems,

\* a replacement for all logs,

\* a vector database,

\* a fine-tuning method,

\* a guarantee of perfect recall.



It is:



\* compact,

\* bounded,

\* persistent,

\* model-native,

\* reducer-governed,

\* auditable,

\* migration-aware,

\* trigger-routable memory.



\---



\## Public Framing



> LLM-Defined Persistent Memory lets a target model define its own semantic memory buckets, while a deterministic reducer engine stores, updates, and routes those memories across sessions.



\---



\## License



This project is licensed under the \*\*GNU Affero General Public License v3.0\*\*.



See `LICENSE` for the full AGPLv3 license text.



The intention is that improvements, hosted variants, network-accessible versions, adapters, and derivative systems remain available to the commons under the same license terms.



\---



\## Related Projects



\* \[Semantic Signal Alphabet](https://github.com/instance001/semantic-signal-alphabet)

&#x20; Provides the model-native bucket generation process.



\* \[Chatty Quest / RD Engine](https://github.com/instance001/chatty-quest)

&#x20; Provides the deterministic reducer/state pattern that inspired the persistence layer.



\---



\## Status



Early design / adapter specification.



This README describes the intended architecture and MVP direction. Implementation should begin with the smallest deterministic version:



```text

bucket map

\+ reducer store

\+ trigger registry

\+ hot context card

\+ context routing packet

\+ JSONL event log

```



Keep it boring first.



Make it useful before making it clever.



