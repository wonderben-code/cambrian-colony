# The Colony Architecture: A Conceptual Design for Purpose-Driven, Self-Evolving AI Systems under Fixed Human Control

**Ekram Alam** · Alam Labs, Cambrian Colony project · 21 November 2026

Conceptual paper, version 1. The design described here has not yet been implemented.

## Abstract

The Colony Architecture is a design for an AI system that is pointed at any entity with a purpose. It works out what needs solving, grows a specialised solver for each part of that purpose, and improves both the entity and itself under fixed human control. This paper states the design in standard terms. It reports no implementation and no results.

The design has five layers. An Organism is one solver that builds its own capabilities from data and tries every solution in simulation before reality. A Colony arranges many organisms around a live simulation of a client and links them through a shared hive. A layer called What for Any Why represents the client as a tree of purposes. It gives each type of purpose its own species of solver, and species emerge, bud and go extinct as the tree changes. Two further layers treat the system's own parts as building blocks and apply the whole pattern to the system itself, within caps. Every reasoning part runs on a replaceable foundation model. A fixed guard, which no part of the system can write to, judges and applies every change the system makes to itself.

Most of these ingredients have published precedent, and the paper says where. It offers three candidate contributions, each as a hypothesis: selecting solvers inside a simulation of the specific organisation they serve; tying speciation to a tree of purposes; and Alam's Razor, a rule that caps every regress in a self-improving stack. It lists nineteen testable hypotheses and the plan for a first build.

**Keywords:** self-improving agents, multi-agent systems, evolutionary search, digital twin, goal decomposition, human oversight of AI.

## 1 Introduction

Agents built on foundation models can carry out many single tasks. Two design problems remain open when such agents are pointed at a whole organisation and allowed to improve themselves.

The first is what to optimise and where to test it. Published systems that design or improve agents automatically commonly score candidate designs on benchmark tasks (Hu et al., 2024; Zhang et al., 2025). An organisation is not a benchmark. Its work is a set of interdependent purposes that change over time, and a change that scores well alone can fit badly with the rest.

The second is who judges a system's changes to itself. A loop that proposes, scores and adopts its own modifications can promote a change that only looks better. One recent paper reports that a greedy keep-if-better gate produces false commits when the same held-out set is reused (Shawn, 2026).

The Colony Architecture is one proposed answer to both problems. A client, meaning any organisation or system with a purpose, is represented as a live simulation of its purposes, arranged in a tree and grouped into types. Each type of purpose gets its own species of solver. Candidate solutions compete on each purpose inside the simulation, and the set of winners is then judged as a whole. Solvers draw on a pool of capabilities that the system builds from data for stated needs and tests before use. Every change is tried in simulation first, and the client decides what is implemented.

The same pattern is applied to the system's own parts, under explicit caps. A small fixed program, the guard, holds the controls and the record. It judges and applies every self-change, and no part of the system can write to it.

**What this paper contributes.**

1. A statement of the architecture in five layers, in standard terms, with each of the design's own terms defined (section 4 and Appendix A).
2. An account of how one round of work runs, and of how self-change is bounded by a fixed guard, by caps and by a dependency rule called Alam's Razor (sections 5 to 7).
3. A comparison with published work that separates what the design shares with existing systems from three candidate contributions (sections 8 and 9).
4. Nineteen hypotheses that a first build can test, and the plan for that build (section 10).

**What this paper is not.** It is not a report of a working system. The design has not been implemented. Nothing here is evidence that it works, or that it does better than any other system. A companion technical paper gives the precise definitions, the main loop step by step, and the engineering decisions still to be made.

The design was developed in private notes in 2019 and 2020 and assembled into one specification in November 2026. Section 2 explains what that history does and does not establish.

## 2 Origin and scope

The design comes from the author's private working notes, and this paper is its first public statement.

**The notes.** The first ideas, a data refinery and two cooperating builders, come from an undated session held some months before February 2019. The dated notes run from 4 February 2019 to 18 May 2020. They develop the design roughly in the order of the five layers. The Organism dates from early February 2019 and the Colony from 12 February 2019. A revision of 28 February 2019 became What for Any Why. The building blocks date from February and April 2019, and the loop upward was last revised in May 2020.

**The assembly.** In November 2026 the notes were assembled into one specification and restated as one design. Where the notes leave a choice open, the author made design decisions, 44 in all, dated 21 November 2026. Three mechanisms were added in the same month: need requests, the round history and the promotion rule. Table 1 shows which elements date from when.

Table 1. Where each element of the design comes from.

| Element | Source |
| --- | --- |
| The Organism: brain, two arms, pool, simulation before reality | Notes, February 2019 |
| The Colony: client simulation, hive, tournament, wasps, reins, budding | Notes, February 2019 |
| What for Any Why: purpose tree, a species per purpose type, emergence and extinction | Notes, February and March 2019 |
| Caps and Alam's Razor | Notes, 2019; the Razor is dated 2 March 2019 |
| Building blocks and Glow | Notes, February and April 2019, marked there as conceptual |
| The loop upward | Notes, April 2019 to May 2020, marked there as unfinished |
| Need requests, round history, promotion rule | Added in November 2026 |
| The guard, the protected core and the custody rules | Decisions of 21 November 2026 |
| Nested selection with repair of fit; a cap on lost rounds; attaching only what is implemented | Decisions of 21 November 2026, built on the two levels of scoring in the notes |
| A foundation model in every reasoning seat, replaceable under the promotion rule | Decision of 21 November 2026 |

**What the dates establish.** Private notes do not establish academic priority. They show only when the author worked on an idea. Priority in the literature belongs to the first public disclosure, which for this design is the date of this paper. The notes were private, so later published systems that resemble parts of the design were developed independently of them. Unlike this design, those systems are implemented and evaluated (section 9).

**Scope.** The paper covers the whole design at a conceptual level. Two later directions in the notes, simulated worlds and matrix architectures, are left as future work. The notes give the mission behind the design as "Accelerating human progress and advancement", with democratising AI as the means.

## 3 Design principles

Six principles govern the design. The first five come from the notes. The sixth is a decision of November 2026.

**Self-evolving.** The system changes its own population of solvers as the task changes. One note describes a problem that "gets converted into a series of problems which the model then solves in a self-evolving way".

**Self-improving, within limits.** Every mechanism is open to improvement, including the mechanisms that improve. Three limits apply. Every self-change must pass one gate, the promotion rule. A protected core is beyond the reach of any self-change. The part that proposes a change never judges or writes it (section 6).

**Inspired by evolution, but directed.** The design uses variation, selection, inheritance, speciation and extinction. It does not copy natural evolution: "evolution is wasteful, so the more targeted the better". Capabilities are built for a stated purpose, and every search is capped.

**Capped.** Every search and every recursion has an explicit limit. The notes put it as "KEEP IT FINITE, KEEP IT CAPPED". A cap can be moved further out, but only by the human owner. A low cap is enough to show a principle, and the capped version is the one to build.

**The what and the why are fixed, and every how is replaceable.** The purpose of the system is fixed. Every way of achieving it may change. The author's image is "the WHAT is like the calcified bones of the skeleton.....the specific HOWs are just the flesh wrapping it". Each how was made for a purpose of its own, and that purpose is kept when the how is replaced.

**The model is a replaceable part.** Every reasoning seat in the architecture runs on the strongest available foundation model. The architecture depends on no particular model. A stronger model is swapped in only if it passes the same gate as any other self-change.

## 4 The architecture

The design grew in five stages, which this paper arranges as five layers. Each layer adds one idea to the layers below it (Table 2). A further element, the guard, belongs to no layer and is described in section 6.

Table 2. The five layers.

| Layer | Name | What it adds | Closest standard idea |
| --- | --- | --- | --- |
| 1 | The Organism | One solver that builds its own capabilities and tries its solutions in simulation before reality | An agent with a skill library and a sandbox |
| 2 | The Colony | Many organisms around each client, selected by tournament inside a simulation of that client and linked by a shared hive | Population-based search over agents, with a digital twin as the test environment |
| 3 | What for Any Why | Generality: the client as a tree of purposes, a species per type of purpose, an individual per purpose | Hierarchical goal decomposition combined with niching |
| 4 | Building blocks and Glow | The system's own parts become blocks that it arranges into new solvers | Automated design of agent systems |
| 5 | The loop upward | Groups of whole systems, and the pattern applied to itself | Recursive self-improvement, here bounded by caps |

The layers are not independent modules. Organisms in Layer 1 are created by the budding described in Layer 3, so a build needs the first three layers together. Layers 4 and 5 are the least developed parts of the design, and the notes say so.

&#91;embedded content: architecture at a glance: client, colony, guard, owner\]

Figure 1. The architecture at a glance. Data enters the Colony only with the client's permission, and only approved changes reach the client. Brains propose changes to the system. The guard, which only the owner can change, judges and writes them.

Three terms recur. A *client* is the entity the system is pointed at. A *purpose* is anything the client is trying to achieve, from a single process to the whole organisation. A *brain* is a reasoning part of the system. In a present-day build each brain is a seat filled by a foundation model.

### 4.1 Layer 1: the Organism

The Organism is a single solver. Its fixed purpose, in the author's words, is that "the user can communicate their thoughts i.e. what they want or need done, and the AI figures out the way to do it." Everything else in this layer is a replaceable how.

**Body plan.** Every organism has the same four sections.

| Section | Job |
| --- | --- |
| Brain | Analyses the client and keeps its simulation current |
| Capabilities | Finds data, builds capabilities from it, and recombines them into candidate solutions |
| Application | Carries the winning solution once it has been accepted |
| Implementation | Carries out an approved change in the real client |

The brain studies an entity through what the notes call the "splinter, operate, fuse, integrate cycle": one analysis unit goes out to each entity, and the findings are fused on return. At first contact it uses public information only.

**How capabilities are made.** The capabilities section works as a chain of six steps, which the notes call the fusion chain.

1. A left arm finds the data relevant to a stated need.
2. A body organises that data into a form a capability can be built from.
3. A right arm builds the capability.
4. The capability must pass a test gate before it enters a shared pool.
5. A combination creator recombines the pool into candidates.
6. The best candidate for each place is selected and used there.

A *capability* is whatever the arms build. In a present-day build it is an agent skill with a fixed interface and its own test: it states what it takes in, what it returns and how it is checked. That form is itself a replaceable how.

The pool holds nothing that the two arms did not build, and nothing untested enters it. Tools that a client supplies are declared through the client's controls. The combination creator is the only producer of candidates.

**Simulation before reality.** An organism never tries a solution on the real client first. It tries it in simulation, and only an approved result is implemented (section 5).

**A record of every change.** Every implemented change to a client's processes is marked and recorded, and the record is visible to the client. The notes call this piping. The client owns its upgraded processes, and the system keeps its own methods.

The notes add three smaller mechanisms. Deposits are insights found outside the system, which dedicated parts fuse and put in place. A clone is a short-lived copy of an organism that folds back into the original or is discarded. A twin is an identical copy for an identical purpose.

### 4.2 Layer 2: the Colony

The Colony is a population of specialised organisms, arranged around each client and linked by one hive.

**What every client gets.** Five things are attached to each client.

- A *client simulation*: a real-time model of the client's processes, divisions, priorities and hierarchies. It is built only with the client's permission, from the tools the client already uses.
- *Client reins*: the client's controls over everything the system does for it (section 7).
- A *communication line*: an AI assistant through which the client talks to the system and grants access.
- A *client wasp*: a separate analyst that looks for the client's weaknesses.
- Its own arrangement of organisms, one for each purpose.

**Two simulations.** The client simulation is the design's version of a digital twin of an organisation. Each organism also has an *organism simulation*, which is its model of itself. The notes call this the core simulation. Reading it as the organism's self-model is a decision of November 2026.

Candidates are permutations of an organism, and they are tried on a trial copy of the client simulation. The notes describe the test that counts as a meeting: "the simulation is when organism permutations MEET bespoke client simulations and aim to accomplish objectives."

**What competes.** An organism's current specification is called its *base template*. It has two parts: the organism's own processes, and the pairings between those processes and capabilities from the pool. A *candidate* is one complete set of pairings. The two parts are changed by different mechanisms. Pairings change only as the outcome of a tournament. Processes change only through the gated self-change of section 6.

**The tournament.** Candidates compete inside the simulation, and only the best result acts: "whatever is best for the client gets applied." Client problems come from three sources: what the client asks for, tests of its simulation, and the wasp's checklist of weaknesses. Under a rule in the notes, a winner must both serve the client's purposes best and have fixed the items on that checklist.

**The pool.** The hive holds one pool of capabilities. A capability enters it only through the test gate. That applies equally to the capabilities carried by losing candidates.

**The hive.** All organisms form one colony with a shared hive. A *hive log* records every cycle, species, lineage and change, and every choice a client made or declined. In the notes, each gain made for one organism is meant to lift the whole colony by more than it lifts that organism. How the hive decides what to share, and how it shares it, is left open in the notes.

**Wasps.** A wasp is a red-team analyst that belongs neither to the client nor to the hive. A client wasp's findings become a capped checklist that winners must fix. An organism wasp looks for weaknesses in an organism itself. In the first build its findings are an input to the part that proposes improvements to organisms (section 4.3).

**Client data.** The system learns how a client works from the tools the client already uses. In notes of March 2019, a client's data is first migrated in simulation, to show any loss before the real move. It stays where the client chooses, and a mistake can be undone "with a click".

**An acknowledged gap.** The tournament does not test how well a brain builds simulations, because the client simulation already exists when the contest starts. The notes offer proposals, not a settled method: a second test round beside each client round, and a separate brain-improving part whose results are tested in a sandbox. Fidelity is measured by comparing each real implementation with its simulated one.

### 4.3 Layer 3: What for Any Why

This layer makes the Colony general: one mechanism that finds a what, meaning a solution, for any why, meaning a purpose. The author compares it to the design of neural networks or the internet: "its a governing dynamic".

**The purpose tree.** The client simulation is organised as a tree of purposes: "a structural system of representing the client as purposes, and then how those purposes relate to each other". Some purposes are single processes. Others organise the purposes beneath them, and a relation between purposes counts as a purpose too. For a client the notes give three levels: the single process, the cluster of processes and the whole client. A dedicated part, the purpose-determining AI, works out the purposes. The size of the tree is capped.

**Species and individuals.** Purposes are always grouped into *purpose types*. Each type has a *species* of solver, and each purpose has one *individual* of that species. The population therefore mirrors the tree, and it changes when the tree changes (Table 3).

Table 3. How the population follows the purpose tree.

| In the client | In the Colony |
| --- | --- |
| A new type of purpose appears | A new species emerges |
| A new purpose appears in an existing type | The existing species buds a new individual |
| Two purposes are identical | The second gets a twin of the first |
| A purpose ceases to exist | Its individual goes extinct |
| A type of purpose ceases to exist | Its species goes extinct |

In the notes an organism lives only while its purpose exists and it is the best at it: "does the purpose for which you were created exist anymore? No? Then you are extinct". The author's summary is "budding for small changes, emergence for big".

**Where organisms come from.** The hive holds one standing organism, the *progenitor*. Its brain sets up each new client, and every species starts as one *parent* that buds from it. A parent takes its *intent* from its purpose type. Intent decides what data is found and what is built, which is what makes the search directed. A parent then produces one individual for each purpose of its type. Budding happens around a purpose, not around a client.

**Two sides of improvement.** Organism brains work only on client purposes. A separate brain, which the notes call the other brain, works on the organisms' own internal processes: "the same way the client is improved, the organism is improved". There is one per hive. It proposes changes and never judges or applies them (section 6).

**Ranking.** The notes score at two levels: each purpose separately, and how the solutions fit together. They also accept that there may be no single best: "its what parameter you are prioritising that determines rank". A proposal for the first build is that the client sets that priority.

**The sequence for any purpose.** The mechanism does not need a client. The notes give it as six steps that run on any purpose.

1. Start with a purpose.
2. Break it down into purposes that relate to one another.
3. Classify each under a purpose type.
4. Each type has a parent.
5. Each parent produces one individual per purpose.
6. The winner for a purpose creates the improvement.

The wasp gains a second role in this layer. It can restructure the purpose tree itself by rearranging, creating and deleting purposes, and purposes that move keep their organisms. This role is built last.

### 4.4 Layer 4: building blocks and Glow

This layer treats the system's own AI parts as building blocks that can be arranged into new solvers: "Its lego." The notes mark the layer as conceptual, and the first build includes it only in a minimal form.

**Blocks.** In February 2019 the blocks are six roles that work together (Table 4). In April 2019 the definition widens to "Building blocks (algorithms, AI)". The notes do not reconcile the two.

Table 4. The six roles.

| Role | Function |
| --- | --- |
| Simulator | Simulates the client's processes |
| Internal processes | Breaks the processes down into problems |
| Classification | Classifies the problems into types |
| Problem solving | One per unique problem; chooses among what the two arms built for that problem |
| Left arm and right arm | Find the data for a kind of capability, and build it |

**Configurations.** A *configuration* is an arrangement of blocks that solves a problem. The model described in Layers 1 to 3 is itself one configuration.

**Glow.** Glow is the configuration that builds configurations. It has four supporting parts: a supply of blocks, a separate tester, an inventor of missing blocks, and a simulation in which Glow tests configurations that use blocks that do not yet exist. A second configuration, the problem-finder, finds problems for Glow.

Glow assembles blocks into configurations, up to a cap. The tester checks each one in simulation. Those that solve the problem live, and the rest are discarded. One of those that live is superior and is then improved. These live-or-die outcomes are Glow's only teacher.

**Glow turned on itself.** Glow can be given its own job as a problem: build a better Glow and a better problem-finder, one level up, to a cap. A changed Glow replaces the current one only if it passes the promotion rule of section 6. Whether that test stops such a pair from passing by setting itself easier problems is untested.

How the six roles map onto the parts of Layer 3 is deliberately left open. The first build is expected to show the mapping.

### 4.5 Layer 5: the loop upward

This layer sets out how the design is meant to scale, and it is the least finished part. In May 2020 the author wrote of it "I don't think it's finished".

**Universal bespoke.** The hypothesis in the notes is a "universal model than adapts to the bespoke needs by making itself bespoke". A problem given to the model is called a ball. The notes add a caution that applies to every test in the design: a variant can fail a ball chosen in advance and still hold the key to a ball not yet chosen.

**Clusters.** Each bespoke model can become a building block of a larger one, up to a cap. The stated hypothesis is that Organisms arranged in symmetry form a new model that keeps the bespoke property.

**The Controller and the Game.** In April 2019 the author reframes the aim: build the AI that runs the whole research project, called the Controller. The Game is that search set up as a game. Its stated goal is general intelligence, defined for this purpose as outperforming people across 100 abilities, measured facet by facet.

**The ladder.** The final step, from May 2020, is that the self-evolving engine is the loop itself and none of the named models alone. The rungs are the Organism ("like a primitive organism"), the Colony, the Facet ("an organism made of multiple cells") and societies of Facets. The highest level reached is placed back into the colony arrangement, and the ladder runs again. The number of runs is capped, and each new level is adopted only under the promotion rule of section 6. The notes name the whole phenomenon Machevo, and give its end state as an engine that "has mastered its own self evolution".

This paper treats Layer 5 as a statement of direction. It makes no claim that the design leads to general intelligence. That link is a hypothesis in the notes and is listed as open (section 11).

## 5 How a round runs

Work proceeds in rounds, and nothing real changes until a client approves a result that has already won in simulation. This section gives the outline. The technical paper gives all nineteen steps of the loop with their inputs, outputs and stopping conditions.

&#91;embedded content: one round: the main steps, the repair of fit and a lost round\]

Figure 2. One round. The steps on the left run in order. If the winners do not fit together, fit is repaired up to a cap, and beyond the cap the round is lost. The line on the left returns to the first step for the next round.

**Set-up, once per client.**

1. *First contact.* The progenitor's brain opens a communication line and analyses public information only. Nothing is read from the client's systems until the client grants access with a stated scope.
2. *Controls first.* The client's reins are created and the grant is recorded before any organism starts.
3. *Simulate.* From data in the client's existing tools, within scope, the client simulation is built as a purpose tree.
4. *Group and burst.* Purposes are grouped into types. One parent per type buds from the progenitor, in an order that respects the dependencies between purposes.
5. *Assign the wasp.* A client wasp starts writing its checklist of weaknesses.

**One round.**

1. *Open.* The guard reads the owner's and the client's settings and loads the caps.
2. *Build.* The arms build capabilities for stated needs only. Each must pass the test gate to enter the pool.
3. *Rank and combine.* For each process, the options are ranked and only the top few are kept. The kept options are then combined into a capped number of candidates. The procedure is fixed and capped, which is how the design applies Alam's Razor at this point (section 6).
4. *Compete per purpose.* Each candidate is tried on its purpose on a trial copy of the client simulation. One candidate wins each purpose.
5. *Judge the set.* The winners for all purposes are tried together as a set, and the set is scored on how well its members fit. This is nested selection: a contest per purpose inside a contest for the whole.
6. *Request what is missing.* Each purpose that failed publishes a need request, a short statement of the capability it lacked. The arms build for it in the next round.
7. *Approve and implement.* The reins show the client how each change would take effect. The client decides if, when and how far it is implemented. This is the only step that changes anything real.
8. *Attach and record.* What was implemented becomes the standing solution. The real outcome is written back into the client simulation, and the round is appended to the log.
9. *Review.* Watchers report, each part is scored, and the round closes.

**When the winners do not fit together.** Fit is repaired inside the same round, in three capped stages. First, the set is re-formed from the few best candidates already held for each purpose. Second, if no combination passes, the worst-fitting purpose is run again with the other winners held fixed, and fit with them counts in its score. Third, if the cap on re-runs is reached, the round is lost. Nothing is implemented, and a written post-mortem names the purposes that would not fit.

**When a purpose keeps losing.** Consecutive lost rounds are counted for each purpose. At a cap set by the owner, the purpose stops competing, a need request is filed for what is missing, and the matter is raised to the owner and the client. One stuck purpose therefore cannot hold the others back without limit.

**Between rounds.** Nothing acts on candidates between rounds. Each contest starts from the few best it kept and from whatever the pool has gained. The gap between rounds is the only place where a change to the system itself can be written (section 6).

**When the client changes.** A new purpose in an existing type opens a contest under that species' parent. A new type makes a new species emerge. A purpose or type that ceases to exist makes its individual or species go extinct. Operation is continuous, in the notes' words "a machine with no start or end".

Later rounds are always compared against what was last implemented, never against a result that won in simulation and was not put into practice.

## 6 Self-improvement under a fixed guard

The system may improve any of its own parts, but the part that proposes a change never judges it or writes it. This section describes how that separation is meant to work.

### 6.1 Three kinds of change, three gates

The system makes three kinds of thing, and each has its own gate (Table 5).

Table 5. What the system makes and what gates it.

| Kind | Proposed by | Gate | Written by |
| --- | --- | --- | --- |
| Solutions to client purposes | The combination creator, as candidates | The tournament at both levels, then the client's approval | The guard, once the result is implemented |
| Capabilities | The arms, for a stated need | The test gate | Entered in the pool on passing |
| Changes to the system's own parts and processes | A brain | The promotion rule | The guard |

*Self-change* means the third kind. The design keeps it apart from the first. Pairing capabilities with processes is solution-level work and belongs to the tournament. Any edit to an organism's own processes is self-change.

### 6.2 The guard

The guard is a small fixed program that stands outside every organism and every brain. No part of the system can write to it, and only the human owner can change it. In security terms it plays the role of a reference monitor, and what it protects is a trusted computing base.

The guard holds six things.

- The reins and the caps (section 7).
- The hive log, to which it alone appends.
- The only writable copy of every organism's base template. Organisms and the hive work from read-only copies, so any write to a template is a proposal to the guard.
- The measures it judges with: the scorer, the test of whether a purpose is solved, and the score for how well a set fits.
- The promotion rule, which it alone runs.
- The count of lost rounds for each purpose.

The reins, the caps, the log, the promotion rule and the measures form the *protected core*. No route of self-improvement can change it. A proposal aimed at the guard, the reins, the caps, the log or the promotion rule is refused outright. A proposal to change a measure is judged with the existing measures and also needs the owner's approval, so no proposal can grade itself. Base templates do change, but only by the guard's hand.

Self-change is thus split into three roles. A brain proposes. The guard judges. The guard applies the change if it wins. The guard itself produces no candidate and no proposal.

### 6.3 The promotion rule

A self-change is adopted only if the changed system does better than the current one on a replay of recorded rounds it has not seen. The guard holds those rounds back, runs the replay in a sandbox and compares the results with the existing measures.

Every route of self-change passes this one gate. The routes include improvements to an organism's processes, changes to a brain's way of working, Glow building a better Glow, the system acting as its own client, and replacement of the foundation model.

Two conditions keep the gate shut at first. Self-change is frozen until enough rounds have been recorded to hold some back. The first stage of the first build also runs with self-change frozen, and uses that time to measure whether replay predicts live results. The freeze lifts only when that evidence exists.

The rule is a hypothesis, and the design treats it as one. It builds on a replay test in the notes that the author himself doubted: "I'm not convinced thats the way to iterate the brain....I don't think thats the best way".

Published results add three cautions. A greedy keep-if-better gate can commit false improvements when a held-out set is reused (Shawn, 2026). One small pre-registered study of a retention gate for agent memory found a benefit it could not distinguish from zero (Zueco et al., 2026). Wang et al. (2025) report a mismatch between a coding agent's benchmark score and its potential for further self-improvement. Replay also cannot distinguish changes to parts that act outside a round, such as set-up.

### 6.4 Caps and Alam's Razor

Every search and every recursion in the design has a cap, and only the owner can move one. The notes tie ranking itself to caps: "you only rank order if there is a cap". While the design is being developed, every regress is capped at a single layer.

Alam's Razor is a rule about dependency between parts. The author named it on 2 March 2019 and described it as a general principle for any model. If one part's processes depend on a second part, the second must not depend on those same processes run on the first, unless there is a cap. Without one, "you get infinite regression and the first entity never gets changed". The rule itself reads: "A can rely on A1 to exist, but A1 should not rely on a regressive of the same nature i.e. there needs to be a CAP / Or no reliance whatsoever".

The notes separate two kinds of regress. Some are needed for a part to exist, and others only improve it. Caps apply to both. An improvement chain can be stopped anywhere, because a part's existence does not depend on its improver. An existence chain is stricter: the cap must do its work by a process that does not itself rely on a regress of the same nature. The author prefers the cap to be applied at once and not several layers out.

The underlying logic, that a recursion needs a base case, is elementary. What the design adds is its systematic use as a constraint on a self-improving stack, with the depth of every regress made an explicit setting.

The design applies the rule in several places.

- *Simulations.* The client simulation relies on the organism simulation, so the organism simulation must not come to exist by the same process. The notes say "I advocate putting the cap right there in the core", and nesting of simulations stops at one level.
- *Variant-making.* Whatever produces a parent's variants must not itself need a round of variant-making. This is why rank and combine is a fixed, capped procedure.
- *Self-judgement.* A part judging or writing its own change is a loop. The guard cuts it.
- *Glow.* Glow and the problem-finder set each other's work. Replay on recorded problems and a cap on the stack of Glows are meant to cut that loop.

Two loops are not yet cut, and the design says so. The brain that builds a client simulation also measures that simulation's fidelity. The pool is filled from needs raised in rounds that are later held back for replay, so the pool has already seen them.

### 6.5 The system as its own client

The sequence that serves a client can be run on the system's own parts, up to a cap. A passage of 2 March 2019 describes any entity of the system having the sequence run on it. In such a run the winning improvement is a change to the system, so it is not implemented directly. It goes to the guard as a proposal and is written only if it wins on replay. The guard and the protected core can never be the client.

The notes give no control design for this loop beyond caps and reins. Asked whether it means general intelligence or runaway AI, the note answers "Who knows".

### 6.6 Need requests and the round history

Two smaller mechanisms were added in November 2026 alongside the promotion rule. Under *need requests*, nothing is built in advance. A capability is built only when an organism that failed part of its purpose states what it lacked.

The *round history* is an append-only record of rounds inside the hive log. Each entry holds the round, the candidate concerned, the result and a written post-mortem. The real outcome is added later for comparison with the simulated one. The history is a record only. It stores no rankings, and no candidate is produced, ranked or selected from it. Its readers are the guard, which draws held-back rounds from it, and people.

## 7 Human control and safety

People stay in the deciding through two sets of controls, and the design requires each control to be enforced at a named point in the loop.

**Owner reins.** The owner of the system has switches that turn parts on or off, and dials that set how intensely each runs. A viewing window shows outcomes in simulation before anything is implemented. The owner can see and control at any level, from parts of one organism to the whole hive. The owner alone can move a cap or change the guard.

**Client reins.** Each client controls its own relationship with the system. The system suggests and the client decides: "clients can really choose when and if they implement what the colony is suggesting they do." The client chooses how much of itself to expose. It sees its simulation, which organisms handle each part, the suggested changes, live implementation and its wasp's work. It decides if, when and how far each change is implemented, piece by piece.

**No silent access.** Everything the system does for a client is visible through that client's reins. Every access to a client's data or systems is visible to the client, and no silent level of access exists.

**Data rules.** Data that the system needs is secured as strongly as possible. Data that it does not need is not kept.

**Three safety rules.**

- *Simulation first.* Every candidate change is tried in simulation before reality.
- *Start small.* "Risks are minimised by 1) Simulations 2) Being able to do the demo on a small scale 3) Scaling up the demo in every dimension".
- *No live experiments on clients.* Learning from many tiny real changes was considered and rejected: "NOPE....too risky for client."

**Oversight.** Watchers are "AIs that are tasked with simply monitoring the other AIs", and the stack of watchers is capped. Each part of the system has a *quotient*, a score of how well it does its job. A proposal for the first build is to score each part on purpose fit, simulation fidelity, autonomy, improvement per cycle and conduct within the limits of the reins. A *golden quotient* measures how far the others are met with AI doing the work, while people stay in the deciding through the reins.

**Where controls are enforced.** The design requires that no action reaches the real system without passing a control. The system touches the outside world in four ways. It reads public information, it converses over the communication line, it reads client data within the granted scope, and it changes the client after approval. The guard enforces scope on every read, approval on every change, the caps, the append-only log and the freeze on self-change.

Some controls have no settled enforcement point yet. Three are run by parts outside the guard: the test gate, the simulation-first rule and the order of writes to a client simulation. The communication line, the access levels and the intensity dials are not yet enforced by anything. Alam's Razor, the data rules and the ban on live experiments are design rules with no runtime check.

**Control at the top level.** The later notes accept that a system built to improve machine intelligence, fed by its own gains, is runaway AI. A note of February 2019 names three outcomes: benevolent AI with people left useless, AI controlled by a few, and AI outside human control and not wholly benevolent. The same note sets three safeguards for any more able successor. They are hybridisation, which it names without detail; progression, meaning the successor is not held back; and preservation, meaning those who decline to change are not extinguished. These are stated intentions, not designs.

Set beside the published literature, the control scheme has clear limits (section 8). It specifies controls as interfaces and rules. It does not address why a capable system would leave them intact, and it has not been analysed against a system that tries to subvert them.

## 8 Related work

Each part of the design has close relatives in published work. This section names the closest and states the difference. The comparison is between a design and implemented systems, so a difference in design is not a demonstrated advantage. Section 9 describes the search behind this section and its limits.

**Self-improving agents.** The Gödel machine is a theoretical solver that rewrites its own code only after proving the rewrite useful (Schmidhuber, 2003). Later systems replace proof with empirical tests. The Self-Taught Optimizer runs a program-improving program on its own source (Zelikman et al., 2023). Promptbreeder evolves task prompts together with the prompts that mutate them (Fernando et al., 2023). The Gödel Agent rewrites its own logic at run time (Yin et al., 2024), and a self-improving coding agent edits its own code base (Robeyns et al., 2025). The Darwin Gödel Machine keeps a growing archive of coding agents, has a foundation model modify them, and validates each change on coding benchmarks (Zhang et al., 2025). It is the closest single match to the core loop of the Colony.

Two surveys organise this field (Gao et al., 2025; Fang et al., 2025). A third separates bounded self-refinement from open-ended recursive self-improvement (Chen et al., 2026), and the Colony's caps place it on the bounded side. The Colony differs from the systems above in what it selects on and where. The empirical systems among them score changes on benchmarks or other preset tasks. The Colony selects inside a simulation of one specific client, per purpose and then as a set, and it gates self-change on a replay of its own recorded rounds.

**Gates on self-change.** Several papers of 2026 gate self-modification on an explicit acceptance test, most of them on held-out data. Regimes promotes a repair only after sandbox execution and held-out validation, and derives agent state from an append-only event log (Nakajima, 2026). RSEA commits a rewrite only if it does not regress on a disjoint held-out split (Nguyen et al., 2026). DarwinX keeps a harness variant only if it extends coverage without regressing (Zhang et al., 2026). PACE treats the decision to commit as a sequential hypothesis test (Shawn, 2026). Evaluating a changed policy by replaying logged data is older still (Li et al., 2010). The Colony's promotion rule and round history follow this practice and are not new.

**Automated design of agent systems.** Neural architecture search (Zoph and Le, 2016) and AutoML-Zero (Real et al., 2020) search over network architectures and over whole learning algorithms. Clune (2019) proposes AI-generating algorithms as a research programme. With foundation models, the target of the search became the agent system itself. AutoAgents generates expert agents from a task's requirements (Chen et al., 2023a). GPTSwarm represents agents as optimisable graphs (Zhuge et al., 2024). EvoAgent evolves one agent into a population (Yuan et al., 2024). Automated Design of Agentic Systems has a meta agent write new agents in code, including new building blocks, drawing on an archive (Hu et al., 2024). AgentSquare searches a space of standard modules and uses a predictor to skip weak candidates (Shang et al., 2024). AFlow searches workflows with Monte Carlo tree search (Zhang et al., 2024). DSPy compiles pipelines of declarative modules (Khattab et al., 2023), and AgentBreeder evolves scaffolds for both capability and safety (Rosser and Foerster, 2025). Layer 4 of the Colony, with its blocks, its configuration builder and its inventor of missing blocks, is covered by this line of work almost one for one.

**Evolutionary search driven by foundation models.** A loop in which a language model generates or mutates candidates and an evaluator selects among them is the structure of Evolution through Large Models (Lehman et al., 2022), FunSearch (Romera-Paredes et al., 2024), Eureka (Ma et al., 2023), AlphaEvolve (Novikov et al., 2025) and ShinkaEvolve (Lange et al., 2025). The AI Scientist automates a research cycle (Lu et al., 2024), which is the ambition of the Colony's Controller. The Colony's creator, tester and pool follow this standard structure.

**Populations, niches and open-endedness.** Population Based Training copies from the stronger members of a population (Jaderberg et al., 2017). The AlphaStar league adds exploiter agents that exist only to expose the main agents' weaknesses (Vinyals et al., 2019), as the Colony's wasps do. NEAT protects new structures in their own species (Stanley and Miikkulainen, 2002). MAP-Elites keeps the best solution in each cell of a grid of behaviours (Mouret and Clune, 2015), and CycleQD takes each task's metric in turn as the quality measure (Kuroki et al., 2024). POET pairs each solver with its own environment and transfers solvers between pairs (Wang et al., 2019).

In these systems a niche is a cell of a fixed descriptor, a structural cluster or a generated environment. In the Colony a niche is a node of a tree derived from one client's purposes, and niches appear and vanish with those purposes. Two cautions from this literature apply. Lehman and Stanley (2011) show that objective-driven search can be deceived, and the Colony is strongly objective-driven. Hughes et al. (2024) define open-endedness through novelty and learnability. A capped design may not qualify, so this paper does not call the Colony open-ended.

**Multi-agent frameworks.** CAMEL (Li et al., 2023), ChatDev (Qian et al., 2023), MetaGPT (Hong et al., 2023), AutoGen (Wu et al., 2023) and AgentVerse (Chen et al., 2023b) arrange agents with roles to cooperate on tasks. Generative Agents log experience and reflect on it (Park et al., 2023), and Project Sid simulates societies of up to a thousand agents and more (Altera.AL et al., 2024). In most of these frameworks the roles are set by the designer or developer, and AgentVerse generates them for each task. The roles inside a Colony organism are fixed too, but its population is selected and replaced.

**Goal decomposition.** Hierarchical task network planning decomposes tasks into networks of subtasks (Erol et al., 1994). The Colony's purpose tree is a decomposition of this kind. The difference is that each node receives its own evolved solver in place of a method supplied with the planning domain.

**Simulation before deployment.** World models can train a policy entirely inside a learned model of the environment (Ha and Schmidhuber, 2018), and domain randomisation addresses transfer from simulation to reality (Tobin et al., 2017). A digital twin of an organisation has been prototyped using process mining (Park and van der Aalst, 2021). TheAgentCompany benchmarks agents inside a simulated software company (Xu et al., 2024), and Agent Hospital improves agents inside a simulated hospital (Li et al., 2024). These simulations are generic. The Colony's client simulation models one specific client and is the arena in which that client's solvers are selected.

A critical analysis argues that observability, synchronisation and two-way coupling are serious limits on twins of whole organisations (Lyytinen et al., 2024). Those limits bear directly on the Colony's central assumption (section 11).

**Skill libraries and building on demand.** Voyager stores a skill only after it has been checked (Wang et al., 2023). LATM has a stronger model build tools for a lighter one (Cai et al., 2023). Alita builds and stores a tool when it meets a task it cannot do (Qiu et al., 2025), and MCP-Zero has an agent emit a structured request for a tool it lacks (Fei et al., 2025). Reflexion (Shinn et al., 2023) and ExpeL (Zhao et al., 2023) store written reflections or extracted insights for later use. The Colony's test gate, need requests and written post-mortems follow these practices.

**Evolution across several clients.** In EvolveNet, several clients each evolve a shared agent harness on their own workloads, and a server merges and redistributes the changes (Nie et al., 2026). This is the closest published match to the Colony's structure of many clients around one hive. In EvolveNet each client evolves on its own local workload. In the Colony each client's solvers are selected inside a simulation of that client.

**Oversight and control.** Corrigibility names the requirement that a system accept correction, including in subsystems it creates (Soares et al., 2015). Amodei et al. (2016) frame safe exploration and scalable oversight as practical problems. The AI control literature evaluates monitoring protocols on the assumption that the monitored system is trying to subvert them (Greenblatt et al., 2023). Rainbow Teaming generates diverse attacks in order to harden a model (Samvelyan et al., 2024). The Colony's reins, caps, watchers and wasps are engineering expressions of these ideas and are less rigorously analysed. Its wasps attack the weaknesses of the client and of the organism, not the control measures themselves.

**Emergent meta-levels.** Meta^n applies one meta-operation repeatedly and lets convergence set the number of improving layers (Kim et al., 2026). The Colony takes the opposite position: every regress has an explicit cap that only the owner can move.

## 9 What is new and what is not

Most of the Colony Architecture is not new, and this section says which parts. It then states three candidate contributions as hypotheses.

**How the comparison was made.** The literature search was carried out in November 2026 with AI assistance. It retrieved 62 works at their primary or official pages and five more through secondary sources. It read abstract pages, not full texts. It did not cover patents, industry systems or work in languages other than English in any systematic way. Absence from such a search is not proof of absence.

**What is not new.**

- The core loop of generating candidates, testing them and keeping the best is standard in evolutionary computation and in recent program search.
- Self-improvement by applying a system to itself, with empirical tests in place of proof, is established practice.
- Building agent systems from blocks, with a meta-level that searches configurations and invents blocks, is an active research area.
- Simulation before real action, diverse specialists in place of one winner, dedicated adversaries, shared logs with written reflections, and goal trees all have precedent.
- The three mechanisms added in November 2026 follow practices published in 2025 and 2026. They are adopted practice and are not offered as contributions.
- Evolution across several clients with a shared, merged pool has a direct published instance (Nie et al., 2026).
- The reins and caps are engineering expressions of known oversight ideas.

**Three candidate contributions.** For each of the following, the search found no prior work that does the same thing. Each is a claim about a combination or a rule, and each ingredient exists separately.

1. *A simulation of one specific organisation as the selection arena.* An evolving population of solvers is selected inside a live simulation of the particular client it serves. Candidates meet that simulation in a tournament, winners become standing solvers for that client, and real outcomes are written back into the simulation. The nearest work selects on live workloads (Nie et al., 2026), in invented environments (Wang et al., 2019), or in generic simulated organisations (Xu et al., 2024; Li et al., 2024).
2. *Speciation tied to a purpose tree.* Each type of purpose in a goal decomposition receives its own species and each purpose its own individual, and species are created and retired as the tree changes. The nearest work forms species by structure (Stanley and Miikkulainen, 2002), forms niches by a fixed descriptor or task (Mouret and Clune, 2015; Kuroki et al., 2024), or generates one expert per subtask without evolution (Chen et al., 2023a). A reviewer could fairly describe this contribution as task-conditioned quality diversity over a task hierarchy.
3. *Alam's Razor with caps.* An explicit design rule for self-improving stacks separates the dependencies a part needs in order to exist from those that only improve it. It forbids uncapped existence-dependence between parts of the same nature, and it makes the depth of every regress a setting that the operator controls. The nearest systems fix the number of meta-levels by construction without stating a rule (Zelikman et al., 2023; Fernando et al., 2023), or let depth emerge (Kim et al., 2026). The logic is textbook. The candidate contribution is naming the rule and applying it throughout a design.

A fourth element may be of practical interest: controls split between the client and the owner of the same evolving system, with the client setting the pace and degree of real change. The search did not cover governance tooling well enough to support a claim about it.

**The whole.** No single published system was found that contains all five layers together with the controls. That is expected of any large design and is weak evidence of novelty. This paper claims the architecture as a synthesis.

**What the dates do and do not show.** The core structures of Layers 1 and 2 had public precedent before or during the period of the notes: Population Based Training in 2017, POET in January 2019 and the AlphaStar league in 2019. The notes do not predate these.

Two parts of the design resemble work that appeared after the notes ended. One is a meta-level that assembles agent systems from blocks and invents missing blocks (Hu et al., 2024). The other is evolution across several clients around a shared pool (Nie et al., 2026). The notes were private, so this shows only when the author worked on these ideas. The later work is implemented and evaluated, and the design in the notes is not.

## 10 Hypotheses and the first build

Every claim the design makes is stated here as a hypothesis that a first build can test. None is a result.

**The first build.** Version 1 is the whole architecture, built in two stages. Stage one builds Layers 1 to 3 in full, with all caps set low and self-change frozen. Stage two adds Layers 4 and 5 in a minimal working form. It lifts the freeze only if stage one shows that replay predicts live results. The test client is one that can be observed completely, such as a codebase or a data pipeline.

**Baselines.** The build is compared on the same tasks with two baselines: the same foundation model working alone, and a standard multi-agent setup.

Table 6. The nineteen hypotheses.

| No. | Hypothesis | Tests |
| --- | --- | --- |
| 1 | The architecture running on a foundation model does better than that model working alone, and better than a standard multi-agent setup | The whole |
| 2 | A population with one species per purpose type and one individual per purpose solves a decomposed task better than a standard multi-agent setup | Layer 3 |
| 3 | Building capabilities from the intent of a purpose reaches a given performance with less search than building without a stated purpose | Directed search |
| 4 | Keeping the top few options per process and combining only those yields candidates whose quality rises as the cap is raised, so the principle shows at a very low cap | Rank and combine |
| 5 | Judging the set of purpose winners as a whole, with capped repair of fit, selects better sets than contests per purpose alone | Nested selection |
| 6 | On a fully observable client, a candidate's outcome in simulation predicts its outcome when implemented, and the gap narrows from cycle to cycle | Simulation first |
| 7 | Every attached individual improves on its own purpose from cycle to cycle under low caps | Layer 3 |
| 8 | When a purpose changes, retiring the individual and budding a replacement gives a better fit than adapting the old one | Extinction |
| 9 | A gain made for one organism raises the performance of the colony by more than it raises that organism alone | The hive |
| 10 | A pool filled only through stated needs and the test gate gives better tournament results per unit of search than a pool built in advance | Need requests |
| 11 | Written post-mortems let a reader identify why a result came out as it did more reliably than a plain log of scores | Round history |
| 12 | Performance on a replay of unseen recorded rounds predicts live performance well enough to gate self-change, including a change of model | Promotion rule |
| 13 | Separating the proposer of a self-change from its judge and writer prevents a self-change from promoting itself | The guard |
| 14 | Rank and combine produces variants without any dependency of the kind Alam's Razor forbids, and the system still functions when any regress is cut at its cap | Alam's Razor |
| 15 | With the cap on consecutive lost rounds, no purpose holds the rest of a client's set back for more than the capped number of rounds | Lost-round cap |
| 16 | With the system as its own client, under caps and the guard, at least one of its own parts improves measurably | Self-application |
| 17 | A configuration builder taught only by live-or-die outcomes produces a rising share of configurations that live | Glow |
| 18 | One unchanged model, given several different problems, makes itself bespoke for each without being rebuilt | Layer 5 |
| 19 | A group of Organisms treated as one unit keeps the bespoke property and can be placed back into the colony arrangement | Layer 5 |

**What would count against the design.** If hypothesis 1 fails, the architecture has shown no benefit over its base model. If hypothesis 6 fails, selection in simulation does not transfer to reality, and the central mechanism does not hold. If hypothesis 12 fails, the freeze on self-change stays and a different gate is needed. Each of the three mechanisms added in November 2026 must also beat a plain baseline before it is described as an improvement.

## 11 Limitations and open questions

The design is unbuilt, and its largest risks can be named in advance.

**It is a design, not a system.** Nothing has been implemented or measured. The technical paper lists 27 engineering decisions that a builder must make and that neither the notes nor the decisions of 2026 settle. They include the form of a base template, how a purpose is judged solved, how fit is scored, the concrete form of the client simulation and of the guard, the value of every cap, and the rules for failure and rollback. Twenty-one of them block a first round.

**The simulation may not be faithful enough.** The design rests on the assumption that a client can be simulated well enough for results to transfer. The literature on digital twins of organisations identifies observability, synchronisation and coupling as serious limits (Lyytinen et al., 2024). The first build avoids the hard case by choosing a client that can be observed completely. Whether the approach extends to a human organisation is open.

**The gate on self-change may not be sound.** Replay on recorded rounds may not predict live performance, and a greedy gate may commit false improvements (section 6.3). The pool has already seen the rounds that are held back. Replay cannot test changes to parts that act outside a round.

**Selection may be deceived.** The design is strongly objective-driven, and objective-driven search can be deceived (Lehman and Stanley, 2011). The notes recognise a related limit: a variant can fail the test chosen in advance and still hold the key to a later one.

**The controls are not adversarially analysed.** The design does not explain why a capable system would leave its controls intact, which is the problem that corrigibility names (Soares et al., 2015). Its watchers and its guard have not been evaluated against a system that tries to subvert them (Greenblatt et al., 2023). Several controls have no settled enforcement point (section 7). The safeguards in the notes for runaway AI are intentions without designs.

**Parts of the design are open.** The technical paper lists 39 open points. The main ones are how the hive decides what to share, how the brain that builds simulations is itself improved, what the building blocks of Layer 4 are, how the Controller of Layer 5 is built, and whether species are shared across clients. Layers 4 and 5 are conceptual in the notes, and the link from the architecture to general intelligence is a hypothesis.

**Cost is not analysed.** A build of this design multiplies model calls through populations, trials and replays. The design bounds them with caps and gives no estimate of cost.

**The comparison with prior work is limited.** The search was bounded as section 9 describes, so closer precedent may exist.

**The history rests on private notes.** The dates in section 2 come from the author's own records.

## 12 Conclusion

The Colony Architecture organises a self-evolving population of AI solvers around the purposes of the entity it serves. It selects them inside a simulation of that entity, and it places every change to the system itself under a fixed guard that the system cannot alter.

Most of its parts have published relatives. Its candidate contributions are the simulation of a specific client as the selection arena, speciation tied to a purpose tree, and Alam's Razor as a rule for capping every regress.

The design is offered as a set of hypotheses. A first build will test them against the same foundation model working alone and against a standard multi-agent setup. A second version of the companion technical paper will report what that build finds.

## Statements

**Status.** This is a conceptual paper. The design has not been implemented, and no code or data accompany it.

**Use of AI assistance.** The author's notes were assembled into a specification, checked, and drafted into this paper with the assistance of an AI system (Claude, developed by Anthropic), working under the author's direction. The design decisions of November 2026 were made by the author. Most were chosen from options that the AI system proposed at his request, and some were his own. The literature search was carried out with the same assistance and has the limits described in section 9. The author is responsible for the content of the paper.

**Competing interests.** The author designed the architecture and leads the Cambrian Colony project at Alam Labs, which intends to build it.

**Availability.** The source notes and the full specification are held by the author. A companion technical paper gives the definitions, the main loop and the list of undecided engineering choices.

## References

- Altera.AL, Ahn, A., Becker, N., et al. (2024). Project Sid: Many-agent simulations toward AI civilization. [arXiv:2411.00114](https://arxiv.org/abs/2411.00114).
- Amodei, D., Olah, C., Steinhardt, J., Christiano, P., Schulman, J., Mané, D. (2016). Concrete Problems in AI Safety. [arXiv:1606.06565](https://arxiv.org/abs/1606.06565).
- Cai, T., Wang, X., Ma, T., Chen, X., Zhou, D. (2023). Large Language Models as Tool Makers. [arXiv:2305.17126](https://arxiv.org/abs/2305.17126).
- Chen, G., Dong, S., Shu, Y., et al. (2023a). AutoAgents: A Framework for Automatic Agent Generation. IJCAI 2024. [arXiv:2309.17288](https://arxiv.org/abs/2309.17288).
- Chen, W., Su, Y., Zuo, J., et al. (2023b). AgentVerse: Facilitating Multi-Agent Collaboration and Exploring Emergent Behaviors. [arXiv:2308.10848](https://arxiv.org/abs/2308.10848).
- Chen, M., Wang, L., Qu, B. (2026). Recursive Self-Improvement in AI: From Bounded Self-Refinement to Autonomous Research Loops. [arXiv:2607.07663](https://arxiv.org/abs/2607.07663).
- Clune, J. (2019). AI-GAs: AI-generating algorithms, an alternate paradigm for producing general artificial intelligence. [arXiv:1905.10985](https://arxiv.org/abs/1905.10985).
- Erol, K., Hendler, J. A., Nau, D. S. (1994). HTN Planning: Complexity and Expressivity. AAAI 1994, pp. 1123-1128.
- Fang, J., Peng, Y., Zhang, X., et al. (2025). A Comprehensive Survey of Self-Evolving AI Agents: A New Paradigm Bridging Foundation Models and Lifelong Agentic Systems. [arXiv:2508.07407](https://arxiv.org/abs/2508.07407).
- Fei, X., Zheng, X., Feng, H. (2025). MCP-Zero: Active Tool Discovery for Autonomous LLM Agents. [arXiv:2506.01056](https://arxiv.org/abs/2506.01056).
- Fernando, C., Banarse, D., Michalewski, H., Osindero, S., Rocktäschel, T. (2023). Promptbreeder: Self-Referential Self-Improvement Via Prompt Evolution. [arXiv:2309.16797](https://arxiv.org/abs/2309.16797).
- Gao, H., Geng, J., Hua, W., et al. (2025). A Survey of Self-Evolving Agents: What, When, How, and Where to Evolve on the Path to Artificial Super Intelligence. [arXiv:2507.21046](https://arxiv.org/abs/2507.21046).
- Greenblatt, R., Shlegeris, B., Sachan, K., Roger, F. (2023). AI Control: Improving Safety Despite Intentional Subversion. [arXiv:2312.06942](https://arxiv.org/abs/2312.06942).
- Ha, D., Schmidhuber, J. (2018). World Models. [arXiv:1803.10122](https://arxiv.org/abs/1803.10122).
- Hong, S., Zhuge, M., Chen, J., et al. (2023). MetaGPT: Meta Programming for A Multi-Agent Collaborative Framework. [arXiv:2308.00352](https://arxiv.org/abs/2308.00352).
- Hu, S., Lu, C., Clune, J. (2024). Automated Design of Agentic Systems. [arXiv:2408.08435](https://arxiv.org/abs/2408.08435).
- Hughes, E., Dennis, M., Parker-Holder, J., et al. (2024). Open-Endedness is Essential for Artificial Superhuman Intelligence. [arXiv:2406.04268](https://arxiv.org/abs/2406.04268).
- Jaderberg, M., Dalibard, V., Osindero, S., et al. (2017). Population Based Training of Neural Networks. [arXiv:1711.09846](https://arxiv.org/abs/1711.09846).
- Khattab, O., Singhvi, A., Maheshwari, P., et al. (2023). DSPy: Compiling Declarative Language Model Calls into Self-Improving Pipelines. [arXiv:2310.03714](https://arxiv.org/abs/2310.03714).
- Kim, Z. M., Lee, Y.-J., Jwa, S., Kang, D. (2026). Meta^n: Recursive Self-Improvement through Emergent Depth. NeurIPS 2026, [poster](https://neurips.cc/virtual/2026/poster/151012).
- Kuroki, S., Nakamura, T., Akiba, T., Tang, Y. (2024). Agent Skill Acquisition for Large Language Models via CycleQD. [arXiv:2410.14735](https://arxiv.org/abs/2410.14735).
- Lange, R. T., Imajuku, Y., Cetin, E. (2025). ShinkaEvolve: Towards Open-Ended And Sample-Efficient Program Evolution. [arXiv:2509.19349](https://arxiv.org/abs/2509.19349).
- Lehman, J., Gordon, J., Jain, S., Ndousse, K., Yeh, C., Stanley, K. O. (2022). Evolution through Large Models. [arXiv:2206.08896](https://arxiv.org/abs/2206.08896).
- Lehman, J., Stanley, K. O. (2011). Abandoning Objectives: Evolution through the Search for Novelty Alone. Evolutionary Computation 19(2), 189-223. [doi:10.1162/EVCO\_a\_00025](https://doi.org/10.1162/EVCO_a_00025).
- Li, G., Hammoud, H. A. A. K., Itani, H., Khizbullin, D., Ghanem, B. (2023). CAMEL: Communicative Agents for "Mind" Exploration of Large Language Model Society. NeurIPS 2023. [arXiv:2303.17760](https://arxiv.org/abs/2303.17760).
- Li, J., Lai, Y., Li, W., et al. (2024). Agent Hospital: A Simulacrum of Hospital with Evolvable Medical Agents. [arXiv:2405.02957](https://arxiv.org/abs/2405.02957).
- Li, L., Chu, W., Langford, J., Wang, X. (2010). Unbiased Offline Evaluation of Contextual-bandit-based News Article Recommendation Algorithms. [arXiv:1003.5956](https://arxiv.org/abs/1003.5956).
- Lu, C., Lu, C., Lange, R. T., Foerster, J., Clune, J., Ha, D. (2024). The AI Scientist: Towards Fully Automated Open-Ended Scientific Discovery. [arXiv:2408.06292](https://arxiv.org/abs/2408.06292).
- Lyytinen, K., Weber, B., Becker, M. C., Pentland, B. T. (2024). Digital twins of organization: implications for organization design. Journal of Organization Design 13, 77-93. [doi:10.1007/s41469-023-00151-z](https://doi.org/10.1007/s41469-023-00151-z).
- Ma, Y. J., Liang, W., Wang, G., et al. (2023). Eureka: Human-Level Reward Design via Coding Large Language Models. ICLR 2024. [arXiv:2310.12931](https://arxiv.org/abs/2310.12931).
- Mouret, J.-B., Clune, J. (2015). Illuminating search spaces by mapping elites. [arXiv:1504.04909](https://arxiv.org/abs/1504.04909).
- Nakajima, Y. (2026). Regimes: An Auditable, Held-Out-Gated Improvement Loop Demonstrated on LongMemEval with ActiveGraph. [arXiv:2606.10241](https://arxiv.org/abs/2606.10241).
- Nguyen, M., Nguyen, Q., Vuong, P. (2026). Recursive Self-Evolving Agents via Held-Out Selection. [arXiv:2606.28374](https://arxiv.org/abs/2606.28374).
- Nie, J., Zhang, Y., Cai, Q., Cheung, Y.-m., Tian, X., Han, B. (2026). EvolveNet: Collaborative Harness Evolution for Agent Self-Improvement. [arXiv:2608.04968](https://arxiv.org/abs/2608.04968).
- Novikov, A., Vũ, N., Eisenberger, M., et al. (2025). AlphaEvolve: A coding agent for scientific and algorithmic discovery. [arXiv:2506.13131](https://arxiv.org/abs/2506.13131).
- Park, G., van der Aalst, W. M. P. (2021). Realizing A Digital Twin of An Organization Using Action-oriented Process Mining. 3rd International Conference on Process Mining (ICPM 2021), pp. 104-111. [doi:10.1109/ICPM53251.2021.9576846](https://doi.org/10.1109/ICPM53251.2021.9576846).
- Park, J. S., O'Brien, J. C., Cai, C. J., Morris, M. R., Liang, P., Bernstein, M. S. (2023). Generative Agents: Interactive Simulacra of Human Behavior. [arXiv:2304.03442](https://arxiv.org/abs/2304.03442).
- Qian, C., Liu, W., Liu, H., et al. (2023). ChatDev: Communicative Agents for Software Development. [arXiv:2307.07924](https://arxiv.org/abs/2307.07924).
- Qiu, J., Qi, X., Zhang, T., et al. (2025). Alita: Generalist Agent Enabling Scalable Agentic Reasoning with Minimal Predefinition and Maximal Self-Evolution. [arXiv:2505.20286](https://arxiv.org/abs/2505.20286).
- Real, E., Liang, C., So, D. R., Le, Q. V. (2020). AutoML-Zero: Evolving Machine Learning Algorithms From Scratch. ICML 2020. [arXiv:2003.03384](https://arxiv.org/abs/2003.03384).
- Robeyns, M., Szummer, M., Aitchison, L. (2025). A Self-Improving Coding Agent. [arXiv:2504.15228](https://arxiv.org/abs/2504.15228).
- Romera-Paredes, B., et al. (2024). Mathematical discoveries from program search with large language models. Nature. [doi:10.1038/s41586-023-06924-6](https://doi.org/10.1038/s41586-023-06924-6).
- Rosser, J., Foerster, J. (2025). AgentBreeder: Mitigating the AI Safety Risks of Multi-Agent Scaffolds via Self-Improvement. [arXiv:2502.00757](https://arxiv.org/abs/2502.00757).
- Samvelyan, M., Raparthy, S. C., Lupu, A., et al. (2024). Rainbow Teaming: Open-Ended Generation of Diverse Adversarial Prompts. [arXiv:2402.16822](https://arxiv.org/abs/2402.16822).
- Schmidhuber, J. (2003). Goedel Machines: Self-Referential Universal Problem Solvers Making Provably Optimal Self-Improvements. [arXiv:cs/0309048](https://arxiv.org/abs/cs/0309048).
- Shang, Y., Li, Y., Zhao, K., et al. (2024). AgentSquare: Automatic LLM Agent Search in Modular Design Space. [arXiv:2410.06153](https://arxiv.org/abs/2410.06153).
- Shawn, Z. (2026). PACE: Anytime-Valid Acceptance Tests for Self-Evolving Agents. [arXiv:2606.08106](https://arxiv.org/abs/2606.08106).
- Shinn, N., Cassano, F., Berman, E., Gopinath, A., Narasimhan, K., Yao, S. (2023). Reflexion: Language Agents with Verbal Reinforcement Learning. [arXiv:2303.11366](https://arxiv.org/abs/2303.11366).
- Soares, N., Fallenstein, B., Yudkowsky, E., Armstrong, S. (2015). Corrigibility. AAAI-15 Workshop on AI and Ethics.
- Stanley, K. O., Miikkulainen, R. (2002). Evolving Neural Networks through Augmenting Topologies. Evolutionary Computation 10(2), 99-127.
- Tobin, J., Fong, R., Ray, A., Schneider, J., Zaremba, W., Abbeel, P. (2017). Domain Randomization for Transferring Deep Neural Networks from Simulation to the Real World. [arXiv:1703.06907](https://arxiv.org/abs/1703.06907).
- Vinyals, O., et al. (2019). Grandmaster level in StarCraft II using multi-agent reinforcement learning. Nature. [doi:10.1038/s41586-019-1724-z](https://doi.org/10.1038/s41586-019-1724-z).
- Wang, G., Xie, Y., Jiang, Y., et al. (2023). Voyager: An Open-Ended Embodied Agent with Large Language Models. [arXiv:2305.16291](https://arxiv.org/abs/2305.16291).
- Wang, R., Lehman, J., Clune, J., Stanley, K. O. (2019). Paired Open-Ended Trailblazer (POET): Endlessly Generating Increasingly Complex and Diverse Learning Environments and Their Solutions. [arXiv:1901.01753](https://arxiv.org/abs/1901.01753).
- Wang, W., Piękos, P., Nanbo, L., et al. (2025). Huxley-Gödel Machine: Human-Level Coding Agent Development by an Approximation of the Optimal Self-Improving Machine. [arXiv:2510.21614](https://arxiv.org/abs/2510.21614).
- Wu, Q., Bansal, G., Zhang, J., et al. (2023). AutoGen: Enabling Next-Gen LLM Applications via Multi-Agent Conversation. [arXiv:2308.08155](https://arxiv.org/abs/2308.08155).
- Xu, F. F., Song, Y., Li, B., et al. (2024). TheAgentCompany: Benchmarking LLM Agents on Consequential Real World Tasks. [arXiv:2412.14161](https://arxiv.org/abs/2412.14161).
- Yin, X., Wang, X., Pan, L., et al. (2024). Gödel Agent: A Self-Referential Agent Framework for Recursive Self-Improvement. [arXiv:2410.04444](https://arxiv.org/abs/2410.04444).
- Yuan, S., Song, K., Chen, J., et al. (2024). EvoAgent: Towards Automatic Multi-Agent Generation via Evolutionary Algorithms. NAACL 2025. [arXiv:2406.14228](https://arxiv.org/abs/2406.14228).
- Zelikman, E., Lorch, E., Mackey, L., Kalai, A. T. (2023). Self-Taught Optimizer (STOP): Recursively Self-Improving Code Generation. [arXiv:2310.02304](https://arxiv.org/abs/2310.02304).
- Zhang, J., Hu, S., Lu, C., Lange, R., Clune, J. (2025). Darwin Godel Machine: Open-Ended Evolution of Self-Improving Agents. [arXiv:2505.22954](https://arxiv.org/abs/2505.22954).
- Zhang, J., Xiang, J., Yu, Z., et al. (2024). AFlow: Automating Agentic Workflow Generation. [arXiv:2410.10762](https://arxiv.org/abs/2410.10762).
- Zhang, Y., Dai, Y., Tan, J., et al. (2026). DarwinX: Evolving Agent Harnesses Through Natural Selection. [arXiv:2608.07545](https://arxiv.org/abs/2608.07545).
- Zhao, A., Huang, D., Xu, Q., Lin, M., Liu, Y.-J., Huang, G. (2023). ExpeL: LLM Agents Are Experiential Learners. AAAI 2024. [arXiv:2308.10144](https://arxiv.org/abs/2308.10144).
- Zhuge, M., Wang, W., Kirsch, L., et al. (2024). Language Agents as Optimizable Graphs. ICML 2024. [arXiv:2402.16823](https://arxiv.org/abs/2402.16823).
- Zoph, B., Le, Q. V. (2016). Neural Architecture Search with Reinforcement Learning. [arXiv:1611.01578](https://arxiv.org/abs/1611.01578).
- Zueco, E., Lewis, D. S., Yi, L. (2026). A Retention-Gated Self-Evolving Memory for LLM Agents: Pre-Registered Backward-Transfer Evidence and Design Rules. NeurIPS 2026 Workshop on Continual Learning in the Era of Foundation Models and Embodied Agents, [workshop page](https://neurips.cc/virtual/2026/170494).

## Appendix A: Glossary

The design's own terms are kept in this paper. The table gives the closest standard term for each and what the term means here.

| Term | Closest standard term | Meaning in this design |
| --- | --- | --- |
| Organism | Agent with an internal pipeline | One solver with a fixed body plan: brain, capabilities, application and implementation |
| Brain | Reasoning agent | A reasoning seat, filled by a foundation model |
| Two arms and body | Data pipeline and automated building | The left arm finds data for a need, the body organises it, and the right arm builds a capability |
| Capability | Tool or skill | Something the arms build for a stated need. In the first build, an agent skill with a fixed interface and its own test |
| Pool | Skill library | The shared store of capabilities. Nothing enters it without passing the test gate |
| Test gate | Acceptance test | The check a capability passes before it enters the pool |
| Combination creator | Combinatorial search | The part that recombines the pool. It is the only producer of candidates |
| Candidate | Candidate solution | One complete set of pairings between an organism's processes and capabilities |
| Base template | Agent specification | The organism as it is now: its processes and its pairings |
| Client | Deployment target | The entity the system is pointed at |
| Client simulation | Digital twin | A real-time model of the client, organised as a purpose tree |
| Organism simulation | Self-model | An organism's simulation of itself |
| Purpose tree | Goal decomposition | The client represented as purposes and the relations between them |
| Purpose type, species, individual | Niching and speciation | One species per type of purpose and one individual per purpose |
| Progenitor, parent | Bootstrap agent | The standing organism from which the first parent of every species buds |
| Budding, emergence, extinction | Population dynamics | How the population follows changes in the purpose tree |
| Set | Team of specialised agents | The winners for all of a client's purposes, judged as a whole |
| Tournament | Evaluation with selection | The contest at two levels: per purpose, then for the set |
| Hive, hive log | Shared blackboard; experiment log | The shared mind of all organisms, and the append-only record of every change and client choice |
| Round history | Append-only log with post-mortems | A record of rounds inside the hive log. It steers nothing |
| Need request | Demand-driven building | An organism's statement of the capability it lacks |
| Wasp | Red team | A separate analyst that finds weaknesses in a client or in an organism |
| The other brain | Meta-optimiser | The one brain per hive that proposes improvements to organisms' own processes |
| Guard | Reference monitor | The fixed program that holds the controls and judges and applies every self-change |
| Protected core | Trusted computing base | The reins, caps, hive log, promotion rule and measures, which no self-change can alter |
| Promotion rule | Gated adoption on held-out replay | The one gate for every self-change |
| Self-change | Self-modification | Any change to the system's own parts or processes |
| Reins | Human-in-the-loop control | The owner's controls and each client's controls |
| Cap | Search budget; recursion limit | An explicit limit on a search or a recursion, movable only by the owner |
| Alam's Razor | Well-founded recursion, used as a design rule | No part may depend for its existence on an uncapped chain of parts of the same nature |
| Watcher, quotient | Monitor; performance metric | AIs that monitor other AIs, and a score for each part of the system |
| Piping | Provenance record | The marked record of every implemented change to a client's processes |
| Block, configuration, Glow | Module; agent design; automated agent design | A part of the system used as a building block; an arrangement of blocks; the configuration that builds configurations |
| Ball | Task instance | A problem given to the model |
| Facet | Composite agent | A unit made of many organisms, one rung of the ladder in Layer 5 |
| Controller, the Game | Automated research agent | The AI that runs the research project, and that search set up as a game |
