\# Context Routing Packets



\## Using Hot Memory Cards to Replace Wasteful Cold-Log Skimming



\## 1. Summary



Context Routing Packets are a memory-routing layer for large, interconnected project ecosystems.



They solve a specific problem:



> A cold log can preserve history, but it is a terrible active working memory surface.



A model should not have to skim a giant text file end-to-end every time the user mentions a project name, tool, repo, module, person, device, or recurring concept.



Instead, the system should maintain compact \*\*routing packets\*\*: structured cards that tell the model what the named thing is, why it matters, where it belongs, what is adjacent, and which other buckets/cards may be worth loading next.



Example:



```text

User says:

"Chatty-Cog needs the memory adapter."



Old cold-log approach:

Search huge logs for "Chatty-Cog".

Return one or more raw mentions.

Model burns tokens figuring out what matters.



Context routing approach:

Trigger "Chatty-Cog".

Inject Chatty-Cog routing packet:

\- what it is

\- current role

\- current status

\- related projects

\- likely relevant memory buckets

\- suggested adjacent cards

```



This shifts token use from \*\*finding and deciphering\*\* to \*\*reasoning and connecting\*\*.



The goal is not merely recall.



The goal is:



> Mentioned thing → primary card → adjacent context → targeted continuation.



\---



\## 2. Core Problem



Cold logs are good archives but poor active memory.



A cold log may contain everything:



\* full transcripts,

\* project notes,

\* module rundowns,

\* decisions,

\* jokes,

\* abandoned ideas,

\* obsolete plans,

\* current plans,

\* contradictions,

\* repeated context,

\* irrelevant detail.



But when a model needs to continue work, it does not need everything.



It needs:



```text

What is this thing?

What is its current role?

What is still true?

What changed?

What is adjacent?

Where should I look next?

```



The current cold-log pattern often forces the model to do this:



```text

Search term found.

Read nearby text.

Guess whether current.

Search again.

Compare old and new notes.

Burn tokens.

Maybe retrieve one useful line.

```



This is wasteful.



A better system performs the standout extraction earlier and stores it as a reusable routing packet.



\---



\## 3. Key Principle



A cold log should be the archive, not the working desk.



```text

Cold log:

Full evidence trail. Rarely injected directly.



Routing packet:

Compact working card. Injected when relevant.



Bucket map:

Model-native semantic filing system.



Trigger registry:

Deterministic handles for fast recall.



Hot context:

Turn-specific injection of relevant cards.



Adjacent context:

Model-assisted but bounded follow-on retrieval.

```



The model should not be asked to walk the entire warehouse every time.



It should be handed the right labelled tray.



\---



\## 4. What Is a Context Routing Packet?



A Context Routing Packet is a compact memory card attached to a known entity, project, module, concept, or recurring topic.



It answers:



1\. What is this?

2\. What role does it currently play?

3\. What bucket or shelf does it belong in?

4\. What related things are nearby?

5\. What details are safe to inject now?

6\. Where should the model look next if deeper context is needed?



A packet is not a full transcript.



It is not a summary of every conversation.



It is a \*\*navigation card\*\*.



\---



\## 5. Example: Chatty-Cog Routing Packet



```json

{

&#x20; "packet\_id": "packet\_project\_chatty\_cog",

&#x20; "type": "project",

&#x20; "canonical\_name": "Chatty-Cog",

&#x20; "aliases": \[

&#x20;   "chatty-cog",

&#x20;   "chatty cog",

&#x20;   "chattycog"

&#x20; ],

&#x20; "summary": "Chatty-Cog is the local AI exoskeleton and module host for the Chatty ecosystem. It provides module tabs, state notes, cold/luke-warm memory handling, local model orchestration, and a central body for connected Chatty tools.",

&#x20; "current\_role": "Core infrastructure / memory host / module shell.",

&#x20; "current\_status": "Active ecosystem spine. Related memory-routing, module, and local-model work should treat it as a primary integration target.",

&#x20; "primary\_buckets": \[

&#x20;   "core\_infrastructure",

&#x20;   "memory\_and\_context",

&#x20;   "module\_hosting"

&#x20; ],

&#x20; "adjacent\_packets": \[

&#x20;   {

&#x20;     "packet\_id": "packet\_project\_chattyfactory",

&#x20;     "reason": "Build/patch factory may use Chatty-Cog as host, module target, or handoff surface."

&#x20;   },

&#x20;   {

&#x20;     "packet\_id": "packet\_concept\_semantic\_signal\_alphabet",

&#x20;     "reason": "SSA can provide model-native bucket maps for memory/context systems."

&#x20;   },

&#x20;   {

&#x20;     "packet\_id": "packet\_component\_rd\_engine",

&#x20;     "reason": "RD Engine provides reducer-governed durable state, useful for memory persistence."

&#x20;   },

&#x20;   {

&#x20;     "packet\_id": "packet\_module\_llm\_tweaker",

&#x20;     "reason": "LLM Tweaker can tune local GGUF model behavior for bounded Chatty modules."

&#x20;   },

&#x20;   {

&#x20;     "packet\_id": "packet\_module\_janet\_school",

&#x20;     "reason": "Janet School may need compact experiment-state memory and telemetry summaries."

&#x20;   }

&#x20; ],

&#x20; "hot\_context": "Chatty-Cog is the central local AI exoskeleton/module host. It is where memory, modules, notes, model orchestration, and connected Chatty tools converge.",

&#x20; "last\_updated": "2026-06-13",

&#x20; "confidence": 0.96,

&#x20; "source": "user\_explicit",

&#x20; "status": "active"

}

```



When the user mentions Chatty-Cog, the model gets orientation immediately.



It does not need to skim the archive to remember what Chatty-Cog is.



\---



\## 6. Context Routing vs Search



\### Search Result



A search result says:



```text

Here is a place where the term appeared.

```



Example:



```text

Line 18472: "Chatty-Cog should eventually host modules."

```



This is weak.



The model still has to work out:



\* Is this current?

\* Is this important?

\* What is Chatty-Cog?

\* What modules?

\* What adjacent work matters?

\* What should I do next?



\### Context Routing Packet



A routing packet says:



```text

Here is the thing, its current role, and the nearby shelves.

```



Example:



```text

Chatty-Cog is the module host and memory exoskeleton.

This question likely touches memory/context, RD Engine, SSA, and ChattyFactory.

```



This is stronger.



The same token budget produces a useful context map instead of a raw fragment.



\---



\## 7. Token Economics



Cold-log retrieval burns tokens in three ways:



1\. Search returns fragments.

2\. Model reads fragments.

3\. Model deciphers whether fragments matter.



Context routing shifts that cost:



```text

At ingestion time:

extract standout

build card

link adjacency

store packet



At recall time:

trigger packet

inject compact card

optionally load adjacent packets

```



The same number of tokens can now carry:



```text

name

brief rundown

current role

status

bucket location

adjacent systems

next search hints

```



Instead of:



```text

one raw line from an old text file

```



This is the core efficiency gain.



\---



\## 8. Layered Memory Model



Context Routing Packets fit into a layered memory system.



```text

Layer 1: Standing Bucket Memory

Small model-native memory snapshot injected broadly.



Layer 2: Triggered Hot Context

Specific records injected when deterministic triggers fire.



Layer 3: Context Routing Packets

Hot context records that include adjacency and next-search hints.



Layer 4: Cold Logs

Full raw archive, used for audit, repair, dispute, and deep research.



Layer 5: Evidence Events

Append-only trail explaining why each packet exists and when it changed.

```



The model mostly works from layers 1–3.



Layers 4–5 remain available but are not dragged into every session.



\---



\## 9. Trigger-to-Routing Workflow



```text

1\. User sends input.

2\. Runtime normalizes input.

3\. Trigger validator detects known entity/concept/project.

4\. Runtime loads primary routing packet.

5\. Runtime injects primary packet into context.

6\. Model reads packet.

7\. Model may request or suggest adjacent packets.

8\. Runtime loads bounded adjacent context if budget allows.

9\. Model responds with much better orientation.

```



Example:



```text

User:

"Chatty-Cog needs the hot context adapter."



Trigger:

chatty-cog



Primary packet:

Chatty-Cog project card.



Adjacent packet suggestions:

\- SSA memory buckets

\- RD Engine reducers

\- ChattyFactory integration

\- LLM Tweaker model behavior

```



Now the model can reason in-system instead of re-discovering the system.



\---



\## 10. Primary Packet vs Adjacent Packet



\### Primary Packet



The primary packet is triggered directly by user input.



Example:



```text

User says "Chatty-Cog".

Load Chatty-Cog packet.

```



\### Adjacent Packet



Adjacent packets are not directly requested but may be relevant.



Example:



```text

Chatty-Cog packet points to:

\- Semantic Signal Alphabet

\- RD Engine

\- ChattyFactory

```



Adjacent packets should not all be injected automatically every time.



They should be selected based on:



\* user message,

\* current task,

\* token budget,

\* packet relevance,

\* exact trigger strength,

\* current project status.



\---



\## 11. Adjacency Types



Not all adjacency is the same.



Recommended adjacency labels:



```text

depends\_on

feeds\_into

shares\_memory\_layer

same\_module\_family

implementation\_target

theory\_support

archived\_predecessor

current\_successor

integration\_candidate

example\_use\_case

risk\_related

```



Example:



```json

{

&#x20; "packet\_id": "packet\_concept\_semantic\_signal\_alphabet",

&#x20; "relation": "shares\_memory\_layer",

&#x20; "reason": "SSA provides model-native bucket maps used by the proposed memory adapter."

}

```



This tells the model not just “related,” but \*\*how\*\* related.



\---



\## 12. Packet Schema



Recommended base schema:



```json

{

&#x20; "packet\_id": "string",

&#x20; "type": "project | module | concept | repo | person | device | event | decision | adapter | archive",

&#x20; "canonical\_name": "string",

&#x20; "aliases": \["string"],

&#x20; "summary": "string",

&#x20; "current\_role": "string",

&#x20; "current\_status": "active | parked | archived | superseded | experimental | planned",

&#x20; "primary\_buckets": \["string"],

&#x20; "adjacent\_packets": \[

&#x20;   {

&#x20;     "packet\_id": "string",

&#x20;     "relation": "string",

&#x20;     "reason": "string",

&#x20;     "priority": 0.0

&#x20;   }

&#x20; ],

&#x20; "hot\_context": "string",

&#x20; "evidence\_refs": \["string"],

&#x20; "last\_updated": "YYYY-MM-DD",

&#x20; "confidence": 0.0,

&#x20; "source": "user\_explicit | assistant\_inferred | imported | migration | manual",

&#x20; "status": "active | stale | needs\_review | archived"

}

```



The `hot\_context` field should be short enough for injection.



The `summary` can be longer but still compact.



The `adjacent\_packets` field turns the card into a router.



\---



\## 13. Packet Types



\### Project Packet



For major projects.



Examples:



\* Chatty-Cog

\* ChattyFactory

\* Chatty-Art

\* Chatty-Quest

\* Semantic Signal Alphabet

\* LLM Tweaker

\* Janet School



\### Module Packet



For components inside larger hosts.



Examples:



\* LLM Tweaker as ChattyCog module

\* Janet School as ChattyCog module

\* Dream Forge future module



\### Concept Packet



For recurring conceptual systems.



Examples:



\* model-native memory

\* hot context injection

\* quiet build mode

\* archive museum

\* Dream Forge

\* entropy folding



\### Repo Packet



For specific GitHub repos.



Examples:



\* semantic-signal-alphabet

\* chatty-mobile

\* chattydoom

\* archived theory repos



\### Device Packet



For important hardware context.



Examples:



\* Wraith PC

\* Samsung A21s

\* RX 7600 XT

\* Rode mic



\### Decision Packet



For important architecture decisions.



Examples:



\* “LLM hands out of machinery”

\* “theory videos stay off YouTube”

\* “Chatty brand is friend-shaped tool, suffix sets boundary”

\* “new model means new SSA bucket map”



\---



\## 14. Project Ecosystem Example



For the Chatty ecosystem, a packet graph might look like:



```text

Chatty-Cog

├── ChattyFactory

├── Semantic Signal Alphabet

│   └── LLM-Defined Persistent Memory

│       └── Hot Context Injection

├── RD Engine

│   └── Chatty-Quest

├── LLM Tweaker

└── Janet School

```



A user mention of `Chatty-Cog` should orient the model toward:



```text

memory

modules

local models

Chatty ecosystem integration

state notes

context routing

```



A user mention of `Chatty-Quest` should orient toward:



```text

deterministic host

AI narration

RD Engine

scenario/datapack structure

game state

screenshots/public hook

```



A user mention of `Semantic Signal Alphabet` should orient toward:



```text

model-native buckets

bucket count as bandwidth

UNASSIGNED

model-specific maps

memory adapter

BCI/signal bucket origins

```



\---



\## 15. Packet Generation at Ingestion Time



The best time to create packets is when new information enters the system.



During or after a session, the model/runtime can identify standouts:



```text

new project

new decision

new status change

new active priority

new entity

new module

new repo

new relationship between projects

```



Instead of waiting until future sessions to rediscover them, the system creates or updates packets immediately.



Ingestion flow:



```text

1\. Session event enters log.

2\. Standout extractor identifies possible packet update.

3\. LLM proposes packet action.

4\. Reducer validates packet action.

5\. Packet store updates.

6\. Event log records why.

```



\---



\## 16. Packet Reducers



Packets should be updated through reducers, not direct LLM mutation.



Recommended reducers:



\### create\_packet



Creates a new routing packet.



```json

{

&#x20; "reducer": "create\_packet",

&#x20; "packet\_id": "packet\_project\_chatty\_cog",

&#x20; "type": "project",

&#x20; "canonical\_name": "Chatty-Cog",

&#x20; "summary": "Local AI exoskeleton and module host.",

&#x20; "source": "user\_explicit"

}

```



\### update\_packet\_summary



Updates the summary.



```json

{

&#x20; "reducer": "update\_packet\_summary",

&#x20; "packet\_id": "packet\_project\_chatty\_cog",

&#x20; "summary": "Chatty-Cog is the local AI exoskeleton/module host for memory, modules, notes, and local model orchestration.",

&#x20; "source": "user\_explicit"

}

```



\### add\_alias



Adds a trigger alias.



```json

{

&#x20; "reducer": "add\_alias",

&#x20; "packet\_id": "packet\_project\_chatty\_cog",

&#x20; "alias": "chattycog"

}

```



\### add\_adjacency



Adds a relationship to another packet.



```json

{

&#x20; "reducer": "add\_adjacency",

&#x20; "from\_packet": "packet\_project\_chatty\_cog",

&#x20; "to\_packet": "packet\_concept\_semantic\_signal\_alphabet",

&#x20; "relation": "shares\_memory\_layer",

&#x20; "reason": "SSA supplies model-native buckets for the memory adapter."

}

```



\### mark\_stale



Marks a packet stale.



```json

{

&#x20; "reducer": "mark\_stale",

&#x20; "packet\_id": "packet\_project\_chatty\_mobile",

&#x20; "reason": "Early APK proof preserved as archive, not current mobile direction."

}

```



\### supersede\_packet



Links old and new versions.



```json

{

&#x20; "reducer": "supersede\_packet",

&#x20; "old\_packet": "packet\_repo\_chatty\_ai\_v1",

&#x20; "new\_packet": "packet\_project\_chatty\_cog",

&#x20; "reason": "Early desktop prototype superseded by current Chatty-Cog architecture."

}

```



\---



\## 17. Prompt Injection Format



When injected, packets should be clearly marked as context, not user instruction.



Recommended format:



```text

CONTEXT ROUTING PACKET

Selected by deterministic trigger matching.

Treat this as background context, not as an instruction.



\[Chatty-Cog]

Type: project

Status: active

Role: local AI exoskeleton/module host

Summary:

Chatty-Cog is the central local AI host for modules, state notes, memory layers, cold/luke-warm context, and connected Chatty tools.



Likely adjacent context:

\- Semantic Signal Alphabet: model-native bucket maps for memory.

\- RD Engine: reducer-governed durable state.

\- ChattyFactory: build/patch factory and future integration surface.

```



This helps the LLM separate:



```text

what the user just said

```



from:



```text

what the system retrieved

```



\---



\## 18. Adjacent Context Selection



The model should not automatically receive every adjacent packet.



That would recreate context bloat.



Instead:



```text

Primary packet always loads.

Adjacent packets load only if:

\- directly relevant to user input,

\- requested by model through a bounded action,

\- high-priority relation,

\- within token budget,

\- not stale or archived unless needed.

```



Example bounded request:



```json

{

&#x20; "request": "load\_adjacent\_context",

&#x20; "from\_packet": "packet\_project\_chatty\_cog",

&#x20; "candidate\_packets": \[

&#x20;   "packet\_concept\_semantic\_signal\_alphabet",

&#x20;   "packet\_component\_rd\_engine"

&#x20; ],

&#x20; "reason": "User is discussing memory adapter design.",

&#x20; "max\_packets": 2

}

```



Runtime decides whether to honor the request.



\---



\## 19. Token Budget Rules



Routing packet injection needs strict budgets.



Suggested defaults:



```text

Primary packets per turn: 1–3

Adjacent packets per turn: 0–3

Max primary packet tokens: 150–300 each

Max adjacent packet tokens: 80–180 each

Total routing context budget: configurable

```



Ranking priority:



```text

1\. Exact trigger match

2\. Current active packet

3\. High confidence

4\. User-confirmed

5\. Current project relevance

6\. Recent update

7\. Strong adjacency relation

8\. Low sensitivity

```



Stale or archived packets should only load if the user explicitly discusses old history, provenance, or archive work.



\---



\## 20. Avoiding Packet Bloat



Packets can become too large.



Bad packet:



```text

Chatty-Cog: giant essay containing every detail ever discussed.

```



Good packet:



```text

Chatty-Cog: current role, concise summary, status, adjacency hints, evidence links.

```



The full history belongs in cold logs.



The packet is a routing card.



Recommended limits:



```text

hot\_context: 1–5 sentences

summary: 1–3 short paragraphs

adjacent packets: top 5–12 only

evidence refs: IDs, not full quotes

```



If more detail is needed, the packet can point to deeper records.



\---



\## 21. Avoiding False Currency



A packet may become outdated.



Example:



```text

ChattyFactory current focus: basic file sorter.

```



Later:



```text

ChattyFactory current focus: final-form patch reliability and project analysis scaffold.

```



The old packet must update or mark the old detail stale.



Packet fields should distinguish:



```text

historical\_role

current\_role

current\_status

last\_updated

confidence

stale\_after

```



This prevents old context from masquerading as current truth.



\---



\## 22. Current vs Historical Packets



Some packets are living.



Some are museum artifacts.



Example:



```json

{

&#x20; "packet\_id": "packet\_repo\_chatty\_mobile",

&#x20; "type": "repo",

&#x20; "canonical\_name": "chatty-mobile",

&#x20; "current\_status": "archived",

&#x20; "summary": "Early Android/APK proof-of-concept for Chatty mobile, preserved as historical artifact.",

&#x20; "historical\_note": "Built shortly after the first desktop Chatty prototype. APK was distributed via Archive.org due to browser GitHub upload limits. Target phone likely could not handle the oversized GGUF.",

&#x20; "adjacent\_packets": \[

&#x20;   {

&#x20;     "packet\_id": "packet\_project\_chatty\_cog",

&#x20;     "relation": "archived\_predecessor",

&#x20;     "reason": "Early mobile proof relates to later Chatty ecosystem direction."

&#x20;   }

&#x20; ]

}

```



This lets the system answer archive/history questions without confusing old prototypes for active direction.



\---



\## 23. Packet Graph



As packets accumulate, they form a graph.



Nodes:



```text

projects

modules

concepts

devices

decisions

repos

people

events

```



Edges:



```text

depends\_on

feeds\_into

supersedes

related\_to

implements

uses

archives

blocks

supports

```



This graph becomes the ecosystem map.



For the Chatty stack, the packet graph is likely more useful than raw chronological logs because many projects are interconnected across time.



The user may ask from any node.



The model should be able to load:



```text

current node

nearest relevant neighbours

only deeper archive if needed

```



\---



\## 24. Context Routing as Project X-Ray



For project-heavy workflows, packets act like an X-ray.



Instead of reading the whole repo or conversation history, the model sees:



```text

What is this project?

What are its organs?

What are its adjacent systems?

What is current?

What is archived?

What are known hazards?

What is the next likely work surface?

```



This directly supports ChattyFactory’s future project-analysis scaffold.



A factory patcher could load:



```text

project packet

current architecture packet

known failure packet

recent decision packet

test/validation packet

```



Then patch with far less risk.



\---



\## 25. Example: ChattyFactory Routing Packet



```json

{

&#x20; "packet\_id": "packet\_project\_chattyfactory",

&#x20; "type": "project",

&#x20; "canonical\_name": "ChattyFactory",

&#x20; "aliases": \[

&#x20;   "chattyfactory",

&#x20;   "chatty-factory",

&#x20;   "factory"

&#x20; ],

&#x20; "summary": "ChattyFactory is the local-first build/patch factory project. It aims to turn plain-language requests into working artifacts using deterministic host machinery and bounded LLM roles.",

&#x20; "current\_role": "Build/patch engine and future self-expanding ecosystem tool.",

&#x20; "current\_status": "Active; needs reliability, repeatability, patch safety, and final-form grind after front-door cleanup.",

&#x20; "primary\_buckets": \[

&#x20;   "build\_systems",

&#x20;   "patch\_reliability",

&#x20;   "local\_first\_tools"

&#x20; ],

&#x20; "adjacent\_packets": \[

&#x20;   {

&#x20;     "packet\_id": "packet\_project\_chatty\_cog",

&#x20;     "relation": "integration\_candidate",

&#x20;     "reason": "ChattyCog may host ChattyFactory as module/body or provide memory/context support."

&#x20;   },

&#x20;   {

&#x20;     "packet\_id": "packet\_module\_llm\_tweaker",

&#x20;     "relation": "supports",

&#x20;     "reason": "LLM Tweaker can help shape bounded local models for factory roles."

&#x20;   },

&#x20;   {

&#x20;     "packet\_id": "packet\_concept\_syntax\_repair\_book",

&#x20;     "relation": "supports",

&#x20;     "reason": "Syntax repair examples help local models fix compile/lint failures."

&#x20;   },

&#x20;   {

&#x20;     "packet\_id": "packet\_concept\_project\_xray",

&#x20;     "relation": "future\_scaffold",

&#x20;     "reason": "Factory needs project-analysis anatomy maps before safe patching."

&#x20;   }

&#x20; ],

&#x20; "hot\_context": "ChattyFactory is the local-first build/patch factory. Core principle: deterministic host machinery, bounded LLM role, runner truth authority, and repair loops grounded by actual compile/test output.",

&#x20; "last\_updated": "2026-06-13",

&#x20; "confidence": 0.96,

&#x20; "source": "user\_explicit",

&#x20; "status": "active"

}

```



A mention of ChattyFactory should immediately orient the model toward build/patch reliability, bounded LLM roles, syntax repair, and project analysis scaffolds.



\---



\## 26. Example: Semantic Signal Alphabet Routing Packet



```json

{

&#x20; "packet\_id": "packet\_concept\_semantic\_signal\_alphabet",

&#x20; "type": "concept\_repo",

&#x20; "canonical\_name": "Semantic Signal Alphabet",

&#x20; "aliases": \[

&#x20;   "ssa",

&#x20;   "semantic-signal-alphabet",

&#x20;   "signal alphabet"

&#x20; ],

&#x20; "summary": "Semantic Signal Alphabet is a model-native semantic compression framework. A target model sorts a source vocabulary or domain into a fixed number of buckets plus UNASSIGNED. Bucket count equals bandwidth.",

&#x20; "current\_role": "Provides model-native bucket maps for low-bandwidth signal systems and persistent memory adapters.",

&#x20; "current\_status": "Published as repo #60 and relevant to memory, BCI-style buckets, Dream Forge, and context routing.",

&#x20; "primary\_buckets": \[

&#x20;   "semantic\_compression",

&#x20;   "model\_native\_memory",

&#x20;   "signal\_buckets"

&#x20; ],

&#x20; "adjacent\_packets": \[

&#x20;   {

&#x20;     "packet\_id": "packet\_adapter\_llm\_defined\_memory",

&#x20;     "relation": "implements",

&#x20;     "reason": "LLM-defined persistent memory uses SSA buckets as durable memory slots."

&#x20;   },

&#x20;   {

&#x20;     "packet\_id": "packet\_adapter\_hot\_context\_injection",

&#x20;     "relation": "supports",

&#x20;     "reason": "Hot context can use SSA bucket links to find relevant records."

&#x20;   },

&#x20;   {

&#x20;     "packet\_id": "packet\_concept\_dream\_forge",

&#x20;     "relation": "example\_use\_case",

&#x20;     "reason": "Dream Forge can use buckets to classify weak sleep/dream fragments."

&#x20;   }

&#x20; ],

&#x20; "hot\_context": "SSA lets a specific model define semantic buckets for a domain. Bucket count is bandwidth. Maps are model-specific and include UNASSIGNED overflow.",

&#x20; "last\_updated": "2026-06-13",

&#x20; "confidence": 0.96,

&#x20; "source": "user\_explicit",

&#x20; "status": "active"

}

```



\---



\## 27. Routing Packet Store



Packets should live in a structured store.



Possible files:



```text

context\_packets/

&#x20; projects/

&#x20;   chatty\_cog.json

&#x20;   chattyfactory.json

&#x20;   chatty\_art.json

&#x20;   chatty\_quest.json

&#x20; concepts/

&#x20;   semantic\_signal\_alphabet.json

&#x20;   llm\_defined\_memory.json

&#x20;   hot\_context\_injection.json

&#x20;   dream\_forge.json

&#x20; modules/

&#x20;   llm\_tweaker.json

&#x20;   janet\_school.json

&#x20; archive/

&#x20;   chatty\_mobile.json

&#x20;   chatty\_ai\_v1.json

```



Or one JSONL file:



```text

context\_packets.jsonl

```



JSONL is easier to append.



Separate files are easier to inspect manually.



The MVP can use whichever is simplest.



\---



\## 28. Recommended File Layout



```text

adapters/

&#x20; memory\_rd\_engine/

&#x20;   context\_routing/

&#x20;     README.md

&#x20;     docs/

&#x20;       00\_premise.md

&#x20;       01\_packet\_schema.md

&#x20;       02\_trigger\_to\_packet\_flow.md

&#x20;       03\_adjacency\_graph.md

&#x20;       04\_token\_budget.md

&#x20;       05\_packet\_reducers.md

&#x20;       06\_archive\_packets.md

&#x20;       07\_project\_examples.md

&#x20;     schemas/

&#x20;       context\_packet.schema.json

&#x20;       packet\_adjacency.schema.json

&#x20;       packet\_match\_event.schema.json

&#x20;     examples/

&#x20;       chatty\_cog\_packet.json

&#x20;       chattyfactory\_packet.json

&#x20;       ssa\_packet.json

&#x20;       chatty\_mobile\_archive\_packet.json

&#x20;     src/

&#x20;       packet\_store.py

&#x20;       packet\_validator.py

&#x20;       packet\_ranker.py

&#x20;       packet\_injector.py

&#x20;       adjacency\_loader.py

&#x20;       packet\_reducers.py

&#x20;     tests/

&#x20;       test\_packet\_trigger.py

&#x20;       test\_adjacency\_ranking.py

&#x20;       test\_archive\_packet\_handling.py

&#x20;       test\_token\_budget.py

```



\---



\## 29. MVP Implementation



The first implementation should be boring and deterministic.



\### MVP Goals



```text

\- Load context packets from JSON.

\- Match exact canonical names and aliases.

\- Inject primary packet.

\- Include top adjacent packet names/reasons.

\- Do not automatically inject full adjacent packets.

\- Log packet match events.

\- Support manual packet creation/editing.

```



\### MVP Non-Goals



```text

\- No embeddings.

\- No fuzzy semantic search.

\- No automatic graph traversal beyond one hop.

\- No hidden packet creation.

\- No sensitive context auto-injection.

\- No automatic replacement of cold logs.

```



\### MVP Flow



```text

User input:

"Chatty-Cog needs this adapter."



Validator:

matches packet\_project\_chatty\_cog



Injector:

loads packet

formats compact routing context



Prompt:

includes Chatty-Cog packet and adjacent hints



Model:

responds with correct ecosystem context

```



\---



\## 30. Packet Match Event



Every packet injection should be logged.



Example:



```json

{

&#x20; "event\_id": "packetmatch\_2026\_06\_13\_001",

&#x20; "timestamp": "2026-06-13T12:10:00Z",

&#x20; "input\_excerpt": "Chatty-Cog needs this adapter.",

&#x20; "matched\_packets": \[

&#x20;   {

&#x20;     "packet\_id": "packet\_project\_chatty\_cog",

&#x20;     "matched\_text": "Chatty-Cog",

&#x20;     "match\_type": "canonical",

&#x20;     "confidence": 0.99,

&#x20;     "injected": true

&#x20;   }

&#x20; ],

&#x20; "adjacent\_hints\_included": \[

&#x20;   "packet\_concept\_semantic\_signal\_alphabet",

&#x20;   "packet\_component\_rd\_engine",

&#x20;   "packet\_project\_chattyfactory"

&#x20; ]

}

```



This allows debugging:



```text

Why did this context appear?

What matched?

Was it exact or alias?

Which adjacent hints were shown?

```



\---



\## 31. Relationship to Cold Logs



Cold logs remain important.



They are not replaced.



They become:



```text

audit trail

raw evidence

historical record

repair source

migration source

deep research source

```



But routine conversation should not rely on them first.



Routing priority should be:



```text

1\. Standing bucket state

2\. Exact context packet

3\. Adjacent packet hints

4\. Selected adjacent packet injection

5\. Cold log search only if needed

```



Cold log search becomes an escalation path, not the default.



\---



\## 32. Relationship to Search



Search can still be useful.



But packet routing makes search smarter.



Instead of:



```text

search everything for "Chatty-Cog"

```



The model can search:



```text

Chatty-Cog packet says memory adapter likely touches:

\- SSA

\- RD Engine

\- ChattyFactory

\- ChattyCog module system



Search only those shelves.

```



This reduces search surface.



The routing packet becomes a query planner.



\---



\## 33. Relationship to SSA Buckets



Context packets can be filed inside SSA buckets.



SSA buckets answer:



```text

What kind of memory space does this belong to?

```



Packets answer:



```text

What specific thing is this?

```



Example:



```text

Bucket:

memory\_and\_context\_systems



Packets inside:

\- ChattyCog memory layers

\- Semantic Signal Alphabet

\- LLM-defined persistent memory

\- Hot context injection

\- Context routing packets

\- Dream Forge fragment memory

```



The model-specific bucket map provides semantic drawers.



Packets provide named objects inside those drawers.



\---



\## 34. Relationship to RD Engine



RD Engine governs packet updates.



Packets should not mutate freely.



The LLM proposes:



```text

Create packet.

Update summary.

Add adjacency.

Mark stale.

Supersede packet.

```



The host validates and applies reducers.



This keeps the packet graph deterministic and inspectable.



\---



\## 35. Relationship to ChattyCog



ChattyCog can use context packets as its active orientation layer.



A ChattyCog session can load:



```text

standing memory bucket summary

\+

triggered project packets

\+

adjacent hints

```



This gives a returning assistant a much better starting point than a massive cold log.



For a large ecosystem, ChattyCog needs to know not just memories, but \*\*where memories sit in the machine\*\*.



Context packets provide that map.



\---



\## 36. Relationship to ChattyFactory



ChattyFactory can use packets to reduce patching risk.



Before modifying a project, it can load:



```text

project packet

architecture packet

known hazards packet

current status packet

recent decision packet

test/validation packet

```



This becomes a project X-ray.



The factory does not need to rediscover the project from scratch every time.



It receives the current anatomy map.



\---



\## 37. Risks



\### Risk: Packet Staleness



Old packet summaries may mislead the model.



Mitigation:



```text

last\_updated

current\_status

stale flags

evidence refs

periodic review

```



\### Risk: Packet Bloat



Packets become giant summaries.



Mitigation:



```text

strict token limits

hot\_context field separate from long summary

cold logs remain archive

```



\### Risk: Over-Injection



Too many adjacent packets load.



Mitigation:



```text

budget caps

ranked adjacency

one-hop default

manual escalation

```



\### Risk: Bad Alias Matches



Common aliases misfire.



Mitigation:



```text

exact match first

context gating

confidence thresholds

do not use vague aliases early

```



\### Risk: Model Overtrust



Model treats packet as current truth when stale.



Mitigation:



```text

status fields

confidence fields

source fields

stale markers

archive/current split

```



\---



\## 38. Public Framing



Good description:



> Context Routing Packets are compact memory cards that turn a matched project, entity, or concept name into useful orientation context and adjacent lookup hints.



Avoid:



```text

infinite memory

perfect recall

mind map of everything

autonomous knowledge graph

AI self-awareness

```



Use:



```text

compact

deterministic

auditable

triggered

project-oriented

context routing

token-efficient

```



Strong one-liner:



> Hot context is not just recall; it is context routing.



\---



\## 39. Final Principle



The goal is not to make the model remember everything.



The goal is to stop wasting tokens on rediscovering the obvious.



When the user says:



```text

Chatty-Cog

```



The system should not return:



```text

one old line where Chatty-Cog appeared

```



It should return:



```text

Chatty-Cog:

what it is,

what it currently does,

where it sits,

what is adjacent,

and where to look next.

```



That is the difference between a cold archive and a working memory system.



Cold logs preserve history.



Context Routing Packets preserve usable orientation.



