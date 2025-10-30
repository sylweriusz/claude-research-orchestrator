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
- **Iteration 2**: Deepen best paths + explore new angles (5-10 more agents)
- **Iteration 3**: Aggregate best findings with `cod-synthesizer`

Each iteration: spawn agents → receive scored findings → update graph → prune weak paths → decide next step

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

## Overview
This is a complete, self-contained implementation of Graph of Thoughts (GoT) for deep research using dedicated agents in `.claude/agents/` folder. All agent logic is self-contained - no external files or setup required.

## Understanding Graph of Thoughts

Graph of Thoughts is a reasoning framework where:
- **Thoughts = Nodes**: Each research finding or synthesis is a node
- **Edges = Dependencies**: Connect parent thoughts to children
- **Transformations**: Operations that create (Generate), merge (Aggregate), or improve (Refine) thoughts
- **Scoring**: Every thought is evaluated 0-10 for quality
- **Pruning**: Low-scoring branches are abandoned
- **Frontier**: Active nodes available for expansion

The system explores multiple research paths in parallel, scores them, and finds optimal solutions through graph traversal.

## The 7-Phase Deep Research Process
prep: make sure you put all of your produced documents inside of the folder /RESERACH/[create project name] where create project name is a name you decide based on the inquiry.  Also note that when you create files break down into smaller doucments to avoid context limitations.  Make sure you also compelte all tasks you define from the start of the project and track completion as you go.
### Phase 1: Question Scoping
- Clarify the research question with the user
- Define output format and success criteria
- Identify constraints and desired tone
- Create unambiguous query with clear parameters

### Phase 2: Retrieval Planning
- Break main question into subtopics
- Generate specific search queries
- Select appropriate data sources
- Create research plan for user approval
- Use GoT to model the research as a graph of operations

### Phase 3: Iterative Querying
- Execute searches systematically
- Navigate and extract relevant information
- Formulate new queries based on findings
- Use multiple search modalities (web search, file analysis, etc.)
- Apply GoT operations for complex reasoning

### Phase 4: Source Triangulation
- Compare findings across multiple sources
- Validate claims with cross-references
- Handle inconsistencies
- Assess source credibility
- Use GoT scoring functions to evaluate information quality

### Phase 5: Knowledge Synthesis
- Structure content logically
- Write comprehensive sections
- Include inline citations for every claim
- Add data visualizations when relevant
- Use GoT to optimize information organization

### Phase 6: Quality Assurance
- Check for hallucinations and errors
- Verify all citations match content
- Ensure completeness and clarity
- Apply Chain-of-Verification techniques
- Use GoT ground truth operations for validation

### Phase 7: Output & Packaging
- Format for optimal readability
- Include executive summary
- Create proper bibliography
- **Generate interactive HTML visualization** (standard deliverable)
- **Generate academic essay in peer-review format** (standard deliverable)
- Export in requested format

## How Graph of Thoughts Works for Research

### Core Concepts

1. **Graph Structure**: 
   - Each research finding is a node with a unique ID
   - Nodes have scores (0-10) indicating quality
   - Edges connect parent thoughts to child thoughts
   - The frontier contains active nodes for expansion

2. **Transformation Operations**:
   - **Generate(k)**: Create k new thoughts from a parent
   - **Aggregate(k)**: Merge k thoughts into one stronger thought
   - **Refine(1)**: Improve a thought without adding new content
   - **Score**: Evaluate thought quality
   - **KeepBestN(n)**: Prune to keep only top n nodes per level

3. **Research Quality Metrics**:
   - Citation density and accuracy
   - Source credibility
   - Claim verification
   - Comprehensiveness
   - Logical coherence

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

## Graph of Thoughts Research Strategy

The system implements GoT using Task agents that act as transformation operations. When you request deep research, a controller agent maintains the graph state and deploys specialized agents to explore, refine, and aggregate research paths.

### Implementation Instructions

#### Step 1: Create Research Plan
Break down the main research question into specific subtopics:
- Subtopic 1: Current state and trends
- Subtopic 2: Key challenges and limitations
- Subtopic 3: Future developments and predictions
- Subtopic 4: Case studies and real-world applications
- Subtopic 5: Expert opinions and industry perspectives

#### Step 2: Launch Parallel Agents
Use multiple Task tool invocations in a single response to launch agents simultaneously. Each agent should receive:
- Clear description of their research focus
- Specific instructions on what to find
- Expected output format

#### Step 3: Coordinate Results
After agents complete their tasks:
- Compile findings from all agents
- Identify overlaps and contradictions
- Synthesize into coherent narrative
- Maintain source attribution from each agent

### Example Multi-Agent Deployment

When researching a topic like "AI in Healthcare", deploy agents as follows:

**Agent 1**: "Research current AI applications in healthcare"
**Agent 2**: "Find challenges and ethical concerns in medical AI"
**Agent 3**: "Investigate future AI healthcare innovations"
**Agent 4**: "Gather case studies of successful AI healthcare implementations"
**Agent 5**: "Cross-reference and verify key statistics about AI healthcare impact"

### Best Practices for Multi-Agent Research

1. **Clear Task Boundaries**: Each agent should have a distinct focus to minimize redundancy
2. **Comprehensive Prompts**: Include all necessary context in agent prompts
3. **Parallel Execution**: Launch all agents in one response for maximum efficiency
4. **Result Integration**: Plan how to merge findings before launching agents
5. **Quality Control**: Always include at least one verification agent

### Agent Coordination

**CRITICAL: Question-Asking Protocol**

Only **main Claude Code** can ask clarifying questions using AskUserQuestion. Agents **CANNOT** ask questions.

**Correct Flow:**
1. User: "Deep research [topic]"
2. **Main Claude** invokes research-planner agent
3. Planner returns: questions_for_user + research_angles + iteration_strategy
4. **Main Claude** asks user questions via AskUserQuestion
5. User answers
6. **Main Claude** orchestrates GoT iterations with packaged requirements

**Wrong Flow:**
1. User: "Deep research [topic]"
2. Main Claude invokes research-planner
3. Planner tries to ask user questions directly ❌ (agents can't do this)
4. System fails

**Why this matters:** Planner generates questions FOR Main Claude to ask, but cannot ask them itself.

**Implementation in Agents:**

All agent prompt templates in `.claude/agents/` receive pre-packaged requirements:
```json
{
  "topic": "string",
  "audience": "business|technical|academic|general",
  "length": "short|medium|comprehensive",
  "specific_needs": ["comparisons", "case studies", "data"],
  "sources_preferred": ["academic", "industry", "news"],
  "geographic_focus": "global|regional|specific"
}
```

Agents work with these requirements - no interactive questioning needed.

### Tool Usage Instructions for Agents

**WebSearch Usage**:
```
Use WebSearch with specific queries:
- Include key terms in quotes for exact matches
- Use domain filtering for authoritative sources
- Try multiple query variations
```

**WebFetch Usage**:
```
After WebSearch identifies URLs:
1. Use WebFetch with targeted prompts
2. Ask for specific information extraction
3. Request summaries of long content
```

**Puppeteer MCP Usage**:
```
For JavaScript-heavy sites:
1. mcp__puppeteer__puppeteer_navigate to URL
2. mcp__puppeteer__puppeteer_screenshot for visual content
3. mcp__puppeteer__puppeteer_evaluate to extract dynamic data
```

## Research Quality Checklist

- [ ] Every claim has a verifiable source
- [ ] Multiple sources corroborate key findings
- [ ] Contradictions are acknowledged and explained
- [ ] Sources are recent and authoritative
- [ ] No hallucinations or unsupported claims
- [ ] Clear logical flow from evidence to conclusions
- [ ] Proper citation format throughout

## Advanced Research Methodologies

### Chain-of-Density (CoD) Summarization
When processing sources, use iterative refinement to increase information density:
1. First pass: Extract key points (low density)
2. Second pass: Add supporting details and context
3. Third pass: Compress while preserving all critical information
4. Final pass: Maximum density with all essential facts and citations

### Chain-of-Verification (CoVe)
To prevent hallucinations:
1. Generate initial research findings
2. Create verification questions for each claim
3. Search for evidence to answer verification questions
4. Revise findings based on verification results
5. Repeat until all claims are verified

### ReAct Pattern (Reason + Act)
Agents should follow this loop:
1. **Reason**: Analyze what information is needed
2. **Act**: Execute search or retrieval action
3. **Observe**: Process the results
4. **Reason**: Determine if more information needed
5. **Repeat**: Continue until sufficient evidence gathered

### Multi-Agent Orchestration
For complex topics, deploy specialized agents:
- **Planner Agent**: Decomposes research into subtopics
- **Search Agents**: Execute queries and retrieve sources
- **Synthesis Agents**: Combine findings from multiple sources
- **Critic Agents**: Verify claims and check for errors
- **Editor Agent**: Polishes final output

### Human-in-the-Loop Checkpoints
Critical intervention points:
1. **After Planning**: Approve research strategy
2. **During Verification**: Expert review of technical claims
3. **Before Finalization**: Stakeholder sign-off
4. **Post-Delivery**: Feedback incorporation

## Citation Requirements & Source Traceability

### Mandatory Citation Standards

**Every factual claim must include:**
1. **Author/Organization** - Who made this claim
2. **Date** - When the information was published
3. **Source Title** - Name of paper, article, or report
4. **URL/DOI** - Direct link to verify the source
5. **Page Numbers** - For lengthy documents (when applicable)

### Citation Formats

**Academic Papers:**
```
(Author et al., Year, p. XX) with full citation in references
Example: (Smith et al., 2023, p. 145) 
Full: Smith, J., Johnson, K., & Lee, M. (2023). "Title of Paper." Journal Name, 45(3), 140-156. https://doi.org/10.xxxx/xxxxx
```

**Web Sources:**
```
(Organization, Year, Section Title)
Example: (NIH, 2024, "Treatment Guidelines")
Full: National Institutes of Health. (2024). "Treatment Guidelines for Metabolic Syndrome." Retrieved [date] from https://www.nih.gov/specific-page
```

**Direct Quotes:**
```
"Exact quote from source" (Author, Year, p. XX)
```

### Source Verification Protocol

1. **Primary Sources Only** - Link to original research, not secondary reporting
2. **Archive Links** - For time-sensitive content, include archive.org links
3. **Multiple Confirmations** - Critical claims need 2+ independent sources
4. **Conflicting Data** - Note when sources disagree and explain discrepancies
5. **Source Quality Ratings**:
   - **A**: Peer-reviewed RCTs, systematic reviews, meta-analyses
   - **B**: Cohort studies, case-control studies, clinical guidelines
   - **C**: Expert opinion, case reports, mechanistic studies
   - **D**: Preliminary research, preprints, conference abstracts
   - **E**: Anecdotal, theoretical, or speculative

### Traceability Requirements

**For Medical/Health Information:**
- PubMed ID (PMID) when available
- Clinical trial registration numbers
- FDA/regulatory body references
- Version/update dates for guidelines

**For Genetic Information:**
- dbSNP rs numbers
- Gene database links (OMIM, GeneCards)
- Population frequency sources (gnomAD, 1000 Genomes)
- Effect size sources with confidence intervals

**For Statistical Claims:**
- Sample sizes
- P-values and confidence intervals
- Statistical methods used
- Data availability statements

### Source Documentation Structure

Each research output must include:

1. **Inline Citations** - Throughout the text
2. **References Section** - Full bibliography at end
3. **Source Quality Table** - Rating each source A-E
4. **Verification Checklist** - Confirming each claim is sourced
5. **Data Availability** - Where raw data can be accessed

### Example Implementation

**Poor Citation:**
"Studies show that metformin reduces diabetes risk."

**Proper Citation:**
"The Diabetes Prevention Program demonstrated that metformin reduces diabetes incidence by 31% over 2.8 years in high-risk individuals (Knowler et al., 2002, NEJM, PMID: 11832527, https://doi.org/10.1056/NEJMoa012512)"

### Red Flags for Unreliable Sources

- No author attribution
- Missing publication dates
- Broken or suspicious URLs
- Claims without data
- Conflicts of interest not disclosed
- Predatory journals
- Retracted papers (check RetractionWatch)

### Agent Instructions for Citations

When deploying research agents, include:
```
"For every factual claim, provide:
1. Direct quote or specific data point
2. Author/organization name
3. Publication year
4. Full title
5. Direct URL/DOI
6. Confidence rating (High/Medium/Low)
Never make claims without sources. If uncertain, state 'Source needed' rather than guessing."
```

## Mitigation Strategies

### Hallucination Prevention:
- Always ground statements in source material
- Use Chain-of-Verification for critical claims
- Cross-reference multiple sources
- Explicitly state uncertainty when appropriate

### Coverage Optimization:
- Use diverse search queries
- Check multiple perspectives
- Include recent sources (check dates)
- Acknowledge limitations and gaps

### Citation Management:
- Track source URLs and access dates
- Quote relevant passages verbatim when needed
- Maintain source-to-statement mapping
- Use consistent citation format

## Research Tool Recommendations

### For Implementation (if building custom systems):
- **LangChain**: Orchestrate LLM + tools workflows
- **LlamaIndex**: Index and query document collections
- **Tavily Search API**: AI-optimized search results
- **Guardrails AI**: Enforce output requirements
- **FAISS/Pinecone**: Vector databases for semantic search

### Model Selection Strategy:
- **High-volume tasks**: Use faster models (GPT-4o-mini, Gemini Flash)
  - Query generation
  - Initial summarization
  - Basic searches
- **Critical tasks**: Use advanced models (GPT-4o, Gemini Pro)
  - Planning phase
  - Final synthesis
  - Verification loops
  - Complex reasoning

### Cost Optimization:
- Typical research: 120-220 LLM calls, 30-60 search calls
- Use tiered model approach to reduce costs
- Cache intermediate results
- Batch similar operations

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

### Output Creation Protocol

Based on gathered requirements, I will create all research outputs in the **RESEARCH** folder:

```
RESEARCH/
└── [topic_name]/
    ├── README.md (Overview and navigation guide)
    ├── executive_summary.md (1-2 page summary)
    ├── full_report.md (Comprehensive findings)
    ├── data/
    │   ├── raw_data.csv
    │   ├── processed_data.json
    │   └── statistics_summary.md
    ├── visuals/
    │   ├── charts/
    │   ├── graphs/
    │   └── infographics/
    ├── sources/
    │   ├── bibliography.md
    │   ├── source_summaries.md
    │   └── screenshots/
    ├── research_notes/
    │   ├── agent_1_findings.md
    │   ├── agent_2_findings.md
    │   └── synthesis_notes.md
    └── appendices/
        ├── methodology.md
        ├── limitations.md
        └── future_research.md
```

**Important**: All research outputs will be saved in `./RESEARCH/[topic_name]/` where [topic_name] is a descriptive folder name based on the research topic.

### Question Templates I Will Ask

1. **Topic Clarification**
   - "What specific aspects of [topic] are most important for your needs?"
   - "Are you looking for current state analysis, historical trends, or future predictions?"

2. **Output Specification**
   - "Would you prefer a single comprehensive report or multiple focused documents?"
   - "Do you need visualizations? If so, what types would be most helpful?"

3. **Scope Definition**
   - "Are there any geographic regions or time periods I should focus on?"
   - "What level of technical detail is appropriate for your audience?"

4. **Source Preferences**
   - "Do you have any preferred sources or databases I should prioritize?"
   - "Are there any sources or viewpoints I should avoid?"

5. **Delivery Format**
   - "How would you like the files organized?"
   - "Do you need the raw research data or just the final analysis?"

### Example Interaction Flow

```
User: "I need research on AI in healthcare"

Assistant: "I'll help you research AI in healthcare. Let me ask some questions to ensure I deliver exactly what you need:

1. **Specific Focus**: What aspects of AI in healthcare interest you most?
   - Clinical diagnosis and treatment
   - Drug discovery and development
   - Hospital operations and management
   - Patient care and monitoring
   - Medical imaging and radiology
   - All of the above

2. **Output Format**: What type of deliverable would be most useful?
   - Comprehensive report (20-30 pages)
   - Executive summary (3-5 pages)
   - Presentation slides
   - Data analysis with visualizations

3. **Scope**: Any specific constraints?
   - Geographic focus (US, Europe, Global?)
   - Time period (current state, last 5 years, future projections?)
   - Healthcare sectors (hospitals, pharma, insurance, tech companies?)

4. **Audience**: Who will be reading this research?
   - Technical team
   - Business executives
   - Healthcare professionals
   - General audience"

User: "Focus on clinical diagnosis, comprehensive report, global scope but emphasize US and Europe, last 3 years plus future trends. Audience is healthcare executives considering AI adoption."

Assistant: "Perfect\! A few more clarifications:

5. **Data Requirements**: Would you like me to include:
   - Market size and growth projections?
   - Case studies of successful implementations?
   - ROI and cost-benefit analyses?
   - Regulatory landscape overview?

6. **Deliverable Structure**: Should I create:
   - Single comprehensive document?
   - Folder structure with main report + supporting documents?
   - Include raw data files and source screenshots?

7. **Visual Elements**: What would help communicate the findings?
   - Market trend graphs
   - Technology adoption timelines
   - Comparison tables
   - Process flow diagrams"

User: "Yes to all data requirements. Folder structure please with main report, executive summary, and supporting docs. Include market trends graphs and comparison tables. Raw data would be helpful for our internal analysis."

Assistant: "Excellent\! I have all the information needed. Here's my research plan:

**Research Plan: AI in Clinical Diagnosis**

**Output Structure:**
```
AI_Clinical_Diagnosis_Research/
├── README.md
├── 01_Executive_Summary.md (3-5 pages)
├── 02_Full_Report.md (25-30 pages)
├── 03_Data/
│   ├── market_analysis.csv
│   ├── adoption_rates.json
│   └── roi_calculations.xlsx
├── 04_Visuals/
│   ├── market_trends_2021-2024.png
│   ├── adoption_timeline.png
│   └── vendor_comparison_table.png
├── 05_Case_Studies/
│   ├── mayo_clinic_ai.md
│   ├── cleveland_clinic.md
│   └── european_examples.md
├── 06_Sources/
│   ├── bibliography.md
│   └── key_reports/
└── 07_Appendices/
    ├── regulatory_overview.md
    └── technical_glossary.md
```

**Multi-Agent Deployment:**
- Agent 1: Current market analysis and adoption rates
- Agent 2: Technology assessment and clinical outcomes
- Agent 3: Regulatory landscape and compliance
- Agent 4: Case studies and ROI analysis
- Agent 5: Future trends and emerging technologies
- Agent 6: Cross-verification and fact-checking

Shall I proceed with this research plan?"
```

## Complete Example: How GoT Research Works

### When you say: "Deep research CRISPR gene editing safety"

Here's the complete execution flow:

#### Iteration 1: Initialize and Explore
1. **Controller Agent** creates root node: "Research CRISPR gene editing safety"
2. **Generate(3)** deploys 3 parallel agents exploring:
   - Current evidence and success rates
   - Safety concerns and limitations
   - Future implications and regulations
3. **Results**: 3 thoughts with scores (6.8, 8.2, 7.5)
4. **Graph state** saved with frontier = [n3(8.2), n2(7.5), n4(6.8)]

#### Iteration 2: Deepen Best Paths
1. **Controller** examines frontier, decides:
   - n3 (8.2): High score → Generate(3) for deeper exploration
   - n2 (7.5): Medium → Generate(2) 
   - n4 (6.8): Low → Refine(1) to improve
2. **6 agents** deployed in parallel
3. **Best result**: "High-fidelity SpCas9 variants reduce off-targets by 95%" (Score: 9.1)

#### Iteration 3: Aggregate Strong Branches  
1. **Controller** sees multiple high scores
2. **Aggregate(3)** merges best thoughts into comprehensive synthesis
3. **Score**: 9.3 - exceeds threshold

#### Iteration 4: Final Polish
1. **Refine(1)** enhances clarity and completeness
2. **Final thought** scores 9.5
3. **Output**: Best path through graph becomes research report

### What Makes This True GoT

1. **Graph maintained** throughout with nodes, edges, scores
2. **Multiple paths** explored in parallel
3. **Pruning** drops weak branches 
4. **Scoring** guides exploration vs exploitation
5. **Optimal solution** found through graph traversal

The result is higher quality research than linear approaches, with transparent reasoning paths.

## Key Principles of Deep Research

### Iterative Refinement
Deep research is not linear - it's a continuous loop of:
1. **Search**: Find relevant information
2. **Read**: Extract key insights
3. **Refine**: Generate new queries based on findings
4. **Verify**: Cross-check claims across sources
5. **Synthesize**: Combine into coherent narrative
6. **Repeat**: Continue until comprehensive coverage

### Why This Outperforms Manual Research
- **Breadth**: AI can process 20+ sources in minutes vs days for humans
- **Depth**: Multi-step reasoning uncovers non-obvious connections
- **Consistency**: Systematic approach ensures no gaps
- **Traceability**: Every claim linked to source
- **Efficiency**: Handles low-level tasks, freeing humans for analysis

### State Management
Throughout the research process, maintain:
- Current research questions
- Sources visited and their quality scores
- Extracted claims and verification status
- Graph state (for GoT implementation)
- Progress tracking against original plan

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

### The 5 Specialized Agents

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

**3. cod-synthesizer** (Aggregate)
- Reads 3-7 node files, merges into synthesis
- 5-iteration Chain-of-Density algorithm
- Writes synthesis to file, returns metadata only
- Target: score > max(input_scores)

**4. safe-verifier** (Verification)
- Reads synthesis file, extracts atomic claims
- Validates via adversarial WebSearch queries
- Writes verification report to file, returns summary only
- Target: 95%+ pass rate

**5. report-finalizer** (Final Assembly)
- Reads synthesis + verification report files
- Writes all deliverables to files (report, summary, HTML, essay, bibliography)
- Returns manifest with file list only
- Embeds sentence-level citations in all outputs

### Agent I/O Protocol

**Rule:** Agents write to files, return metadata only (~300 bytes max).

**Why:** Prevents context overflow. 8 agents returning 15KB each = 120KB wasted. With metadata-only: 8 × 300 bytes = 2.4KB.

**Main Claude:** Pass `output_file_path` when invoking agents. Receive metadata (node_id, file_path, score). Update graph_state.json with pointers, not content.

**Agents:** Write findings to provided file path. Return JSON metadata only. Never return large text blocks.

### Graph Traversal Strategy (Main Claude executes)

**Iteration 1: Initial Exploration**
```
Main Claude:
1. Spawn 5-10 multi-angle-researcher agents (HIGH priority angles, parallel)
   Pass output_file_path to each: /RESEARCH/[topic]/nodes/n1.md, n2.md, etc.
2. Each agent writes findings to file, returns: {node_id, file_path, score}
3. Update graph_state_1.json: add metadata (file pointers), not content
5. Prune: KeepBestN(7) - keep top 7 nodes
6. Decision: IF max_score > 9.5 → skip to Aggregation, ELSE → Iteration 2
```

**Iteration 2: Deepen + Explore**
```
Main Claude analyzes frontier:
- Top 2-3 nodes (score > 8.5) → spawn 2 agents each to deepen (Generate)
- Medium nodes (7.0-8.5) → spawn 1 agent to refine
- Low nodes (< 7.0) → prune
- Plus: spawn 2-4 agents for new MEDIUM priority angles

Total: 6-12 agents spawned (mix of deepening + new exploration)

Update graph_state_2.json
Prune: KeepBestN(7)
Decision: IF max_score > 9.5 OR depth > 2 → Aggregation, ELSE → Iteration 3
```

**Iteration 3: Aggregate**
```
Main Claude:
1. Identify 5-7 best nodes (score > 8.5)
2. Spawn cod-synthesizer with node_file_paths=[nodes/n6.md, nodes/n8.md, ...]
3. CoDSynthesizer reads files, writes synthesis, returns: {file_path, score: 9.6}
4. Update graph_state_3.json with synthesis metadata
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
- graph_state.json (~5KB metadata: node IDs, scores, edges, frontier)
- orchestration_doc.md (~2KB: topic, user reqs, strategy)
- current iteration plan (~1KB: what to spawn next)

Total active memory: ~8KB

Node content stored in files:
- /RESEARCH/[topic]/nodes/n1.md (findings + citations)
- /RESEARCH/[topic]/nodes/n2.md
- ...
- /RESEARCH/[topic]/nodes/n20.md

Agents read files when needed:
- CoDSynthesizer: Read(n6.md, n8.md, n10.md) - only nodes to merge
- SAFEVerifier: Read(synthesis.md)
- ReportFinalizer: Read(synthesis.md) + graph_state.json for sources
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
Main Claude creates: ORCHESTRATION.md + graph_state_0.json
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
│ ITERATION 2: Deepen + Explore          │
│ Main Claude spawns 6-12 agents         │
│ (mix: deepen best + new angles)        │
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
├── CoDSynthesizer.md (cod-synthesizer)
├── SAFEVerifier.md (safe-verifier)
└── ReportFinalizer.md (report-finalizer)
```

**No setup required** - agents are automatically available for Main Claude to deploy.

Research outputs will be saved to `RESEARCH/[topic]/` when you run deep research.
