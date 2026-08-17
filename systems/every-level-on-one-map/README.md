# Every Level on One Map

> Part of the [Computational Neuroscience](../../README.md) mastery track · *A route to command, one build at a time.*

## Overview

Neuroscience spans levels that rarely sit on the same page: ion channels, circuits, systems, behaviour. This build represents them as one typed, queryable concept graph loaded from YAML, so a question such as what the shortest path is from an ion channel to spatial navigation has a traceable answer instead of a reading recommendation. A reading list can describe those concepts; it cannot calculate the route between them.
The schema is fixed before any code interprets the data. Six node types carry the structure and the content: marr_level (3), scale_layer (4), and subfield (6) form the spines, while landmark (42), method (8), and model (6) hang the science on them. Eight edge types are allowed and no more: is-at-level, is-at-scale, belongs-to, underlies, explains, measured-by, inferred-from, and reproduced-by. The vocabulary is closed, so a spelling variation fails the integrity check rather than silently becoming a ninth edge type that no query knows about.
Work was split into three Linear tickets on dependency order, each on its own branch and pull request, because the schema had to exist before the loader could parse it and the loader had to work before queries could traverse anything. Expected answers were written before the queries ran, the graph was exported and run cold, and the build closed by thickening one branch and writing the teach-back that has to defend the structure rather than describe it.

## Architecture

```mermaid
---
title: Every Level on One Map
---
flowchart TD
%%{init: {"theme":"base","themeVariables": {"primaryColor":"#1B4332","primaryTextColor":"#F4D03F","primaryBorderColor":"#F4D03F","secondaryColor":"#264653","tertiaryColor":"#2F5233","lineColor":"#F4D03F","fontFamily":"ui-monospace, SFMono-Regular, Menlo, Consolas, monospace","fontSize":"13px"}}}%%
    classDef datastore fill:#264653,stroke:#F4D03F,stroke-width:2px,color:#FFFFFF
    classDef service fill:#1B4332,stroke:#F4D03F,stroke-width:2px,color:#F4D03F
    classDef event fill:#7B42BC,stroke:#F4D03F,stroke-width:2px,color:#FFFFFF
    classDef io fill:#0d1117,stroke:#F4D03F,stroke-width:1.5px,color:#F4D03F,font-style:italic

    Learner[/Learner mapping a field across levels/]
    Answer[/A traceable path, not a reading recommendation/]

    subgraph Delivery["Delivery trail"]
        COM1[(COM-1: schema and YAML)]
        COM2[(COM-2: loader and integrity checker)]
        COM3[(COM-3: query pack with expected answers)]
        Order{{Dependency order: schema, then loader, then queries}}
    end

    subgraph Schema["Typed schema"]
        Spines[(Spines: marr_level 3, scale_layer 4, subfield 6)]
        Content[(Content: landmark 42, method 8, model 6)]
        Edges[(Eight edge types, closed)]
        Rejects{{A ninth relation fails the check, never joins silently}}
    end

    subgraph Build["Load and prove"]
        Yaml[(neuroscience_graph.yaml)]
        Loader(loader.py)
        Graph[(Typed graph in NetworkX)]
        Checker(checks.py integrity checker)
    end

    subgraph Query["Query pack"]
        Expected[(Expected answers written first)]
        Path(Shortest path: ion channel to spatial navigation)
        Verdict{{Query output matched against the written answer}}
    end

    subgraph Close["Extend and close"]
        Thicken(Thicken one branch)
        ColdRun{{Exported and run cold}}
        TeachBack[(Teach-back defending the structure)]
    end

    Learner -- "plans in" --> COM1
    COM1 -- "unblocks" --> COM2
    COM2 -- "unblocks" --> COM3
    COM1 -- "sequenced by" --> Order
    COM1 -- "declares" --> Spines
    COM1 -- "declares" --> Content
    COM1 -- "declares" --> Edges
    Edges -- "closed set means" --> Rejects
    Spines -- "structures" --> Yaml
    Content -- "populates" --> Yaml
    Yaml -- "parsed by" --> Loader
    Loader -- "produces" --> Graph
    Checker -- "rejects entries breaking" --> Rejects
    Graph -- "must pass" --> Checker
    Expected -- "written ahead of" --> Path
    Graph -- "traversed by" --> Path
    Path -- "compared against expected" --> Verdict
    Graph -- "grown by" --> Thicken
    Graph -- "exported and re-run" --> ColdRun
    Thicken -- "explained in" --> TeachBack
    Verdict -- "gives" --> Answer
    ColdRun -- "proves reproducibility for" --> Answer
    TeachBack -- "defends the map behind" --> Answer

    class COM1,COM2,COM3,Spines,Content,Edges,Yaml,Graph,Expected,TeachBack datastore
    class Loader,Checker,Path,Thicken service
    class Order,Rejects,Verdict,ColdRun event
    class Learner,Answer io
```

The diagram shows the topology and data flow of the system as built. The full architectural narrative, with screenshots and prose, lives in [`documents/every-level-on-one-map.md`](./documents/every-level-on-one-map.md).

## Implementation

This system is built across **7 phases**:

1. The Vision: Building a Queryable Map of Neuroscience
2. Laying the Foundation: Environment, Repository, and Project Board
3. Designing the Schema: Six Node Types and a Closed Edge Vocabulary
4. Proving the Graph Sound: Loader and Integrity Checker
5. Querying the Graph: Expected Answers Written Before Running
6. Exporting, Running Cold, and Shipping Documentation
7. Extending the Graph: Thickening a Branch and Writing the Teach-Back

For the full walkthrough with screenshots and step-by-step content, see [`documents/every-level-on-one-map.md`](./documents/every-level-on-one-map.md).

## Validation

Each build phase below is documented in [`documents/every-level-on-one-map.md`](./documents/every-level-on-one-map.md), with screenshots, configuration, and notes as captured during the build:

- ✅ The Vision: Building a Queryable Map of Neuroscience
- ✅ Laying the Foundation: Environment, Repository, and Project Board
- ✅ Designing the Schema: Six Node Types and a Closed Edge Vocabulary
- ✅ Proving the Graph Sound: Loader and Integrity Checker
- ✅ Querying the Graph: Expected Answers Written Before Running
- ✅ Exporting, Running Cold, and Shipping Documentation
- ✅ Extending the Graph: Thickening a Branch and Writing the Teach-Back
