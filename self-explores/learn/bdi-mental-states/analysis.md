---
date: 2026-03-26
type: learning
task: ceskills-i63
tags: [bdi-mental-states, cognitive-architecture, RDF, SPARQL, ontology, T2B2T, LAG, DOLCE, neuro-symbolic]
---

# bdi-mental-states - Deep Analysis

## Score

| Rubric | Score | Justification |
|--------|-------|---------------|
| Completeness | 5/5 | Exhaustive coverage: BDI ontology core classes, mental processes, cognitive chain pattern, T2B2T paradigm, temporal dimensions, compositional entities, justification/explainability, integration patterns (LAG, SEMAS, JADE/JADEX, FIPA ACL), anti-patterns, competency questions. 4 reference files totaling ~1000 lines. |
| Usability | 2/5 | Highly specialized domain (RDF/SPARQL/ontology engineering). Requires deep knowledge of semantic web technologies, DOLCE foundational ontology, and BDI cognitive science. Not directly applicable without significant domain expertise. No scripts directory -- only reference code. |
| Modularity | 4/5 | Clean separation: SKILL.md for patterns/guidelines, bdi-ontology-core.md for class hierarchy and restrictions, rdf-examples.md for complete Turtle examples, sparql-competency.md for validation queries, framework-integration.md for SEMAS/JADE/LAG code. Each reference is self-contained. |
| Safety | 3/5 | Anti-patterns section helps prevent modeling errors. Validation queries (V1-V5) detect constraint violations. Inconsistency detection example (conflicting location beliefs). But: no guidance on handling adversarial belief injection, belief revision under conflicting evidence, or safety constraints on intention formation in autonomous agents. |

## Uu diem va Nhuoc diem

**Uu diem:**
- Strongest theoretical grounding in the collection: Bratman (1987), Rao & Georgeff (1995), Zuppiroli et al. (2025)
- T2B2T paradigm (Triples-to-Beliefs-to-Triples) provides bidirectional flow between external knowledge and internal states -- powerful for neuro-symbolic AI
- Complete cognitive chain: WorldState -> Perception -> Belief -> Desire -> Intention -> Plan -> Task -> Action -> WorldState -- full cycle
- Compositional mental entities with hasPart relations enable selective updates without reconstructing entire beliefs
- Temporal validity intervals on all mental states enable diachronic reasoning
- Justification tracking provides explainability: every intention traces back through desires to beliefs to evidence
- DOLCE alignment ensures interoperability with established foundational ontologies
- 27 competency questions + 5 validation queries provide comprehensive implementation verification
- Logic Augmented Generation (LAG) pattern bridges gap between LLMs and formal reasoning

**Nhuoc diem:**
- Most specialized skill in the collection -- requires RDF/SPARQL/ontology expertise that few practitioners have
- No scripts/ directory -- only code appears in reference files, harder to discover and run
- Steep learning curve: understanding requires cognitive science + semantic web + ontology engineering
- Limited immediate practical applications for typical AI/LLM projects
- JADE/JADEX framework is Java-based and aging -- modern alternatives exist
- Missing: belief revision algorithms (AGM theory), practical performance considerations for large-scale belief stores, comparison with simpler state management approaches
- No benchmarks or empirical evidence for BDI vs simpler agent architectures in LLM contexts

## References Insights

**Insight 1 - DOLCE Ultra Lite Alignment Table (from bdi-ontology-core.md):**
The bdi-ontology-core reference provides a complete alignment between BDI classes and DOLCE Ultra Lite (DUL) superclasses that is not in SKILL.md. Key mappings: Belief -> dul:InformationObject (information-bearing entity), Desire -> dul:Description (describes desired state), WorldState -> dul:Situation (configuration of entities), MentalProcess -> dul:Event (temporally extended occurrence). This alignment is critical because it positions BDI ontology within the established upper ontology ecosystem, enabling interoperability with any system that uses DOLCE. SKILL.md mentions DOLCE alignment in guideline #2 but doesn't provide the actual mapping table.

**Insight 2 - Ontological Restrictions as Formal Constraints (from bdi-ontology-core.md):**
The reference defines OWL restrictions that enforce structural integrity: `Belief rdfs:subClassOf [owl:Restriction; owl:onProperty bdi:refersTo; owl:someValuesFrom bdi:WorldState]` means every Belief MUST reference at least one WorldState. `Intention rdfs:subClassOf [owl:Restriction; owl:onProperty bdi:fulfils; owl:cardinality 1]` means every Intention fulfills EXACTLY one Desire. `BeliefProcess rdfs:subClassOf [owl:Restriction; owl:onProperty bdi:generates; owl:allValuesFrom bdi:Belief]` means BeliefProcess can ONLY generate Beliefs. These formal restrictions enable automated consistency checking via OWL reasoners -- a powerful validation mechanism not discussed in SKILL.md beyond guideline #9's brief mention.

**Insight 3 - FIPA ACL Communication Protocol (from framework-integration.md):**
The framework-integration reference provides a complete `BDICommunicator` class for inter-agent belief sharing using FIPA Agent Communication Language. Three communication patterns: `share_belief` (INFORM performative with Turtle content), `request_belief_confirmation` (QUERY-IF with SPARQL), and `propose_intention` (PROPOSE for coordinated intentions). This multi-agent communication protocol is only briefly mentioned in SKILL.md under "Multi-Agent Communication: Integrate with FIPA ACL for cross-platform belief sharing" without implementation details. The key insight is that beliefs are shared as serialized Turtle RDF, enabling heterogeneous agent platforms to exchange mental states.

**Insight 4 - Location Inconsistency Detection with Temporal Overlap (from framework-integration.md):**
The `detect_location_inconsistency` function in framework-integration.md uses a SPARQL query that finds conflicting location beliefs with overlapping temporal intervals. It checks for agents that simultaneously hold beliefs about being in different locations at the same time (`FILTER(?start1 < ?end2 && ?start2 < ?end1)`). This concrete inconsistency detection pattern shows how temporal validity intervals enable automated conflict resolution -- a practical application of the temporal dimensions discussed abstractly in SKILL.md.

**Insight 5 - Complete BDI Triple Store with Cognitive Chain Tracing (from framework-integration.md):**
The `BDIMentalStateStore` class wraps a SPARQL triple store and provides `get_cognitive_chain(intention_uri)` that traces: Intention -> fulfilled Desire -> motivating Belief -> referenced WorldState -> specified Plan in a single SPARQL query. This end-to-end cognitive chain retrieval demonstrates the practical power of the ontological approach: any intention's complete reasoning chain is queryable. SKILL.md shows individual chain links but doesn't provide this complete chain tracing implementation.

## Cross-skill Comparison

**bdi-mental-states vs multi-agent-patterns:**
multi-agent-patterns provides coordination architecture (supervisor, pipeline, debate) while bdi-mental-states provides the cognitive model FOR each agent. BDI gives each agent in a multi-agent system a formal internal state (beliefs about the world, desires as goals, intentions as commitments). The FIPA ACL integration in bdi-mental-states is specifically designed for multi-agent communication. multi-agent-patterns could leverage BDI for more sophisticated agent reasoning: instead of simple tool-call agents, agents with explicit belief tracking, desire formation, and intention commitment. However, the complexity cost may outweigh benefits for most LLM-based multi-agent systems.

**bdi-mental-states uniqueness in the collection:**
This is the only skill that provides a formal cognitive architecture. All other skills treat agents as black boxes or simple input-output systems. BDI provides INTERNAL structure for agent reasoning. The T2B2T paradigm is unique: no other skill addresses bidirectional transformation between external data formats and internal agent states. The explainability through justification chains is the strongest explainability pattern in the collection.

**Contrast with simpler approaches:**
Most skills in the collection use pragmatic, lightweight state management. bdi-mental-states is the heavyweight theoretical approach. For most practical LLM agent work, a simplified version (just tracking beliefs as a list, desires as goals, intentions as current plan) would capture 80% of the value with 20% of the complexity. The full ontological approach is most valuable when formal verification, cross-platform interoperability, or research-grade explainability is required.

## Tich hop

- **OMC agent reasoning**: Lightweight BDI could formalize OMC agent states -- beliefs (what agent knows about codebase), desires (task goals), intentions (current execution plan). Would improve explainability of agent decisions
- **Explainability**: "Why did agent do X?" -> trace justification chain from action through intention through desire to belief to evidence. Directly applicable to OMC's verifier output
- **Multi-agent coordination**: BDI mental state sharing via FIPA ACL pattern could enhance OMC's team pipeline -- agents share beliefs about codebase state rather than just passing messages
- **Research applications**: Full BDI ontology valuable for cognitive AI research, neuro-symbolic AI pipelines, and formal agent verification
- **LAG pattern**: Logic Augmented Generation is applicable whenever LLM outputs need formal constraint validation -- could constrain OMC agent tool usage through ontological restrictions

## Ghi chu ky thuat

- Version 1.0.0 (no explicit date in SKILL.md). 295 lines in SKILL.md.
- Most specialized skill in the entire collection. Only skill without a scripts/ directory.
- 4 reference files: bdi-ontology-core.md (206 lines -- class hierarchy, properties, restrictions, DOLCE alignment), rdf-examples.md (316 lines -- complete cognitive workflow, multi-agent coordination, conflict resolution, T2B2T payment example), sparql-competency.md (420 lines -- 27 queries + 5 validation queries), framework-integration.md (582 lines -- SEMAS, LAG, JADE/JADEX, triple store, FIPA ACL).
- Total reference material: ~1524 lines -- largest reference corpus relative to SKILL.md size.
- Primary sources: Zuppiroli et al. 2025, Rao & Georgeff 1995, Bratman 1987.
- Key namespace: `bdi: <https://w3id.org/fossr/ontology/bdi/>`.
- Reused Ontology Design Patterns: EventCore, Situation, TimeIndexedSituation, BasicPlan, Provenance.
- The rdf-examples.md commuter scenario (traffic -> alternate route) is the most complete end-to-end BDI example covering all 7 phases.
- No dependency on other skills in the collection, but complements multi-agent-patterns.
