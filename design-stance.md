\## Design Stance



This repository is a guide, not a rulebook.



The patterns described here are application-agnostic. They can be adapted for personal assistants, project memory, game engines, research logs, local AI toolchains, telemetry systems, education tools, or any other system where an LLM needs compact continuity without dragging entire cold logs into active context.



The important ideas are:



\* let the model help define useful semantic buckets,

\* let the host/runtime control persistent state,

\* update memory through explicit reducer-style actions,

\* keep cold logs as audit history rather than active working context,

\* use deterministic triggers to inject relevant hot context,

\* treat context routing packets as navigation cards, not sacred schemas,

\* migrate carefully when changing models instead of pretending all models think the same way.



None of these patterns require one specific implementation.



A project could use JSON files, SQLite, a graph database, flat Markdown, a Rust host, a Python prototype, a game engine, a local GGUF stack, or something else entirely.



The point is not to prescribe machinery.



The point is to show a possible shape:



```text

model-native buckets

\+ host-governed state

\+ hot context cards

\+ context routing packets

\+ cold audit logs

```



Use the pattern where it helps.



Ignore or replace the parts that do not fit.



This is a map of useful design pressure, not a cage.



