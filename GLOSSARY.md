# Glossary (Repo Excerpt)

For the full glossary, see: https://github.com/instance001/Whatisthisgithub/blob/main/GLOSSARY.md

This file contains only the glossary entries for this repository. Mapping tag legends and global notes live in the full glossary.

## llm-defined-persistent-memory
| Term | Alternate term(s) | Alt map | External map | Relation to existing terminology | What it is | What it is not | Source |
| --- | --- | --- | --- | --- | --- | --- | --- |
| LLM-Defined Persistent Memory | model-native persistent memory adapter | = | ~ | Compact persistent memory architecture | Memory adapter where an LLM defines its own semantic memory buckets and a deterministic runtime stores and updates those buckets through governed reducers | Not infinite memory; not transcript replay; not fine-tuning | llm-defined-persistent-memory/readme.md |
| The model defines the drawers | drawers metaphor | ~ | ~ | Model-native bucket stance | Principle that the target model creates the semantic bucket map that future sessions will read back in compact form | Not a human-authored taxonomy imposed first; not one universal bucket map across models | llm-defined-persistent-memory/readme.md |
| Persistent memory slots | memory slots | ~ | ~ | Bucket-backed memory structure | Stored slots derived from a fixed semantic bucket map generated through Semantic Signal Alphabet | Not an unstructured note dump; not a raw vector index | llm-defined-persistent-memory/readme.md |
| Hot context cards | hot context | ~ | ~ | Triggered context injection | Specific entity or project cards injected only when relevant so the model gets compact targeted context instead of searching long logs | Not always-on full memory loading; not raw archive retrieval | llm-defined-persistent-memory/readme.md |
| Cold logs as audit history | cold logs | ~ | ~ | Archive-not-working-memory split | Cold logs remain available as audit history while active working memory is kept compact and routed | Not active prompt memory by default; not discarded history | llm-defined-persistent-memory/readme.md |
| Reducer-controlled updates | governed reducer actions | ~ | ~ | Controlled memory mutation rule | Memory state changes are performed through explicit reducers rather than unconstrained rewriting by the model | Not freeform self-editing memory; not unguided summary drift | llm-defined-persistent-memory/readme.md |
