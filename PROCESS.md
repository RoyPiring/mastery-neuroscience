# How these builds are made

The README is the summary. This is the full build methodology and the honest framing behind this track: the do-first order, the field-scaled safety layer in full, the binding contract every build carries, and the delivery, audit, and documentation rails each build ships through.

## Start here (the do-first order)

**Catalog: `ideas/mastery-neuroscience-100/mastery-neuroscience-100.md`**

Do-first order lives in `ideas/mastery-neuroscience-100/mastery-neuroscience-100-RANKING.md`. Start at rank 1 and go in order.

| Rank | Start with |
|------|------------|
| 1 | The Neuroscience Concept Map |
| 2 | Marr's Three Levels of Analysis |
| 3 | The Neuron Doctrine & Brain Organization |
| 4 | What Computational Neuroscience Is |
| 5 | The Math & Code On-Ramp |

Then run intake triage (`pipeline/07_pipeline_intake_triage.md`, floor 70) and ingest it into ONE standalone project file from `templates/PROJECT_SCAFFOLD.md`. `python scripts/verify_project.py <file> --paste` must return GO before you paste, and GO is SHAPE, not quality: the four audits are the gate.


## The honest route

A PhD in neuroscience is an ACADEMIC RESEARCH degree. This track builds subject-matter command and a portfolio by doing, not a degree.


## Safety layer (binding)

- **Not medical advice.** Nothing here diagnoses, treats, or interprets a clinical finding for any person.
- **Human and animal data carry ethics duties**: IRB and IACUC guidelines and each dataset's access terms govern use.
- **Know what the data actually is** before claiming from it (for example, IBL is MOUSE data, not human).
- **Wet-lab and animal work is supervised-only**, under the hosting lab's oversight.


## The binding contract (what every project here must carry)

- **Point of view.** PoV-M, the learner-principal. Every payload is written as someone building command of neuroscience, weighted to computational neuroscience by doing, never as a textbook narrator. The persona is binding on all payloads (rule 1).
- **Build constraints.** 90 minutes of build time, AI-native and agent-built (rule 19), free-tier first, and no metered pay-per-token API as the only path.
- **The signature artifact, three parts.** The validated learning system, its CONCRETE self-check (a reproduced value, a named benchmark, or a rubric threshold, never "it looks right"), and a TEACH-BACK that proves it was understood rather than copied.
- **Every system is an end-to-end BUILD.** Research serves the build; research alone is not a system. Input, process, output, and a self-check.


## Onboarding a project here

1. Pick the next system in do-first order from this field's catalog and its `-RANKING.md`, then run intake triage (`pipeline/07_pipeline_intake_triage.md`).
2. Ingest it into ONE standalone file at `projects/NN_<shorthand>.md` from `templates/PROJECT_SCAFFOLD.md`, carrying the design-first pre-artifacts, the delivery rails, and the documentation rails.
3. `python scripts/verify_project.py <file> --paste` must return GO. GO is SHAPE, not quality.
4. Run the four audits: quality gate (99), Codex cross-eval, currency, and simple-language.
5. `python scripts/check_workspace.py` must return CLEAN, then commit and update `governance/MASTERY_LEDGER.md`.


## Delivery rails (rule 27)

Every build ships through real engineering process on tools already in use, so each project is
in-the-trenches experience rather than a spec.

- **Code host:** a private GitHub repo per build, created BY THE BUILD (not by hand at onboarding).
- **Ticket tracker:** Linear, one project per build in the dedicated workspace. The cohort tier is
  the one exception and uses GitHub Projects, per rule 27's per-repo tracker pin.
- **The loop:** tickets planned as epics, stories and tasks, each worked on a branch and closed by
  the pull request that merges it, ending in a tagged release and plain-language repo docs.
- **Completion is not "it runs".** It is every pull request merged, every ticket closed, the
  tracker project Completed, the release tagged, and the documentation set present (rule 28).
  Nothing ships as an un-PR'd commit, an open ticket, or an undocumented repo.
- The per-repo signature GitHub features for this repo live in `governance/DELIVERY_RAILS.md`.


## The four audits (every project, before commit)

| Audit | Owner | Bar |
|-------|-------|-----|
| Quality gate | agent 03 | 99 or higher, hard, not convergence-eligible |
| Codex cross-eval | agent 05 | findings-clean; score advisory, 80 floor |
| Currency | agent 06 | 95, findings-gated |
| Simple-language | agent 07 | 95, findings-gated |

The convergence rule is the single source of truth on thresholds: the quality gate is a hard 99,
and the three cross-evals are findings-gated with the score advisory at an 80 floor, so a
findings-clean audit at or above 80 is a logged pass even below its target. **Nothing ships with
an open high-confidence finding.**

**GO is SHAPE, not quality.** `verify_project.py` checks that the payloads are clean enough to
paste. It is not an audit and it is not approval to ship. Record the AUDIT's score in
`quality_gate`, never the verifier's result: `check_workspace.py` check K fails a project that
records "structural/mechanical" or "verify_project.py --paste GO" as its quality gate, because
that reports a perfect audit that never ran.


## Documentation rails (rule 28)

Every project onboarded here ships a DOCUMENTATION SET as a build output, the same way it ships a private repo and a ticket board (rule 27). The set is profiled to this tier, because an infrastructure system, a client deliverable, and a learning artifact are not read by the same person for the same reason. The full standard is `governance/DOCUMENTATION_RAILS.md`.

**Format.** Markdown is canonical, and `docs/` ships in the SAME pull requests as the work it describes. HTML is an optional generated view only. This is a constraint rather than a taste: GitHub renders Markdown inside a private repo on the free plan, including Mermaid diagrams and relative links, and does NOT render a clicked `.html` file. GitHub Pages and Wikis are both unavailable on a private repo on the free plan (checked 2026-07-19). The reversal trigger is a public repo or a paid plan.

**The visual view (optional, free).** The GitHub Markdown view is flat: no sidebar, no search, no reading order. `python scripts/build_wiki.py <project-repo>` renders the same `docs/` tree into ONE self-contained `docs/wiki.html` you open by double-click: sidebar navigation, search, light and dark themes, rendered diagrams, and dead-link detection. Standard library only, no new tool and no subscription, works offline and from a downloaded zip. It is GENERATED, never hand-edited, and `--check` fails when it no longer matches the Markdown, so the visual view cannot drift away from the source. The day a repo goes public, the same file publishes to GitHub Pages unchanged.

**Universal, every project.** A README answering what this is, why it exists, and how to run it inside the first screen. A SOURCE MAP naming the files behind each section, so a stale page is a lookup rather than a memory test. A `reviewed:` date, with any point-in-time number labelled a snapshot. Plain language, no em dashes. And the one rule: no claim floats free, so every claim traces to a file, a command, a measured number, or a named source, and anything else is labelled `[ESTIMATE]`.

**This tier's pages (mastery profile).** Dominant modes are tutorial and explanation. The reader is the learner, later, and anyone they teach.

1. What I was trying to learn, and why: your own model of it, not a textbook recap.
2. The build: what it is and how to run it.
3. A WORKED EXAMPLE: one instance solved end to end, because for a self-directed learner a worked example beats jumping straight to practice.
4. The self-check: the concrete expected value, benchmark, or reference the result is matched against, how to reproduce it, and what a WRONG answer looks like. Explaining the incorrect case is what makes the check teach rather than merely pass.
5. Retrieval prompts: five to ten closed questions answerable without opening the repo, scheduled rather than optional, because learners reliably underrate retrieval practice and skip it.
6. The teach-back: the short explanation to a novice that proves the thing was understood rather than copied.
7. What I got wrong, and the open questions.
8. Sources, each with its correct host.

**Cut here.** Operations, deployment, runbooks, decision records, and license ceremony. A learning artifact documents understanding, not uptime.

**Where it lands in onboarding.** The project file carries a `### Documentation rails` pre-artifact naming the profile; R2 names the documentation set as a deliverable so the generator hears about it and the build does not quietly stop at "it runs"; and `scripts/verify_project.py` hard-checks both for projects stamped pipeline_version 0.18.0 or higher. Projects committed earlier are grandfathered and reported SKIP.
