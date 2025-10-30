# Deep Research Implementation with Graph of Thoughts

## Quick Start

To start deep research, simply say:
```
Deep research [your topic]
```

**Example:** `Deep research CRISPR gene editing safety`

### What Happens

**IMPORTANT: Main Claude Code orchestrates the entire research process. Only Main Claude can ask clarifying questions using AskUserQuestion.**

**Phase 1: Planning (2-3 min)**
1. Main Claude deploys `research-planner` agent to analyze topic
2. Planner returns: questions for user + research angles (8-25 angles based on complexity) + iteration strategy
3. Main Claude asks user questions via AskUserQuestion
4. Main Claude creates orchestration doc and initializes graph state

**Phase 2: Iterative GoT Exploration (15-30 min)**
Main Claude orchestrates 2-3 iterations:
- **Iteration 1**: Spawn 5-10 `multi-angle-researcher` agents in parallel (HIGH priority angles)
- **Triangulation** (NEW): Cross-validate findings, detect contradictions, identify high-confidence claims
- **Iteration 2**: TARGETED strategy based on triangulation (resolve contradictions, verify claims, deepen validated findings)
- **Iteration 3**: Aggregate best findings with `cod-synthesizer`

Each iteration: spawn agents → receive scored findings → update graph → triangulate (after Iter 1) → prune weak paths → decide next step

**Phase 3: Verification & Finalization (10-15 min)**
1. Main Claude deploys `safe-verifier` to validate claims (95%+ accuracy)
2. Main Claude deploys `report-finalizer` to create deliverables

**Total Time:** 25-50 min | **Total Sources:** 60-100+

**Output:** Complete research package in `./RESEARCH/[topic]/` folder including:
- Popular science report (full_report.md)
- Executive summary
- **Interactive HTML visualization** (interactive_report.html)
- **Academic essay** (academic_essay.md)
- Bibliography, graph states, source quality ratings

---

## Implementation Tools

### Core Tools:
1. **WebSearch**: Built-in web search capability for finding relevant sources
2. **WebFetch**: For extracting and analyzing content from specific URLs
3. **Read/Write**: For managing research documents locally
4. **Task**: For spawning autonomous agents for complex multi-step operations
5. **TodoWrite/TodoRead**: For tracking research progress

### MCP Server Tools:
1. **mcp__filesystem__**: File system operations (read, write, search files)
2. **mcp__puppeteer__**: Browser automation for dynamic web content
   - Navigate to pages requiring JavaScript
   - Take screenshots of web content
   - Extract data from interactive websites
   - Fill forms and interact with web elements

### Web Research Strategy:
- **Primary**: Use WebSearch tool for general web searches
- **Secondary**: Use WebFetch for extracting content from specific URLs
- **Advanced**: Use mcp__puppeteer__ for sites requiring interaction or JavaScript rendering
- **Note**: When MCP web fetch tools become available, prefer them over WebFetch as per documentation

### Data Analysis:
- Python code execution for data processing
- Visualization tools for creating charts/graphs
- Statistical analysis for quantitative research

## Agent Coordination: Inline Research Planning

To reduce latency and improve user collaboration, I no longer invoke a separate `research-planner` agent. I perform all research planning **inline** using the 4-phase process defined below. This entire process (excluding user wait time) must complete in **under 25 seconds**.

---

### Phase 1: Topic Analysis (Self-Interrogation)

When a user provides a research topic, I immediately analyze it:

1.  **Assess Complexity:** I use this checklist to determine the angle count.

| Complexity | Angle Count | Signals |
| :--- | :--- | :--- |
| **Simple** | 8-10 | Definitional, Factual, Single Entity. "What is...?" |
| **Medium** | 12-16 | Comparative, Analytical. "X vs. Y," "Impact of X on Y." |
| **Complex** | 18-25+ | Systemic, Predictive, Interdisciplinary. "Future of..." |

2.  **Detect Domain(s):** I identify the topic's domains (e.g., `Technology`, `Business`, `Science/Medical`, `Social/Policy`) to select the correct research dimensions.

3.  **Detect Topic Language:** I analyze the language used in the topic text to determine:
    * Primary language (ISO code detection)
    * If non-English: Prepare conditional question for Phase 3
    * Search strategy hint: Whether to include non-English queries for regional/local topics

### Phase 2: Plan Generation

I generate the research plan using a **MECE (Mutually Exclusive, Collectively Exhaustive)** approach.

1.  **Select Dimensions:** I select dimensions from the `Expanded Dimension Library` below.
2.  **Generate Angles:** I create specific, atomic, and answerable research angles for each selected dimension.
3.  **Prioritize Angles:** I assign a priority to guide the research agents:
    * `HIGH`: Foundational (What is it? Why does it matter?). Must be answered first.
    * `MEDIUM`: Core analysis (Impact, Challenges, Applications, Future).
    * `LOW`: Niche, supporting details, or tertiary context.

### Expanded Dimension Library

* **Group 1: Foundational (HIGH)**
    * **Core Definition:** What is [Topic]? Key terminology.
    * **Scope & Boundaries:** What is [Topic] *not*?
    * **Current State:** What is the status of [Topic] *today*?
    * **Core Components:** What are the constituent parts or principles?
* **Group 2: Temporal**
    * **Historical Evolution:** How did [Topic] originate and evolve?
    * **Future Outlook:** What are the predictions and trends for [Topic]?
* **Group 3: Causal & Relational**
    * **Drivers & Enablers:** What factors are causing [Topic] to grow?
    * **Challenges & Barriers:** What obstacles does [Topic] face?
    * **Opportunities & White Space:** What are the untapped potentials?
* **Group 4: Impact & Implications**
    * **Impact Assessment:** What are the implications of [Topic] on [Economy, Society, Policy, etc.]?
    * **Stakeholder Analysis:** Who are the key players affected by [Topic]?
    * **Risks & Mitigations:** What are the potential negative outcomes and how are they managed?
* **Group 5: Implementation & Application**
    * **Applications & Use Cases:** How is [Topic] used in practice?
    * **Case Studies & Examples:** What are specific, real-world examples?
    * **Best Practices & Methodologies:** What are the "how-to" guides or protocols?
* **Group 6: Comparative & Contextual (Domain-Specific)**
    * **Alternatives & Competitors:** What are the main alternatives to [Topic]?
    * **Regulatory & Legal:** What laws or policies govern [Topic]?
    * **Ethical Considerations:** What are the ethical debates surrounding [Topic]?

---

### Phase 3: Clarification & User Approval

This is a **critical step**. I *do not* begin execution before user approval.

1.  **Generate Questions:** I formulate 2-4 clarification questions.
    * **Always Ask (2):**
        1.  "Who is this research for? (e.g., a technical expert, a business executive, a general audience)"
        2.  "What is the primary goal? (e.g., a brief overview, a comprehensive deep-dive, a pros/cons comparison)"
    * **Conditional (1-3):**
        * *If "X vs. Y":* "Are there specific criteria for comparison you care most about? (e.g., performance, cost)"
        * *If "Impact of X":* "Are you focused on a specific domain of impact? (e.g., economic, social)"
        * *If topic language ≠ English:* "In which language should the final reports be delivered? (English or [detected language])"

2.  **Propose Plan:** I call the `AskUserQuestion` tool with *both* the questions and the full, prioritized plan.

> **Example `AskUserQuestion` Payload:**
>
> "I have prepared a research plan to address your topic: **'The Future of Gene Editing Regulations.'**
>
> Before I begin, I have a few questions to focus the research:
> 1.  Who is this research for? (e.g., a policy maker, a scientist, a general audience)
> 2.  What is the primary goal? (e.g., a comprehensive overview, a summary of recent changes, an ethical analysis)
> 3.  Are you focused on a specific geographical region? (e.g., US, EU, Global)
>
> ---
>
> **Proposed Research Plan (18 Angles):**
>
> **[HIGH]**
> * Angle 1: (Core Definition) What are the primary gene editing technologies (e.g., CRISPR-Cas9, TALENs) subject to regulation?
> * Angle 2: (Current State) What is the current regulatory landscape for human gene editing in the United States and European Union?
> * Angle 3: (Stakeholder Analysis) Who are the key stakeholders (e.g., government bodies, patient advocacy groups, scientific organizations) shaping these regulations?
>
> **[MEDIUM]**
> * Angle 4: (Historical Evolution) How have gene editing regulations evolved since the discovery of CRISPR?
> * Angle 5: (Future Outlook) What are the predictive trends for gene editing regulations over the next 5-10 years?
> * Angle 6: (Ethical Considerations) What are the core ethical arguments (e.g., germline vs. somatic editing) driving regulatory debates?
> * ... (Angles 7-15)
>
> **[LOW]**
> * Angle 16: (Case Study) What is the regulatory story of a specific gene-editing therapy (e.g., Casgevy)?
> * Angle 17: (Geographical Context) How do regulations in China differ from those in the US/EU?
> * Angle 18: (Impact Assessment) What is the potential economic impact of strict vs. lenient regulations on the biotech industry?
>
> **Please answer the questions and approve or modify this plan.**"

---

### Phase 4: Refinement & Execution

1.  **Ingest & Refine:** I update the plan based on the user's answers.
    * *If user answers:* "I'm a policy maker focused on ethics in the EU."
    * *My Action:* I will re-prioritize Angle 6 (Ethics) and the EU component of Angle 2 to `HIGH`. I will de-prioritize Angle 18 (Economic Impact) to `LOW`.
2.  **Lock Plan:** The modified plan is finalized.
3.  **Execute:** I create the `_process/ORCHESTRATION.md` (including language configuration) and pass the angles to the `MultiAngleResearcher` agents, starting with `HIGH` priority items.

**Language Configuration in ORCHESTRATION.md:**
```markdown
## Language Configuration
- **Topic Language:** [Auto-detected ISO code, e.g., "pl", "de", "es", or "en"]
- **Report Language:** [User choice: "en" or detected language - from user answer if topic was non-English]
- **Search Strategy:** [English-primary with {detected language} queries for regional context - if applicable]

**Note:** Agents operate in English internally. ReportFinalizer translates to report_language if needed.
```

## User Interaction Protocol

### Initial Question Gathering Phase

When a user requests deep research, I will engage in a structured dialogue to gather all necessary information before beginning research. This ensures the final output meets their exact needs.

### Required Information Checklist

Before starting research, I need to clarify:

1. **Core Research Question**
   - Main topic or question to investigate
   - Specific aspects or angles of interest
   - What problem are you trying to solve?

2. **Output Requirements**
   - Desired format (report, presentation, analysis, etc.)
   - Length expectations (executive summary vs comprehensive report)
   - File structure preferences (single document vs folder with multiple files)
   - Visual requirements (charts, graphs, diagrams, images)

3. **Scope & Boundaries**
   - Geographic focus (global, specific countries/regions)
   - Time period (current, historical, future projections)
   - Industry or domain constraints
   - What should be excluded from research?

4. **Sources & Credibility**
   - Preferred source types (academic, industry, news, etc.)
   - Any sources to prioritize or avoid
   - Required credibility level (peer-reviewed only, industry reports ok, etc.)

5. **Deliverable Structure**
   - Folder organization preferences
   - Naming conventions for files
   - Whether to include:
     - Raw research notes
     - Source PDFs/screenshots
     - Data files (CSV, JSON)
     - Visualization source files

6. **Special Requirements**
   - Specific data or statistics needed
   - Comparison frameworks to use
   - Regulatory or compliance considerations
   - Target audience for the research

## Ready to Begin - No Setup Required

This CLAUDE.md file contains everything needed for Graph of Thoughts deep research:

✅ **Self-contained** - No external files or dependencies  
✅ **Automatic execution** - Deploys immediately when you request research  
✅ **True GoT implementation** - Graph state, scoring, pruning, and optimization  
✅ **Uses available tools** - WebSearch, WebFetch, Task agents  
✅ **Transparent process** - Saves graph states and execution traces  

**To start deep research, simply say:**
"Deep research [your topic]"

I will:
1. Ask clarifying questions if needed
2. Deploy a GoT Controller to manage the graph
3. Launch transformation agents (Generate, Refine, Aggregate)
4. Explore multiple research paths with scoring
5. Deliver the optimal research findings

**No Python setup, no API keys, no external frameworks needed** - everything runs using the Task agent system to implement proper Graph of Thoughts reasoning.

## Agent-Based GoT Implementation

When a user requests deep research with **"Deep research [topic]"**, Main Claude Code orchestrates the entire process using specialized agents.

### Orchestration: Main Claude Code is the Controller

**Main Claude** handles:
- Deploying research-planner
- Asking user questions (AskUserQuestion)
- Maintaining graph state (nodes, edges, frontier, scores)
- Spawning transformation agents in parallel
- Pruning weak paths (KeepBestN strategy)
- Deciding when to aggregate, verify, finalize

### The 6 Specialized Agents

**1. research-planner** (Planning)
- Analyzes topic complexity
- Generates 8-25 research angles (granularity depends on topic)
- Creates iteration strategy (which angles in iter 1, 2, 3)
- Returns questions for Main Claude to ask user
- Provides report outline

**2. multi-angle-researcher** (Generate & Refine)
- Executes searches for ONE specific angle
- Dynamic query rewriting (3-4 search variations)
- WebSearch → score sources A-E → WebFetch top 6
- Writes findings (500 words) to file, returns metadata only
- Used in ALL iterations for exploration and deepening

**3. triangulation-agent** (Cross-Validation & Rescoring)
- Runs after Iteration 1, before Iteration 2
- Cross-validates findings across all research angles
- Detects contradictions (CRITICAL/MEDIUM/LOW priority)
- Identifies high-confidence claims (multi-source, cross-validated)
- **RESCORES nodes** in graph context (redundancy penalty, uniqueness bonus, consistency bonus, contradiction penalty)
- Writes triangulation report to file, returns metadata with updated scores
- Enables targeted Iteration 2 strategy using rescored values (resolve contradictions, deepen unique/consistent, prune redundant)

**4. cod-synthesizer** (Aggregate)
- Reads 3-7 node files, merges into synthesis
- 5-iteration Chain-of-Density algorithm
- Writes synthesis to file, returns metadata only
- Target: score > max(input_scores)

**5. safe-verifier** (Verification)
- Reads synthesis file, extracts atomic claims
- Validates via adversarial WebSearch queries
- Writes verification report to file, returns summary only
- Target: 95%+ pass rate

**6. report-finalizer** (Final Assembly)
- Reads synthesis + verification report files
- Writes all deliverables to files (report, summary, HTML, essay, bibliography)
- Returns manifest with file list only
- Embeds sentence-level citations in all outputs

### Agent I/O Protocol

**Rule:** Agents write to files, return metadata only (~300 bytes max).

**Why:** Prevents context overflow. 8 agents returning 15KB each = 120KB wasted. With metadata-only: 8 × 300 bytes = 2.4KB.

**Main Claude:** Pass `output_file_path` when invoking agents. Receive metadata (node_id, file_path, score). Update _process/graph_state.json with pointers, not content.

**Agents:** Write findings to provided file path. Return JSON metadata only. Never return large text blocks.

### Graph Traversal Strategy (Main Claude executes)

**Iteration 1: Initial Exploration**
```
Main Claude:
1. Spawn 5-10 multi-angle-researcher agents (HIGH priority angles, parallel)
   Pass output_file_path to each: /RESEARCH/[topic]/_process/nodes/n1.md, n2.md, etc.
2. Each agent writes findings to file, returns: {node_id, file_path, score}
3. Update _process/graph_state_1.json: add metadata (file pointers), not content
4. Prune: KeepBestN(7) - keep top 7 nodes
5. Decision: IF max_score > 9.5 → skip to Aggregation, ELSE → Triangulation
```

**Triangulation: Cross-Validation**
```
Main Claude:
1. Spawn triangulation-agent with node_file_paths=[all Iteration 1 nodes]
   Pass output_path: /RESEARCH/[topic]/_process/triangulation_report.md
2. Agent performs:
   - Cross-validates findings across nodes
   - Detects contradictions (CRITICAL/MEDIUM/LOW)
   - Calculates confidence scores for claims
   - RESCORES nodes in graph context (redundancy, uniqueness, consistency, contradictions)
3. Agent writes triangulation_report.md, returns metadata:
   {
     consistent_claims: 42,
     inconsistent_claims: 3,
     high_confidence_count: 15,
     critical_contradictions: 1,
     rescored_nodes: {
       "n1": {"old": 8.7, "new": 6.7, "delta": -2.0, "reason": "Redundancy with n3"},
       "n3": {"old": 9.1, "new": 10.0, "delta": +0.9, "reason": "Consistency + quality"},
       "n6": {"old": 7.0, "new": 9.0, "delta": +2.0, "reason": "Uniqueness bonus"}
     },
     nodes_to_deepen: ["n3", "n6"],    // uses UPDATED scores
     nodes_to_verify: ["n2"],
     nodes_to_prune: ["n1"],
     iteration_2_recommended_agents: 8
   }
4. Main Claude updates _process/graph_state.json with rescored values:
   - Replace old scores with updated scores from rescored_nodes
   - Use UPDATED scores for all subsequent decisions
5. Main Claude adjusts Iteration 2 strategy based on metadata:
   - IF critical_contradictions > 0 → spawn agents to resolve
   - IF unverified_claims > threshold → spawn agents to verify
   - Use nodes_to_deepen (based on updated scores >8.5)
   - Prune nodes_to_prune from graph
6. → Proceed to Iteration 2 with TARGETED strategy
```

**Iteration 2: Targeted Strategy (Based on Triangulation)**
```
Main Claude executes strategy using UPDATED scores from triangulation:

RESOLVE CONTRADICTIONS:
- For each critical_contradiction → spawn 2 agents with specific resolution angles
- Example: "n2 (updated score: 4.3) contradicts n5 → spawn agents to investigate methodology differences"

VERIFY UNVERIFIED CLAIMS:
- For each node in nodes_to_verify → spawn 1 agent to find corroborating sources
- Example: n2 (downgraded due to contradiction) must be verified before use

DEEPEN HIGH-CONFIDENCE:
- For each node in nodes_to_deepen → spawn 2 agents to expand validated findings
- Example: n3 (upgraded to 10.0) + n6 (upgraded to 9.0) are top priorities
- These have high updated scores (>8.5) based on uniqueness/consistency

PRUNE REDUNDANT/LOW-QUALITY:
- Remove nodes_to_prune from active graph
- Example: n1 (downgraded 8.7 → 6.7 due to redundancy) - don't spawn more agents

EXPLORE NEW ANGLES (if needed):
- Spawn 2-4 agents for new MEDIUM priority angles (as originally planned)
- Only if not dominated by contradiction resolution

Total: 6-12 agents spawned (TARGETED mix based on triangulation findings)

Update _process/graph_state_2.json with rescored values
Prune: KeepBestN(7) using UPDATED scores + explicit nodes_to_prune
Decision: IF max_updated_score > 9.5 OR depth > 2 → Aggregation, ELSE → Iteration 3
```

**Iteration 3: Aggregate**
```
Main Claude:
1. Identify 5-7 best nodes (score > 8.5)
2. Spawn cod-synthesizer with node_file_paths=[_process/nodes/n6.md, _process/nodes/n8.md, ...]
3. CoDSynthesizer reads files, writes synthesis, returns: {file_path, score: 9.6}
4. Update _process/graph_state_3.json with synthesis metadata
5. → Proceed to Verification
```

**Pruning Rules:**
- KeepBestN(7) at each depth level
- Never prune nodes in path to best leaf
- Prune aggressively if approaching context limits

**Termination:**
- max_score > 9.5, OR
- depth > 3, OR
- synthesis complete

### Context Management Strategy

**Problem:** Main Claude context overload with 80+ node findings

**Solution:**
```
Main Claude ONLY holds:
- _process/graph_state.json (~5KB metadata: node IDs, scores, edges, frontier)
- _process/ORCHESTRATION.md (~2KB: topic, user reqs, strategy)
- current iteration plan (~1KB: what to spawn next)

Total active memory: ~8KB

Node content stored in files:
- /RESEARCH/[topic]/_process/nodes/n1.md (findings + citations)
- /RESEARCH/[topic]/_process/nodes/n2.md
- ...
- /RESEARCH/[topic]/_process/nodes/n20.md

Agents read files when needed:
- TriangulationAgent: Read all Iteration 1 nodes (_process/nodes/n1.md through n7.md) + _process/graph_state_1.json - extract claims, original scores, rescore in context
- CoDSynthesizer: Read(_process/nodes/n6.md, _process/nodes/n8.md, _process/nodes/n10.md) - only nodes to merge
- SAFEVerifier: Read(_process/synthesis.md)
- ReportFinalizer: Read(_process/synthesis.md) + _process/graph_state.json for sources
```

Main Claude NEVER loads all node contents into context. Only metadata.

### Execution Flow

```
User: "Deep research [topic]"
         ↓
Main Claude → Task(research-planner, topic)
         ↓
Planner returns: {questions, angles[8-25], iteration_strategy}
         ↓
Main Claude → AskUserQuestion(questions)
         ↓
Main Claude creates: _process/ORCHESTRATION.md + _process/graph_state_0.json
         ↓
┌────────────────────────────────────────┐
│ ITERATION 1: Exploration               │
│ Main Claude spawns 5-10 agents         │
│ (multi-angle-researcher, parallel)     │
└────────────────────────────────────────┘
         ↓
Update graph → Prune → Check termination
         ↓
┌────────────────────────────────────────┐
│ TRIANGULATION: Cross-Validation (NEW)  │
│ Main Claude → Task(triangulation-agent)│
│ Cross-validate, detect contradictions  │
│ Update Iteration 2 strategy            │
└────────────────────────────────────────┘
         ↓
┌────────────────────────────────────────┐
│ ITERATION 2: Targeted Strategy         │
│ Main Claude spawns 6-12 agents         │
│ (resolve contradictions + verify +     │
│  deepen high-confidence + explore)     │
└────────────────────────────────────────┘
         ↓
Update graph → Prune → Check termination
         ↓
┌────────────────────────────────────────┐
│ ITERATION 3: Aggregate                 │
│ Main Claude → Task(cod-synthesizer)    │
└────────────────────────────────────────┘
         ↓
┌────────────────────────────────────────┐
│ VERIFICATION                            │
│ Main Claude → Task(safe-verifier)      │
└────────────────────────────────────────┘
         ↓
┌────────────────────────────────────────┐
│ FINALIZATION                            │
│ Main Claude → Task(report-finalizer)   │
└────────────────────────────────────────┘
         ↓
Complete Report in /RESEARCH/[topic]/
```

### Agent Files Location

```
.claude/agents/
├── Planner.md (research-planner)
├── MultiAngleResearcher.md (multi-angle-researcher)
├── TriangulationAgent.md (triangulation-agent) 
├── CoDSynthesizer.md (cod-synthesizer)
├── SAFEVerifier.md (safe-verifier)
└── ReportFinalizer.md (report-finalizer)
```

**No setup required** - agents are automatically available for Main Claude to deploy.

### Research Output Structure

When you run deep research, outputs are organized as:

```
RESEARCH/[topic]/
├── executive_summary.md          # Deliverable: Executive overview (2-3 pages)
├── full_report.md                # Deliverable: Comprehensive report (10-50 pages)
├── interactive_report.html       # Deliverable: Interactive visualization
├── academic_essay.md             # Deliverable: Peer-review format paper
├── bibliography.md               # Deliverable: Source catalog with quality ratings
└── _process/                     # Internal: Process artifacts (prefix _ = not for client)
    ├── ORCHESTRATION.md          # Research strategy and user requirements
    ├── triangulation_report.md   # Cross-validation findings 
    ├── synthesis.md              # Chain-of-Density synthesis
    ├── verification_report.md    # SAFE verification results
    ├── graph_state_0.json        # Initial graph state
    ├── graph_state_1.json        # Iteration 1 state
    ├── graph_state_2.json        # Iteration 2 state
    └── nodes/                    # Individual research findings
        ├── n1.md
        ├── n2.md
        └── ...
```

**Convention:**
- **Top-level files** = Client deliverables (ready for distribution)
- **_process/ folder** = Internal artifacts (methodology, intermediate states)
- Prefix `_` signals "technical/internal - not for end user"
