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

### Required Files
- `CLAUDE.md` - Main orchestration instructions (400+ lines, inline planning framework)
- `.claude/agents/` - 5 specialized research agents:
  - **multi-angle-researcher** - Source gathering (8-25 angles based on complexity)
  - **triangulation-agent** - Cross-validation, contradiction detection, node rescoring
  - **cod-synthesizer** - Chain-of-Density synthesis (5 iterations)
  - **safe-verifier** - Fact-checking (95%+ accuracy)
  - **report-finalizer** - Formatting & deliverables

**Portability:** Copy `CLAUDE.md` + `.claude/agents/` to any project. No configuration needed.

## How It Works

**Black Box Simplicity:**
- Input: Topic + answers to 2-4 questions
- Process: Graph of Thoughts orchestration (hidden from user)
- Output: Complete research package with 60-100+ sources

**Under the Hood:**
- Main Claude Code orchestrates directly (no controller agent)
- Inline planning (8-25 angles based on topic complexity)
- Graph of Thoughts with quality-based pruning
- Triangulation with node rescoring (redundancy penalty, uniqueness bonus, consistency bonus)
- Chain-of-Density summarization
- Search-Augmented Factuality Evaluation (SAFE)
- Massive parallelization (5-12 agents per iteration)

## Architecture

```
User: "Deep research [topic]"
         ↓
Main Claude: Inline planning (analyze complexity, generate 8-25 angles)
         ↓
Main Claude → AskUserQuestion (2-4 questions)
         ↓
Main Claude orchestrates 2-3 GoT iterations:
         ↓
┌────────────────────────────────────┐
│ ITERATION 1: Exploration (5-10 min)│
│ 5-10 multi-angle-researcher agents │
└────────────────────────────────────┘
         ↓
┌────────────────────────────────────┐
│ TRIANGULATION: Cross-Validation    │
│ Rescores nodes, detects conflicts  │
│ Updates graph with context-aware   │
│ scores (redundancy, uniqueness)    │
└────────────────────────────────────┘
         ↓
┌────────────────────────────────────┐
│ ITERATION 2: Targeted (5-10 min)   │
│ 6-12 agents (resolve contradictions│
│ + deepen unique/consistent nodes)  │
└────────────────────────────────────┘
         ↓
┌────────────────────────────────────┐
│ ITERATION 3: Aggregate (3-5 min)   │
│ CoD synthesis + SAFE verification  │
└────────────────────────────────────┘
         ↓
Complete Report (60-100+ sources)
```

## Example Usage

```
Deep research CRISPR gene editing safety
```

Main Claude will ask:
- Audience (business/technical/academic/interdisciplinary)
- Primary goal (brief overview / deep-dive / comparison)
- Specific aspects or focus areas (optional)

Then handles the rest automatically.

## Output Structure

```
RESEARCH/[topic]/
├── executive_summary.md
├── full_report.md (25-35 pages, popular science)
├── academic_essay.md (peer-review format)
├── interactive_report.html (visualization)
├── bibliography.md (A-E source quality ratings)
└── _process/ (internal artifacts)
    ├── ORCHESTRATION.md (research strategy)
    ├── triangulation_report.md (cross-validation findings)
    ├── synthesis.md (Chain-of-Density synthesis)
    ├── verification_report.md (SAFE results)
    ├── graph_state_0.json, graph_state_1.json, ...
    └── nodes/ (n1.md, n2.md, ... 15-25 nodes)
```

## Key Features

### Adaptive Granularity
- **Simple topics**: 8-10 research angles (definitional, factual)
- **Medium topics**: 12-16 research angles (comparative, analytical)
- **Complex topics**: 18-25 research angles (systemic, predictive, interdisciplinary)
- Main Claude decides based on topic complexity signals

### Triangulation & Rescoring 
- Cross-validates findings across all research angles after Iteration 1
- Detects contradictions (CRITICAL/MEDIUM/LOW priority)
- **Rescores nodes** in graph context:
  - Redundancy penalty (-2 points): 80%+ overlap with higher-scored node
  - Uniqueness bonus (+2 points): Covers topic no other node addresses
  - Consistency bonus (+1 point): Cross-validated by 2+ nodes
  - Contradiction penalty (-3 points): Involved in critical contradiction
- Enables targeted Iteration 2 strategy (resolve conflicts, deepen unique findings, prune redundant)

### Massive Parallel Sourcing
- Each angle = 1 agent
- Each agent = 6 sources (A-E quality ratings)
- Example: 15 angles × 6 sources = **90 sources** in ~10 min via parallelization

### Context Management
- Main Claude holds only metadata (~8KB: node IDs, scores, file paths)
- Node content saved to files (not loaded into context)
- No context overload even with 80+ findings

### Graph of Thoughts
- Multiple parallel exploration paths
- Quality-based pruning (KeepBestN using updated scores)
- Adaptive iteration depth (2-3 iterations based on findings)
- Transparent reasoning traces (graph states saved)

## Performance

- Planning: 2-3 min (inline, no agent overhead)
- Iteration 1: 5-10 min (5-10 agents parallel)
- Triangulation: 30-60 sec (cross-validation)
- Iteration 2: 5-10 min (6-12 agents parallel, targeted)
- Iteration 3: 3-5 min (synthesis)
- Verification: 5-7 min (SAFE)
- Finalization: 5-8 min
- **Total: 25-50 minutes**

## Quality Standards

- ✅ **60-100+ sources** (adaptive based on topic complexity)
- ✅ **95%+ claim verification rate** (SAFE methodology)
- ✅ **Sentence-level citations** (Author, Year, "Title", URL)
- ✅ **A-E source quality ratings** (peer-reviewed → blogs)
- ✅ **Multi-source corroboration** (claims validated across nodes)
- ✅ **Cross-angle triangulation** (contradictions detected early)
- ✅ **Context-aware scoring** (rescoring eliminates redundancy, rewards uniqueness)

## Version

**Version:** 3.0 (Triangulation & Inline Planning)
**Updated:** 2025-10-30
**Framework:** Graph of Thoughts + Chain-of-Density + SAFE + Triangulation
**Total Agents:** 5 (multi-angle-researcher, triangulation-agent, cod-synthesizer, safe-verifier, report-finalizer)
**Status:** ✅ Production Ready

## Changelog

### v3.0 (2025-10-30)
- ✅ Added: Triangulation agent (cross-validation, contradiction detection, node rescoring)
- ✅ Enhanced: Context-aware GoT scoring (redundancy penalty, uniqueness bonus, consistency bonus)
- ✅ Replaced: research-planner agent → inline planning (faster, no agent overhead)
- ✅ Enhanced: User approval step (shows all angles + priorities before execution)
- ✅ Enhanced: Targeted Iteration 2 strategy (resolve contradictions, verify claims, deepen validated)

### v2.0 (2025-10-29)
- ❌ Removed: research-controller agent (over-engineered)
- ✅ Simplified: Main Claude orchestrates directly
- ✅ Enhanced: Adaptive angle granularity (8-25 based on complexity)
- ✅ Enhanced: Context management strategy (files, not memory)
- ✅ Enhanced: 60-100+ sources (vs 15-30 in v1.0)
