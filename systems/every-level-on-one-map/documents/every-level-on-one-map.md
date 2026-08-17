<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Every Level on One Map

**Project Link:** [View Project](https://nextwork.ai/projects/ab8c608b-a7ec-46aa-b731-c7c6cb60fd82)

**Author:** Roy Piring Jr: Sr. Cloud Engineer | Architect  
**Email:** rpiringhawaii@gmail.com

---

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/ab8c608b-a7ec-46aa-b731-c7c6cb60fd82_95pp25v5)

## The Vision: Building a Queryable Map of Neuroscience

### Commitment before code

I built a typed, queryable neuroscience concept graph from a YAML source. The graph connects concepts across several scale layers and represents their relationships as structured data. I used this design to study how neuroscience ideas connect instead of treating each term as an isolated definition.

The graph supports questions such as, "What is the shortest path from an ion channel to spatial navigation?" Answering that question requires a traceable route across concepts that exist at different levels. A reading list can describe those concepts, but it cannot calculate the path between them. By committing to a typed graph before writing the code, I established that each node and relationship needed a defined role that the system could query and test.

## Laying the Foundation: Environment, Repository, and Project Board

### What this step accomplishes

I set up the schema header and defined the approved edge vocabulary for neuroscience_graph.yaml. This established the format used to represent neuroscience concepts, their types, and the relationships connecting them. The YAML file became the source that the rest of the build could load, test, and query.

Defining the structure first prevented later code from deciding what the data meant while it was being processed. The loader could expect a known format, and the integrity checker could reject entries that did not follow it. This foundation also kept the graph readable outside Python because the concepts and connections remained visible in YAML. The result was one controlled source that supported graph construction, automated checks, queries, visualization, and study exports.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/ab8c608b-a7ec-46aa-b731-c7c6cb60fd82_104gbfbk)

### Organizing work with Linear tickets

I divided the work into three Linear tickets in the Computational Neuroscience team under the Neuroscience Concept Graph project. COM-1 "Define schema and populate YAML" covered topology.md and neuroscience_graph.yaml. COM-2 "Build loader and integrity checker" covered loader.py and checks.py. COM-3 "Write query pack with expected answers" covered queries.py and test_graph.py.

Each ticket mapped to its own branch because the file sets were separate and the work followed a dependency order. The schema had to exist before the loader could parse it, and the loader had to work before queries could traverse the graph. Separate branches also produced three reviewable pull requests. A loader failure could not block the schema work, and each change remained a clean, revertable unit in git history.

## Designing the Schema: Six Node Types and a Closed Edge Vocabulary

### What this step accomplishes

I defined the metadata schema and the closed relationship vocabulary for the YAML source. These rules gave the neuroscience graph a fixed structure that the loader, queries, and automated tests could interpret consistently. The schema controlled which node and edge types the graph could accept.

This step mattered because a graph can be connected while still being difficult to query. If similar concepts use different type names or relationships, a query can miss valid entries without producing an error. A closed vocabulary turns that inconsistency into a failed check. It also gives each relationship a specific meaning across the full graph. The result was a typed source where every accepted node and edge followed the same contract before NetworkX loaded it into memory.

### The typed graph schema

The schema contains six node types: marr_level (3), scale_layer (4), subfield (6), landmark (42), method (8), and model (6). The first three form the structural spines of the graph. The final three contain the scientific content attached to those structures.

The graph also uses eight edge types: is-at-level, is-at-scale, belongs-to, underlies, explains, measured-by, inferred-from, and reproduced-by. Each relationship describes a defined connection that queries can recognize across the data.

The vocabulary is closed, so an edge using any other relationship fails the integrity check. This prevents a spelling variation or improvised label from silently becoming a new edge type. Adding another relationship requires a deliberate schema change rather than an untracked change during data entry.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/ab8c608b-a7ec-46aa-b731-c7c6cb60fd82_nbo1nj10)

### The sharpest distinction: measured-by vs inferred-from

A measured-by edge states that an instrument directly records the quantity. For example, spike_times measured-by patch_clamp means the recording contains the spike itself without an interpretive step between the signal and the result. The graph contains exactly 3 of these relationships.

An inferred-from edge states that the instrument does not directly record the final quantity. Each of the 5 edges names an intermediate_signal, such as BOLD, fluorescence, or scalp potential, and a model, such as HRF convolution, deconvolution, or spike sorting, that reconstructs spikes from that signal.

This distinction affects downstream trust. If the inference model is wrong, later claims based on those spike times inherit the error. That is why extracellular_probe_recording remains on the inferred side even though it is a standard method for obtaining spike times.

## Proving the Graph Sound: Loader and Integrity Checker

### What this step accomplishes

I built a Python loader that reads the static YAML source and transforms it into a queryable NetworkX graph. The loader creates typed nodes, directed edges, and attributes that the rest of the code can inspect. This kept the graph definitions separate from the Python functions that operate on them.

I also created integrity tests that compare the loaded graph with the schema rules. The checks verify that nodes and edges use the required definitions and that the overall structure remains consistent. This matters because valid YAML only proves that the file can be parsed. It does not prove that the relationships use the approved vocabulary or that the graph forms the intended structure. The checker provides repeatable evidence that the data meets those declared conditions before queries run.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/ab8c608b-a7ec-46aa-b731-c7c6cb60fd82_br87dvre)

### Structural errors caught and fixed

The first integrity run returned 3 passed, and I changed no edges in response. Claiming that the run found and fixed an error would describe work that did not happen. I had already tested the three invariants through ad-hoc validation while connecting the graph in the earlier step.

I created belongs-to as 56 edges and is-at-scale as 50 edges. These were near-total coverage relationships, so every landmark, method, and model received one of each by construction. That design made orphan nodes difficult to create during the initial build.

I still tested whether the checks could fail. Adding a floating_term node, a feels-related-to edge, and a two-component split caused all three checks to fire and identify the correct node or edge. The green run therefore reflected a clean graph rather than tests that could never detect a failure.

## Querying the Graph: Expected Answers Written Before Running

### What this step accomplishes

I wrote query functions that inspect the typed relationships in the neuroscience graph. The functions were designed to return results that matched expected answers written before execution. This kept the first output from becoming the answer simply because the code produced it.

The queries checked conceptual rules encoded in the graph, including the boundary between direct measurement and inference. Expected-answer tests made those rules repeatable and exposed any result that was missing, unexpected, or attached to the wrong relationship type. They could prove whether the implementation matched the claims recorded in the data. They could not independently prove that every claim was scientifically correct because the scientific judgment still came from the graph's source definitions.

### What the absence-list test proves

The absence-list test proves a limited negative claim about this graph. None of the five listed methods appears on a measured-by edge. That result shows that the recorded-versus-inferred boundary declared in the meta block still holds after 165 hand-wired edges. The five parametrized cases also identify the exact offender if a method crosses that boundary later.

The test does not prove that the classification is correct across neuroscience. It is a regression check on this inventory. It would still pass if patch_clamp were placed on the absence list and fmri were listed among direct recorders.

Its value is that it makes the declared judgment hard to break silently. The scientific position lives in meta.absence_list, while the test detects any edge that no longer matches it, including a return to the claim that "fMRI shows the brain thinking."

## Exporting, Running Cold, and Shipping Documentation

### What this step accomplishes

I exported the neuroscience graph as a browsable HTML visualization and created a 20-card flashcard deck from its relationships. The HTML view made the graph easier to explore visually, while the cards turned its edges into retrieval practice. Both outputs came from the same structured source rather than separate hand-written materials.

I also prepared a cold-run assessment to test whether I could recall the relationships without using the map. This separated recognition from retrieval. Seeing a connection in a graph can feel familiar without proving that I can explain it from memory. The combined workflow gave me three ways to inspect the same system: querying it through code, navigating it visually, and answering questions without the graph open. The documentation recorded how those outputs were produced and what each one could prove.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/ab8c608b-a7ec-46aa-b731-c7c6cb60fd82_bif6bj22)

### Tracing misses back to structural errors

The cold run has not happened yet. I built and smoke-tested cold_run.py, but I have not taken the assessment. There are no missed answers to analyze, and naming any would invent results that do not exist.

The deck contains 20 cards drawn from 165 edges. belongs-to accounts for 56 edges, and is-at-scale accounts for 50. Those cards can often be answered through category logic, so a high score on them would provide less evidence of detailed recall.

The more diagnostic cards come from inferred-from and underlies. inferred-from requires the exact intermediate_signal on the back of the card. underlies covers 19 content edges added late to fix the unlinked-landmark failure. Those edges are the newest and least rehearsed part of the graph, so misses there would identify relationships that need closer review.

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/ab8c608b-a7ec-46aa-b731-c7c6cb60fd82_voli51of)

## Extending the Graph: Thickening a Branch and Writing the Teach-Back

![Image](https://nextwork.ai/refreshed_maroon_timid_jujube/uploads/ab8c608b-a7ec-46aa-b731-c7c6cb60fd82_p920znmg)

### New nodes added and lessons from the teach-back

I thickened the plasticity landmark branch with 10 new landmarks: STDP, homeostatic scaling, metaplasticity, LTD, silent synapses, synaptic tagging and capture, AMPA trafficking, NMDA coincidence detection, the BCM sliding threshold, and spine structural plasticity. I also added 35 edges. The graph grew from 69/165 to 79/200, with all 13 tests still green.

The teach-back required the most care around extracellular_probe_recording. Technical shorthand says that spike times are inferred through sorting, but plain language can make that sound like the standard method is unreliable. I clarified that it is both the field's workhorse and an inferential instrument.

I applied the same care to "What a Count Means." Saying that "3 instruments record spike times" describes this hand-built file, not neuroscience as a whole.

## Reflections: Tools, Concepts, and Time

### Key tools and concepts

I used Python with NetworkX, Pyvis, PyYAML, and pytest. NetworkX represented and queried the directed graph, Pyvis produced the browsable HTML view, PyYAML loaded the source data, and pytest checked the declared structural rules and expected answers. I also used GitHub for version control, Linear to organize the work, and Anki for spaced-repetition retrieval.

The main concepts were typed directed graphs, closed relationship vocabularies, automated integrity checks, and cold-run testing. Together, they showed how a structured source can support several outputs without changing its underlying claims. The closed vocabulary kept queries consistent, while the checks verified that later edits still followed the schema. The cold-run method added a separate test of whether I understood the structure without looking at it.

### Time and challenge

This build took approximately 70 minutes. The hardest part was debugging the integrity tests after thickening the graph. The new sub-domain nodes initially became orphans because I had not connected them to the existing hierarchy with relationships from the required edge vocabulary.

Fixing that issue required more than adding arbitrary links. Each new node needed a valid place in the typed structure so that the graph remained connected without weakening the schema. The teach-back created a second challenge by forcing me to explain measured-by and inferred-from in plain language. The distinction was more nuanced than it appeared during the first structural pass. Writing it out showed that direct recording and scientific reliability are different questions, especially for extracellular_probe_recording.

### Looking ahead

I completed this build today to learn how to represent a complex scientific domain as a structured, queryable knowledge graph. The work gave me direct practice with automated integrity checks, closed relationship vocabularies, graph traversal, and interactive visualization. It also required me to explain what the graph's counts and classifications could prove without treating them as universal facts.

The next step is to apply the same graph-modeling approach to [insert another domain or project, e.g., mapping software architecture or financial systems]. That extension would test whether the schema and workflow can transfer beyond neuroscience. The goal is to map core concepts, verify their relationships through defined rules, and make the resulting structure easier to inspect through queries and visual paths.

---

*Built with [NextWork](https://nextwork.ai) - [View this project](https://nextwork.ai/projects/ab8c608b-a7ec-46aa-b731-c7c6cb60fd82)*
