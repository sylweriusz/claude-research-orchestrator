# Deep Research System - Graph of Thoughts Implementation

## Quick Start

### Installation

```bash
git clone https://github.com/sylweriusz/claude-research-orchestrator.git
cd claude-research-orchestrator

# Launch Claude Code with permission bypass (sandbox mode)
claude --dangerously-skip-permissions
```

### Usage

```
Deep research [your topic]
```

That's it! The system will:
1. Ask you 2-4 clarifying questions
2. Generate a comprehensive research report in 25-50 minutes
3. Save everything to `/RESEARCH/[topic]/`

### About Permission Mode

The `--dangerously-skip-permissions` flag enables autonomous agent operation:
- Agents execute file operations without manual approval
- Web searches and tool calls proceed automatically
- Required for parallel agent orchestration (5-12 concurrent agents)

**Security:** This flag bypasses permission checks. Use only in trusted directories or sandboxed environments.

## What You Get

- Executive summary (2-3 pages)
- Full research report (25-35 pages)
- Academic essay (peer-review format)
- Interactive HTML visualization
- Complete bibliography with A-E source quality ratings
- 60-100+ sources (organic, no duplicates)
- 95%+ claim verification rate

## System Components

### Active Files (Required)
- `CLAUDE.md` - Main orchestration instructions (for Main Claude)
- `.claude/agents/` - 5 specialized research agents
  - research-planner (strategy & question generation)
  - multi-angle-researcher (source gathering, 8-25 angles)
  - cod-synthesizer (Chain-of-Density synthesis)
  - safe-verifier (fact-checking)
  - report-finalizer (formatting & deliverables)
- `RESEARCH/` - Empty folder where research outputs will be saved

## How It Works

**Black Box Simplicity:**
- Input: Topic
- Questions: Main Claude asks 2-4 clarifying questions
- Output: Complete research package
- Hidden: Graph of Thoughts orchestration with 5 specialized agents

**Under the Hood:**
- **Main Claude Code orchestrates** (no controller agent)
- Graph of Thoughts (GoT) framework with scoring & pruning
- Chain-of-Density (CoD) summarization (5 iterations)
- Search-Augmented Factuality Evaluation (SAFE)
- Multi-angle sourcing with adaptive granularity (8-25 angles)
- Massive parallelization (5-12 agents per iteration)

## Architecture

```
User: "Deep research [topic]"
         ↓
Main Claude → Task(research-planner)
         ↓
Planner returns: questions + angles[8-25] + iteration_strategy
         ↓
Main Claude → AskUserQuestion
         ↓
Main Claude orchestrates 2-3 GoT iterations:
  - Iteration 1: 5-10 agents (parallel)
  - Iteration 2: 6-12 agents (deepen + explore)
  - Iteration 3: Aggregate best findings
         ↓
Main Claude → Verify → Finalize
         ↓
Complete Report (60-100+ sources)
```

## Portability

Copy these files to any project:
```
CLAUDE.md
.claude/agents/
```

No configuration needed. System is completely self-contained.

## Example Usage

### Minimal
```
Deep research CRISPR gene editing safety
```

Main Claude will ask you questions and handle the rest.

### What Main Claude Will Ask
- Audience (business/technical/academic/interdisciplinary)
- Length (short 10-15 pages / medium 25-35 / comprehensive 50+)
- Specific aspects or focus areas
- Source preferences (if applicable)

## Output Structure

```
RESEARCH/[topic]/
├── ORCHESTRATION.md (research strategy)
├── executive_summary.md
├── full_report.md (25-35 pages, popular science)
├── academic_essay.md (peer-review format)
├── interactive_report.html (visualization)
├── nodes/
│   ├── n1.md (findings from iteration 1)
│   ├── n2.md
│   └── ... (15-25 nodes)
├── sources/
│   ├── bibliography.md
│   └── source_quality_table.md (A-E ratings)
├── graph_state_0.json
├── graph_state_1.json
└── graph_state_N.json (final)
```

## Quality Standards

- ✅ **60-100+ sources** (adaptive based on topic complexity)
- ✅ **95%+ claim verification rate** (SAFE methodology)
- ✅ **Sentence-level citations** (Author, Year, "Title", URL)
- ✅ **A-E source quality ratings** (peer-reviewed → blogs)
- ✅ **Multi-source corroboration** (claims validated across sources)
- ✅ **Complete audit trail** (graph states show reasoning)
- ✅ **Context management** (Main Claude holds only metadata, not full content)

## Performance

- Planning: 2-3 min
- Iteration 1: 5-10 min (5-10 agents parallel)
- Iteration 2: 5-10 min (6-12 agents parallel)
- Iteration 3: 3-5 min (synthesis)
- Verification: 5-7 min
- Finalization: 5-8 min
- **Total: 25-50 minutes**

## Key Features

### Adaptive Granularity
- **Simple topics**: 8-10 research angles
- **Medium topics**: 12-16 research angles
- **Complex topics**: 18-25 research angles
- Planner decides based on interdisciplinary nature and depth required

### Massive Parallel Sourcing
- Each angle = 1 agent
- Each agent = 6 sources
- Example: 15 angles × 6 sources = **90 sources** (in ~10 min via parallelization)

### Context Management
- Main Claude holds only metadata (~8KB)
- Node content saved to files
- No context overload even with 80+ findings

### Graph of Thoughts
- Multiple parallel exploration paths
- Quality-based pruning (KeepBestN(7))
- Adaptive iteration depth (2-3 iterations)
- Transparent reasoning traces

## Version

**Version:** 2.0 (Simplified Architecture)
**Updated:** 2025-10-29
**Framework:** Graph of Thoughts + Chain-of-Density + SAFE
**Total Agents:** 5 (Main Claude orchestrates)
**Status:** ✅ Production Ready

## Changelog from v1.0

- ❌ Removed: research-controller agent (over-engineered)
- ✅ Simplified: Main Claude orchestrates directly
- ✅ Enhanced: Adaptive angle granularity (8-25 based on complexity)
- ✅ Enhanced: Context management strategy (files, not memory)
- ✅ Enhanced: 60-100+ sources (vs 15-30 in v1.0)
- ✅ Fixed: Question-asking protocol (only Main Claude can ask)
