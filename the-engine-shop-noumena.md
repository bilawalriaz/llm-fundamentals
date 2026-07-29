# The Engine Shop: AI, Software Engineering, and the End of Handcrafted Implementation

**Series by Noumena (noumena.com) — July 12, 2026**

A three-part essay series on what happens to software engineering when AI makes implementation abundant but leaves judgment, taste, and experience as scarce as ever.

---

## Table of Contents

1. [Central Thesis](#central-thesis)
2. [The Rocket Engine Metaphor](#the-rocket-engine-metaphor)
3. [The Missing Oracle Problem](#the-missing-oracle-problem)
4. [The Review Bottleneck](#the-review-bottleneck)
5. [Contract-First, Implementation-Disposable](#contract-first-implementation-disposable)
6. [The Return of Hard Contracts and Ceremony](#the-return-of-hard-contracts-and-ceremony)
7. [Containment: The Agent's World](#containment-the-agents-world)
8. [Preserving Every Explosion: Version Control for Agents](#preserving-every-explosion-version-control-for-agents)
9. [Branching the Search: Stacks, Bookmarks, and Candidate Trees](#branching-the-search-stacks-bookmarks-and-candidate-trees)
10. [Preserving the Saga, Not the Transcript](#preserving-the-saga-not-the-transcript)
11. [Scaling to a Fleet: The Work-Order System](#scaling-to-a-fleet-the-work-order-system)
12. [Assembly, Not Tournament: Recombination](#assembly-not-tournament-recombination)
13. [The N1 Problem Returns: Integration](#the-n1-problem-returns-integration)
14. [The Bitter Lesson Objection](#the-bitter-lesson-objection)
15. [What the Human Should Finally See](#what-the-human-should-finally-see)
16. [The Compiler Metaphor](#the-compiler-metaphor)
17. [The Artisan's New Role](#the-artisans-new-role)
18. [Reference Surfaces: The Three-Plate Method](#reference-surfaces-the-three-plate-method)
19. [Why Not "Factory"](#why-not-factory)
20. [Key Technologies Referenced](#key-technologies-referenced)
21. [Glossary of Concepts](#glossary-of-concepts)

---

## Central Thesis

The claim that "AI makes software cheap" is wrong in most of the ways that matter. **AI makes implementation cheap** — the act of turning a decision into working code. Everything around that act has kept its old price. Someone still has to decide what the system is supposed to do, define how the pieces fit together, draw the security boundaries, and judge whether the thing that came back actually works. Judgment, taste, and experience still cost what they always cost.

This mismatch is the central economic fact of AI-assisted development. Agents can produce code far faster than humans can meaningfully evaluate it. Most of what they produce will be plausible but mediocre. Some will be subtly wrong. Some will solve the local problem while quietly damaging the system around it. And buried in the flood, every so often, will be an implementation better than the one you would have written yourself.

The potential is real, but only if we stop treating every generated artifact as a finished piece of work awaiting human approval, one careful reading at a time.

**The organizing principle: contracts should be durable, implementations disposable.** The durable artifacts are the ones that define what the system means. Everything inside those boundaries can be generated, rejected, rewritten, and replaced without ceremony.

---

## The Rocket Engine Metaphor

### Apollo vs. The Engine Shop

Two approaches to building confidence in complex machines:

**Apollo (NASA):** Converted resources into preflight confidence. Vast apparatus of modeling, review, documentation, component qualification, static firing, integration testing, mission rehearsal. Built facilities specifically to fire Saturn V stages. Every launch surrounded by an institution designed to make failure unlikely.

**The Engine Shop (Soviet N1 program):** Resource-constrained. Leaned on direct contact with hardware — build, fire, inspect, modify, fire again. Less simulation, more ignition. Used failure as an input and a guide. Boris Chertok's history records that the N1's NK-15 engines achieved substantially higher chamber pressure and specific impulse than the Saturn V's F-1.

### Where the Engine Shop Runs Out

The N1's first stage (Block A) clustered thirty engines into a single system of staggering complexity. That complete stage was never static-fired on the ground. Individual engine firings could not reveal the interactions among engines, plumbing, structure, wiring, and vehicle dynamics. All four N1 launches failed, and the program died.

**The lesson:** Component success does not compose automatically into a working system. The engine shop made failure frequent, cheap, and informative — but could not test the integrated system cheaply before flight.

### What We Should Steal

- **From the engine shop:** Willingness to generate, fire, break, discard, and try again. Many attempts, destructive pressure, direct observation, no sentimentality about designs that don't survive.
- **From Apollo:** Seriousness about contracts, integration, and the places where failure is unacceptable.
- **The inversion:** Generation should be liberal because selection must be severe. Put expensive certainty where meaning and integration live, and cheap destructive iteration everywhere else.

Do not bolt a slop cannon onto Apollo's review process and expect the humans to keep up.

---

## The Missing Oracle Problem

The relocation of discipline from preflight analysis into test environments, instrumentation, and speed of iteration works for rockets because rockets live in a universe with an external judge: physics. A combustion chamber either holds pressure or it doesn't. The universe is the oracle, and it works for free.

**Software has no equivalent.** A program can compile and be useless. A service can satisfy its API contract while implementing the wrong business rule. A test suite can be green because it encodes the same mistaken assumption as the code it tests. Correctness in software depends on intent, and intent often exists nowhere but in the heads of the people building the system.

### The Tempting (But Wrong) Response

Mandate exhaustive tests, perfect simulations, formal specifications, a complete executable model. But a complete oracle for a complex system is usually harder and more expensive to build than the system itself. If you could fully specify every valid behavior in advance, you would have already done most of the engineering.

### The Practical Alternative

Use the **hard judges already lying around**. Software has no single physics, but it is full of local analogs:

| Mechanism | Role |
|-----------|------|
| Compiler, type checker | **Judge** — rejects classes of invalid implementations |
| Database constraint, transaction | **Judge** — rejects invalid state mechanically |
| Interface definition, state machine | **Judge** — rejects illegal transitions |
| Sandbox, capability boundary, resource budget | **Blast wall** — limits damage from what judges miss |

**Judges** make bad work easier to kill. **Blast walls** make surviving mistakes cheap enough to learn from. The goal is not one perfect oracle. The goal is to arrange the system so that generated code collides with as many hard, cheap, existing oracles as possible on its way in.

---

## The Review Bottleneck

When AI drops into the conventional workflow: a test fails, an agent produces a patch, a human reviews it. If the agent produces patches ten or a hundred times faster than the human can read them, **the review queue becomes the throughput limit of the entire system**. The bottleneck moves from writing code to reading (and ideally understanding) it.

Reading is the worst place for the bottleneck. AI output is hard to review precisely because it is plausible. It looks clean, uses familiar patterns, and often passes obvious tests. The reviewer must reconstruct the agent's assumptions, hunt for omitted edge cases, notice quietly weakened constraints, and verify the agent solved the actual problem.

Flooding a human with plausible code doesn't produce proportional productivity — it produces **review fatigue, apathy (LGTM, ship it), and eventually a reviewer who approves 800 plausible lines and meets the bug in production.**

You cannot fix this by throttling agents to human reading speed (forfeits the advantage) or waving code through unread (dangerous). The only remaining option: **arrange the system so that most bad implementations die before any human sees them**.

This is the engine shop principle: generation can be liberal because selection is severe. Exhaustive line-by-line review of every candidate can no longer remain the default proof that software is safe.

**We will not read every line. We must remain capable of reading any line.**

### Severe Selection Requires Judgeable Codebases

The design of the codebase itself matters profoundly. A loose, dynamically shaped system gives an agent enormous freedom to be almost correct: invent a field name, ignore an invalid state, widen an input, skip a permission check. The failure may not surface until three services later.

A tightly specified system drags failures back toward the moment of creation: the type checker rejects the invalid combination, the RPC compiler rejects the incompatible interface, the database rejects the impossible row, the capability system rejects the unauthorized call. The implementation is forced to confess its misunderstanding early, to a machine.

**The AI-native codebase should be designed not merely to help machines write code, but to help machines prove code wrong.**

---

## Contract-First, Implementation-Disposable

### The Inversion of the Old Tradeoffs

For the last 20-30 years, mainstream software development optimized for human ergonomics: dynamic languages, schemaless databases, informal JSON APIs. Rules that might have lived in schemas or generated clients migrated into application code because humans were writing everything and boilerplate consumed human hours.

**AI inverts the tradeoff.** An agent does not get tired of writing structs. It does not resent schema migrations, exhaustive enum matches, generated clients, or validation layers. AI is cheap at ceremony. What AI is expensive at is ambiguity.

An underspecified request gives an agent room to make decisions that are locally reasonable and globally wrong. An underspecified interface lets two agents independently invent incompatible interpretations. A schemaless data model postpones their disagreement until runtime, when it is most expensive to discover.

### The Durable/Disposable Split

**Durable artifacts** (define what the system means): types, schemas, service definitions, legal state transitions, invariants, permission model, transaction boundaries.

**Disposable artifacts** (generated, rejected, rewritten): everything inside those boundaries. A human should care intensely when an agent changes a contract, weakens a type, alters a schema, adds a state transition, or relaxes an invariant — those changes expand the space of behavior the system permits. A human should not have to read every line of an implementation that stays inside an already-accepted contract.

"Disposable" is not "irrelevant." It is a property the system must earn: enough intent must survive in the contracts, tests, examples, policies, and evidence above the implementation that replacing the code does not mean erasing the design. **If deleting the implementation deletes knowledge nobody preserved anywhere else, then the implementation is not disposable yet.**

### Limits

No architecture makes bad software impossible. Contracts can be incomplete. Types can encode the wrong model. A database constraint can faithfully enforce a mistaken business rule. The achievable goal is narrower: make broad classes of bad code impossible to express, impossible to persist, unable to cross a boundary, or immediately visible when they try.

---

## The Return of Hard Contracts and Ceremony

The conclusion: **the return of late-1990s tight-ass contracts, minus the late-1990s human cost.** Not XML everywhere, not bureaucracy for its own sake, but a healthy distrust of the implementer, made explicit everywhere it can be made to bite.

Specific recommendations:

- **Languages whose type systems eliminate broad classes of invalid programs before they run** (e.g., Rust's ownership model enforces memory safety and turns concurrency errors into compile-time failures)
- **Explicit service definitions instead of improvised network requests** (e.g., gRPC with Protocol Buffers, typed clients and servers)
- **Relational schemas as part of the executable definition** (NOT NULL, CHECK, UNIQUE, foreign-key constraints let the database refuse invalid state)
- **Workflows modeled as explicit state machines** rather than constellations of loosely related booleans
- **Generated code granted narrow capabilities** rather than ambient access to databases, filesystems, and admin APIs (least privilege exists to bound the damage of buggy or compromised code)

The old stack optimized for the cost of human authorship. The new one should optimize for the cost of machine-generated ambiguity. Ceremony has become infrastructure. Strong types, hard schemas, explicit contracts, relational constraints, state machines, and capability boundaries are no longer bureaucratic drag — they are the pressure vessel that lets you fire ten thousand engines and only spend time on the ones that hold.

---

## Containment: The Agent's World

### The --yolo Problem

To use frontier AI at its potential, you are pulled toward `--yolo`: stop asking permission, let the model edit files, run tools, launch subagents, and generally get on with it. This is also insane — the agent has a shell and can overwrite unrelated work, delete directories, poison configuration, expose credentials, or spend money through external APIs.

### Build a World the Model is Allowed to Destroy

The first mistake is putting the agent inside your world and asking it to behave. You don't need a better-behaved model. You need a smaller world.

**Design principles:**

1. **Isolated container or VM** whose destruction is acceptable
2. **Sparse filesystem view** (mounted via something like FUSE): only the repository paths, tools, build artifacts, and supporting files relevant to the work
3. **Real permissions on paths:** readable but not writable, visible only through a generated interface, or invisible from the model's perspective
4. **Scoped network access:** explicit destinations, narrow credentials, bounded side effects
5. **Compute and token budgets** to limit how large an explosion the agent can create before somebody notices
6. **No ambient authority:** the agent receives the minimum set of powers required to attempt the task. Subagents inherit those powers (or a narrower subset)

This is **capability security applied to the development environment.** The agent can operate freely inside the boundary because the boundary has already encoded the decision.

### Permission Theater vs. Containment

Clicking _Allow_ on 200 individual commands feels safe because a human was technically involved, but the human is rarely evaluating each action with enough context. A capability-bounded environment moves the meaningful decision earlier: what should this agent be able to touch at all? The agent is allowed to be reckless because the environment is not.

### Limits of Containment

Containment only solves the stupidest class of failure. A container can stop the model from deleting your home directory. It can't stop it from deleting the conceptual integrity of the subsystem it was asked to improve. That's why the hard contracts from Part 1 still matter: the filesystem boundary limits the operational blast radius; types, schemas, permissions, tests, and state machines limit the semantic one.

---

## Preserving Every Explosion: Version Control for Agents

### The Problem with Git for Agents

Traditional Git assumes a human author is curating the history. The developer recognizes a meaningful unit of work, stages relevant files, writes a commit message. Until then, the working tree is private scratch space.

This assumption is badly suited to autonomous agents. A model may spend 40 minutes exploring three approaches, get the difficult part right halfway through, overwrite it while trying to clean up, then confidently present the worst of the three attempts as the final answer. **The best state produced by an agent is often not the state in which it stops.**

### Jujutsu (jj)

Jujutsu models the working copy itself as a commit, snapshots changed contents as commands run, records repository mutations in an operation log, and supports multiple workspaces backed by the same repository. Earlier states can be inspected and recovered even if the agent never paused to package them into a clean commit.

For an agent, the operation log is a **flight recorder**. We can recover the version that passed one critical test before the cleanup broke it. We can branch a new attempt from the exact point where the original run made an interesting discovery.

This is more than undo — it's the ability to discover, after the fact, that an earlier state was valuable. It changes what `--yolo` means: from trusting the model not to make destructive edits, to assuming it will make destructive edits and ensuring none of them can erase the project's history.

### What Version Control Cannot Do

Version control can't unsend an email, unpublish a package, recover an exfiltrated secret, or refund a cloud bill from an accidental thousand-node cluster. That's why capability boundaries, credential systems, network policy, and resource budgets remain separate layers. The sandbox contains external consequences; `jj` preserves the changing repository state.

Together they answer: **what did the agent change, and how do we get back?** The third question: **which parts of what it changed were actually useful?**

---

## Branching the Search: Stacks, Bookmarks, and Candidate Trees

### Hypotheses Need Identity

A capable model doesn't approach hard work as one clean line from issue to implementation. It forms hypotheses — the bug may be in the parser, the schema, a race, an incorrect caller, or the abstraction itself. A frontier model with subagents will naturally divide that search.

What we don't want is all those hypotheses poured into one working tree like six cooks improvising in the same saucepan.

### Implementation Bookmarks

Each line of exploration needs an identity: **implementation bookmarks** — named speculative states that can fork from one another, evolve independently, be compared, and later recombine. A bookmark isn't a polished answer; it's a claim about where in the search tree an interesting possibility currently lives.

### Sapling (sl)

The continuously captured `jj` draft preserves the work while it's still wet. As useful decisions emerge, **Meta's Sapling** gives them structure. Surviving changes can be arranged into a **stack of commits**: a schema change at the bottom, a new service contract above it, generated clients above that, and implementation candidates on top.

Key Sapling features for this workflow:

- **Stacks as first-class work**
- **Smartlog** exposes relationships among commits instead of making the operator reconstruct them from branch archaeology
- **`absorb` command**: takes pending edits and, when their origin is unambiguous, moves them into the earlier commit that introduced the relevant lines

### Why Stacks Matter

A subagent working on the implementation may discover the RPC contract itself is missing a field. In an ordinary branch, it adds the field along with 400 lines of unrelated code — the contract decision is trapped inside the implementation candidate. With a stack and `absorb`, that change can be moved down into the contract commit where it belongs, and every implementation above it can be restacked against the revised premise.

The history isn't polished for aesthetic reasons. **The search is being made editable.**

The branch-and-pull-request model asks a binary question: merge or close? That's the wrong question. Failure is rarely monolithic — an agent can be wrong overall and still produce the best test, the cleanest schema, or the right security observation.

**Example:** Three agents working on scoped API credentials. Agent 1 produces a correct schema and naive authorization. Agent 2 writes right authorization but invents a catastrophic migration. Agent 3 never gets the feature working but writes the regression test that proves the first two are broken. There is no winning branch. The correct system is distributed across the failures.

A stack gives the work anatomy: the schema can survive without the authorization code, the regression test can be extracted from the failed attempt, the dangerous migration can be isolated for explicit review. **The agent should be free to generate slop. The system should be good at performing surgery on the slop afterward.**

---

## Preserving the Saga, Not the Transcript

### The Polite Fiction of Human PRs

Human-authored pull requests present a fiction: the author understood the problem, selected a design, implemented it, and arrived at the final diff by a straight line. Dead ends are omitted. The history is cleaned until the result looks intentional.

With AI, aggressively erasing failed paths becomes a mistake because **the failures are part of the evidence used to judge the survivor.** We need to know: why was candidate A abandoned? Which test killed candidate B? Did the final implementation actually resolve the original issue, or did the issue quietly mutate until the implementation could pass?

### What to Preserve

- **The issue**: original intent
- **Searchable conversation history**: reasoning and disagreement
- **`.handoff/` artifacts per workspace**: what it attempted, what it learned, what failed, which state is currently valuable, what the next agent shouldn't waste time repeating
- **Implementation bookmarks**: candidate genealogy
- **Tests and automated reviews**: evidence that killed or promoted each line of work

This is preserving the **saga** — not a surveillance log of everything the model did, but the engineering history required to understand why this particular collection of parts survived. The final review object should present evidence with enough color and fidelity that somebody can reconstruct the experiment.

---

## Scaling to a Fleet: The Work-Order System

### Two Distinct Kinds of Parallelism

1. **Intra-task parallelism:** One frontier model decomposes one objective among several subagents sharing a parent context and a common goal. (The test cell.)
2. **Inter-task parallelism:** Many top-level agents working across an entire project — different issues, assumptions, baselines, timelines, overlapping files, and occasionally incompatible ideas.

The first needs autonomy; the second needs an **operating system.**

### The Issue Tracker as Work-Order System

At ten, a hundred, or a thousand simultaneous agents, the issue tracker stops being a human backlog and becomes the shop's work-order system. Issues record durable intent, priority, dependencies, known constraints, affected contracts, and expected evidence of completion. An issue may be opened by a human, a failed test, an automated review, a production trace, or another agent.

An issue shouldn't contain the implementation — it should coordinate the search for one. One issue may launch a single workspace for a straightforward repair, ten workspaces exploring different designs, or a research-only attempt to map the affected system before code.

### Workspace Isolation and Synchronization

**Isolation:** Agents don't share one mutable filesystem, one process tree, or one set of half-written changes.

**Synchronization:** Every workspace must know which repository state it began from, which contracts have changed since, which neighboring attempts overlap with it, and whether its assumptions have gone stale. When a lower-level schema lands, workspaces built on the old schema should be rebased, retested, restarted, or declared obsolete.

### CitC-Style Cloud Workspaces

A layer (named after Google's "clients in the cloud") gives each workspace a sparse, current projection of the same underlying repository without requiring every agent to clone the entire world. The agent sees the files it needs through a mounted view; the system retains knowledge of the global repository, the workspace baseline, and every other attempt operating against it.

### Handoff Artifacts at Scale

Conversation history and `.handoff/` artifacts become more important because agents leave. Their context windows fill. The parent model is replaced. Work gets paused and resumed by something else. A durable handoff should let the next agent understand the state of the attempt without reading every token produced by its predecessor.

### Explicit Progress States

Researching, implementing, testing, blocked on a contract, awaiting another stack, superseded, ready for integration — these aren't cosmetic labels. They let the scheduler decide where to send capacity and let the human understand the shop without opening 100 chat transcripts.

---

## Assembly, Not Tournament: Recombination

### Beyond Winner-Take-All

Most multi-agent demonstrations treat parallelism as a tournament: generate several answers, ask a judge to score them, keep the winner. This is better than taking the first answer but leaves most value on the floor.

Software candidates are not indivisible contestants. In the credentials example, a tournament scores all three agents as losers. But the correct system doesn't exist anywhere in the original candidate set — it has to be assembled: take the schema from Agent 1, the authorization logic from Agent 2, the test corpus from Agent 3, and generate a fresh implementation against all three.

### Granular Selection

The earlier machinery (continuously recoverable drafts, bookmarks, stacks, smartlog, `absorb`) enables selection at several resolutions:
- An entire workspace
- One candidate bookmark
- A commit from the middle of a stack
- A logical hunk inside that commit
- A semantic change extracted from an otherwise failed implementation

### Making Recombination Ordinary

Today, doing this across branches is a small archaeological expedition. In the engine shop, recombination must be ordinary. The smartlog should expose the candidate tree and let the operator fork, move, rebase, hide, revive, compare, and combine parts directly. The version-control system should preserve where each piece came from. The workspace layer should materialize proposed combinations and run them as new candidates.

---

## The N1 Problem Returns: Integration

### Semantic Conflicts

Every candidate can look excellent in isolation. Every workspace can have green tests. Every component can satisfy its local contract. We can still assemble thirty brilliant engines into one vehicle that explodes because nobody tested the interactions.

Textual conflicts are the easy ones. The dangerous collisions are the ones that merge cleanly: two candidates can modify entirely different files and still be in fundamental conflict. They can also modify the same lines for mechanically different but semantically equivalent reasons.

### Rich Diff and Merge Analysis

A serious agent-native system needs richer analysis: types, schemas, RPC definitions, permission graphs, state machines, dependency structure, tests, and runtime behavior. The goal isn't a prettier summary — it's understanding how the behavior of one candidate overlaps with, contradicts, or complements another.

### The Integration Matrix

The engine shop needs more than a winner-selection loop. It needs an integration matrix:

- Each candidate tested against the **baseline** from which it began (did it solve the assigned problem?)
- Each candidate tested against **current main** (the world may have changed)
- Individual stack layers validated where meaningful
- Candidate combinations materialized into temporary composite workspaces and tested together

### Contract-Driven Integration

We can't brute-force every combination. The system must use structure to decide what deserves pressure:

- Candidates touching the same contract → tested together
- Changes widening a permission boundary → combined with dependent callers
- Schema migration → exercised against every implementation selected above it, against production-shaped data
- Semantic diff analysis identifies dangerous edges: shared state, shared types, retries, transactions, security boundaries, lifecycle transitions

### AI Review Inside the Loop

A review agent can inspect every change, identify suspicious surfaces, flag widened permissions, compare competing implementations, and create new issues for problems outside the current task. What it can't do is establish correctness merely by nodding at another model's work. **AI reviewing AI is another sensor on the stand. It is not physics.**

The hard judgments come first: compiler, types, schemas, constraints, permissions, tests, resource limits. Model review interprets the evidence, finds gaps, routes the next investigation. Human judgment remains where the meaning of the system changes or where available evidence can't settle the question. The point is not to eliminate human review — it's to stop wasting it on everything else.

---

## The Bitter Lesson Objection

**The objection:** This is an absurd amount of machinery wrapped around temporarily bad models. In three years the model will write the right implementation on the first try.

**The response:** Rich Sutton's Bitter Lesson argues that methods which ultimately win are general methods capable of turning increasing computation into better results — specifically, **learning and search.** The engine shop is the search half: generate candidates, apply pressure, preserve useful discoveries, reject failure, recombine what survives.

A future model may perform much more of that search internally and hand the operator one answer instead of 40 visible candidates. The shop moved behind the wall — it didn't cease to exist. Somewhere, hypotheses were still generated, evaluated, rejected, and combined. One-shot perfection is not the Bitter Lesson. Scalable computation is.

**Most of this machinery isn't compensation for model stupidity — it's compensation for parallel work being structural.** Google built enormous shared repositories and custom systems not because Google engineers were insufficiently intelligent, but because large numbers of capable people changing one shared system create coordination costs that intelligence alone does not dissolve.

Agents inherit all those problems and add several more: transient memory, higher output rate, speculative exploration, partial successes inside failed attempts, and the ability to execute commands without a human watching. Better models reduce the error rate inside each stream of work; they don't make concurrent streams stop overlapping, going stale, depending on one another, or disagreeing about the shape of the system.

**Containment is permanent.** No amount of RL makes `--yolo` safe on your real machine. Prompt injection, poisoned dependencies, hostile inputs, inherited credentials, and accidental external side effects are properties of the world the worker operates inside. Least privilege is not accommodation for incompetence — it's what allows competent people to make mistakes without converting one local action into a company-wide incident. **Competence is not containment.**

**The intent gap is permanent.** A compiler receives a formal program where consequential semantic decisions have already been encoded. An AI coding system receives an incomplete expression of intent. A better model can infer unstated intent more accurately, but it can't verify against a decision nobody has made or a business rule that exists only in somebody's head. Reliability increases as consequential parts of intent move out of human heads and into durable forms: contracts, schemas, examples, policies, tests, state machines.

**The concession is real but narrow.** Better models will reduce exploration width and may turn some of today's explicit bookmarks into internal implementation details. But an autonomous development system will still need scope, recoverable state, provenance, coordination, stale-work detection, integration, and an external representation of intent. The shop is permanent. The fixtures are not.

---

## What the Human Should Finally See

By the time a candidate reaches a person, the system should have done considerably more than run tests and generate a summary.

### The Review Object

Not a thousand-line surprise, but a **stack** whose shape explains the work: contract, migration, tests, implementation, generated consequences, operational changes. The human should see:

- The original issue and current interpretation
- Contract changes separated from their implementations
- Candidate lineage
- Important failed alternatives
- Automated review findings
- Isolated and integrated test results
- Reasons this particular stack was assembled from these particular pieces

A reviewer should be able to inspect the permission change without reading generated client code, approve the schema and reject the implementation above it, replace one candidate with another while preserving the regression suite, and know whether an implementation was selected because it was actually better or because it was the last agent still running.

### The Interactive Smartlog

Sapling's smartlog presents relevant commit history as a graph. In an agent-native environment, that graph expands beyond commits to include: issues, workspaces, conversations, agents, drafts, implementation bookmarks, stacks, checks, reviews, conflicts, and promotion state.

**A pull-request queue is an inbox. The engine shop needs a map.**

The map should show competing implementations, shared contracts, superseded candidates with unique tests, and integration candidates failing in CI. The operator should move through this graph, inspect evidence, request another attempt, extract a useful part, or kill an entire line of exploration without reconstructing history from seven browser tabs.

---

## The Compiler Metaphor

### The Economic Position

AI-generated code is headed toward the same economic position as compiler output, with one enormous caveat: **AI is a much worse compiler.**

A conventional compiler accepts a formal program and emits a lower-level representation while attempting to preserve specified semantics. An AI coding system accepts an incomplete, ambiguous, and frequently contradictory expression of intent, then proposes both the semantics and the implementation at once. Those are not equivalent operations.

But they occupy a similar economic position: the human increasingly works at a higher level of intent, a machine emits a lower-level implementation, most output passes through the surrounding toolchain without requiring line-by-line inspection, and specialists descend when behavior, performance, or evidence no longer makes sense. **The compiler metaphor describes a position in the stack, not an equivalence of behavior.**

### How Compilers Earned Trust

The industry built a formidable apparatus around compilers: language specifications, regression suites, reproducible toolchains, profilers, disassemblers, fuzzers, translation validators, formal proofs. The mature relationship with compilers was never blind trust — it was **selective distrust supported by excellent tooling.**

FORTRAN succeeded because John Backus's team understood a high-level language would be rejected unless the generated code could compete with handcrafted code. Serious programmers read the emitted assembly to check the compiler's work for years afterward — then, gradually, they stopped. Not because the output became beautiful, but because the discipline around the compiler made reading unnecessary outside unusual circumstances.

### AI as the Worst Compiler Ever Built

A compiler is deterministic translation software with formal semantics, conformance tests, and decades of research. An LLM hallucinates, forgets constraints, changes its answer, and flatters the operator. The response: **the model alone is not the compiler. The entire development system is.**

- The model generates code
- The repository supplies context
- The contracts narrow legal meaning
- The tests and production evidence provide partial judgment
- The workspace contains failure
- The candidate history records what was tried
- The human remains responsible for deciding whether the original intent was coherent and whether the result deserves to become real

### Linus Torvalds' Position

At the 2026 Open Source Summit, Torvalds objected to people boasting that almost all their code was AI-written. Under the same accounting, compilers already write all the code the machine executes. His three surviving criteria: does its job (testable), isn't stupid about its environment (reviewable), doesn't break anything around it (containment).

The Linux kernel's policy: AI-assisted contributions are allowed, but an AI agent cannot legally sign the Developer Certificate of Origin. A human must review the generated work, certify provenance and licensing, and take full responsibility. **Authorship becomes less interesting. Accountability does not.**

---

## The Artisan's New Role

### The Old Schism

Two tribes have existed long before AI:

- **Artisans:** Quality exists partly inside the implementation. Simplicity, locality, honest names, and coherent structure preserve intent and prevent the software from becoming impossible to govern.
- **Factory workers:** Quality lives primarily in the behavior of the finished system. Does it solve the problem? Is it secure enough, fast enough? Did it ship? Customers don't pay for beautiful internal interfaces.

The factory workers won the volume war decades ago. Then AI strapped a rocket to the factory.

### What AI Amplifies

A person who once needed a team can now ask a model to build an application — without understanding the data model, authorization system, concurrency semantics, or why the deployment manifest grants permission to communicate with half the internet. They judge from the outside: it launches, the button works, the demo is convincing.

This is **an N1 with a web frontend** — thirty individually impressive engines, each apparently functioning, integrated into a machine whose parts do not share one coherent idea of what the machine is. The artisans are right to recoil. But they are wrong if they believe this will restore the old economics of hand-authored implementation.

### The AI-Native Artisan (Not a Detached Architect)

The AI-native artisan is an architect in the older sense: the person responsible for the integrity of the whole machine who also understands how the machine is made. Most of the time, they work above the implementation — defining concepts, contracts, state models, trust boundaries, performance envelope, failure semantics, integration plan, and quality bar.

Then a number looks wrong. A latency distribution develops an unexplainable tail. An authorization path feels naive. A race appears only under one particular ordering. Three agents converge on the same abstraction because they inherited the same false premise. **The artisan goes back underground** — reads the implementation, opens the query plan, reproduces the interleaving, writes the low-level code the compiler would not produce, or discovers that the contract itself made the correct implementation impossible.

This is not failure of the development model. It's how abstraction has always worked: the higher layer handles ordinary cases, making expertise at the lower layer disproportionately valuable when the abstraction leaks.

### AI as Mirror and Amplifier

AI is a mirror and an amplifier. It reflects the operator's model of the problem and produces that model at scale. Give it coherent intent, strong taste, and experienced judgment, and it multiplies those things. Give it naive assumptions, weak standards, and no grasp of the system, and it multiplies those too. **If you are dumb as a box of rocks, AI does not turn you into a principal engineer. It gives your misunderstanding a fleet, a CI pipeline, and a polished README.**

Taste and experience become more valuable, not less. The scarce skill is recognizing the missing premise, the naive abstraction, the trust boundary nobody modeled, the locally sensible change that will poison the system, and the test suite that proves much less than everybody thinks.

### Which Standards Survive

AI does not make clean code irrelevant. It changes which parts of cleanliness deserve scarce human judgment:

- **Standards that encode risk** remain sacred: rules against ambient authority, explicit transaction boundaries, limits on hidden state, dependency direction, unbounded resource behavior, cleverness in security-critical paths.
- **Standards that encode preference** become automation: set the formatter and linter once, let the agent emit the boring adapter.

**Authorial beauty** (elegant compression, clever mechanisms, wit another expert can appreciate) loses economic value in ordinary application code. **Structural beauty** (few legal states, narrow authority, explicit contracts, local reasoning, replaceable parts) gains value everywhere.

The AI-native codebase may be less elegant line by line and more elegant in the shape of what it permits. It may be verbose where a human would be concise, repetitive where a human would introduce a clever abstraction, and ceremonial where a human would prefer flow. That is acceptable when the repetition is generated, the contract is clear, and the behavior is easier to judge.

The judgment that used to go into a beautiful function now goes into a **beautiful contract**: the schema making invalid states unrepresentable, the state machine with no illegal transitions, the permission model exactly as wide as it needs to be. These artifacts are read by humans, carefully, indefinitely. **Contract changes are the new bottleneck — and the right bottleneck to have.**

---

## Reference Surfaces: The Three-Plate Method

### The Machinist's Problem

How do you make a genuinely flat reference surface without already owning a flatter, more precise machine? Comparing two plates is not enough (they can fit perfectly while one is concave and the other convex). The solution is the **three-plate method**: three imperfect surfaces compared in alternating pairs, high spots marked and scraped away, precision emerging from repeated disagreement among all three surfaces — no single plate accepted as the original truth.

### Software's Reference Surfaces

This is "almost suspiciously appropriate" for software, which has no single physics oracle. The types disagree with the tests. The tests disagree with production traces. The implementation disagrees with the profiler. Two candidate stacks disagree about the contract. The model's explanation disagrees with the hardware counters. **Every surface is imperfect. Their disagreement tells us where to look.**

The **reference surfaces of software** are the durable bodies of intent and evidence from which implementations inherit their precision: contracts, schemas, tests, policies, examples, traces, and failure history. Someone still has to scrape them.

### The Craft Concentrates

The flattest surfaces in the world are still made by hand, because you cannot use a machine to make a surface more accurate than the machine itself. Every precision machine on earth is descended, through generations of tooling, from a reference surface somebody scraped manually. Mechanization didn't eliminate that craft — it concentrated it at the top of the accuracy chain, in the few places whose precision everything else inherits.

**That is where the artisans end up. Not gone. Concentrated.**

---

## Why Not "Factory"

"I keep reaching for the phrase 'AI software factory,' and every time I say it I wince."

A factory is organized around repeatability — it suppresses variance and stamps known parts from a settled design. The system described deliberately creates variance: many hypotheses, many candidate implementations, liberal experimentation, severe selection, salvage, recombination, integrated pressure, and a scrap bin that does most of the quality control. It is not an assembly line.

**The right image is the engine shop.** The agents produce engines. The shop fires them. Part 1 supplies the pressure vessel. Part 2 supplies blast walls, black boxes, test cells, and machinery for preserving and recombining useful wreckage. Part 3 supplies the people who decide what holding pressure actually means.

The analogy remains imperfect because software never speaks for itself as honestly as metal under load. Contracts and tests reveal only what we have taught them to notice. A component can survive every test and still embody the wrong intent. That is why the artisan remains — not hand-polishing every generated part, and not as a detached architect, but as the custodian of meaning, quality, coherence, and the reference surfaces from which the rest of the system inherits its standards.

---

## Key Technologies Referenced

| Technology | Role in the Engine Shop |
|-----------|------------------------|
| **Rust** | Type system eliminating broad classes of invalid programs; ownership model enforces memory safety and turns concurrency errors into compile-time failures |
| **gRPC / Protocol Buffers** | Explicit service definitions; typed clients and servers generated from interface definitions |
| **PostgreSQL constraints** (NOT NULL, CHECK, UNIQUE, FOREIGN KEY) | Database refuses invalid state rather than trusting every generated caller |
| **Jujutsu (jj)** | Working copy as commit; operation log as flight recorder; continuous mutation history |
| **Sapling (sl)** | Stacks, smartlog, absorb; makes search editable and candidate relationships visible |
| **FUSE** | Sparse filesystem views for capability-bounded agent workspaces |
| **Google CitC** (clients in the cloud) | Sparse, current projections of shared repository without full clones |
| **Noumena Code** | The product built by the authors embodying these principles (issues → scoped workspaces → agent drafts → stacks → review) |

---

## Glossary of Concepts

| Concept | Definition |
|---------|------------|
| **Engine shop** | The operating model: generate many candidates, fire them under pressure, preserve useful wreckage, recombine, repeat. Liberal generation, severe selection. |
| **Hard contracts** | Types, schemas, service definitions, state machines, invariants, permission boundaries — the durable artifacts defining what the system means |
| **Disposable implementation** | Code that can be regenerated, rejected, and replaced without ceremony because intent survives above it |
| **Local oracles / hard judges** | Compilers, type checkers, database constraints, state machines — mechanisms that reject invalid implementations mechanically |
| **Blast walls** | Sandboxes, capability boundaries, resource budgets — limit damage from what judges fail to recognize |
| **Capability-bounded environment** | Agent receives minimum powers required; subagents inherit or narrow those powers; no ambient authority |
| **Sparse filesystem view** | Agent sees only the repository paths and tools relevant to its task, with real read/write permissions per path |
| **Flight recorder** | jj's operation log preserving every repository state so useful intermediates can be recovered after the fact |
| **Implementation bookmarks** | Named speculative states in the candidate tree; not polished answers, but claims about where interesting possibilities live |
| **Stack** | Ordered series of commits: schema → contract → clients → implementations; makes work editable and reviewable by layer |
| **Smartlog** | Graph showing commit/candidate/workspace relationships; the map replacing the pull-request inbox |
| **Saga (vs. transcript)** | Preserving engineering history (why candidates survived or died) rather than raw activity logs |
| **`.handoff/` artifact** | Per-workspace document: what was attempted, learned, failed; what the next agent shouldn't repeat |
| **Recombination** | Assembling the best parts of multiple failed candidates into a system that no single candidate contained |
| **Integration matrix** | Testing candidates not just against their baseline but against current main, overlapping contracts, and each other |
| **Semantic diff** | Analysis of how candidates overlap, contradict, or complement — beyond textual line changes |
| **Authorial beauty** | Elegant compression, clever mechanisms, aesthetic code — loses economic value in ordinary AI-generated code |
| **Structural beauty** | Few legal states, narrow authority, explicit contracts, local reasoning — gains value everywhere |
| **Reference surfaces** | Contracts, schemas, tests, policies, traces — the durable bodies of intent from which implementations inherit precision |
| **Three-plate method** | Precision emerging from repeated disagreement among imperfect surfaces — applied to software's competing oracles |

---

*Series published July 12, 2026 at noumena.com/essays/series/the-engine-shop/. The authors disclose in Part 2 that this is the system Noumena has been building (Noumena Code), and that the architecture grew out of trying to use frontier agents to build a real system with a team of one.*
