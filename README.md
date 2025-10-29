# Claude Research Orchestrator

> Multi-agent research system powered by Graph of Thoughts architecture and Claude Code

## Quick Start

```bash
Deep research [your topic]
```

The orchestrator will:
1. Ask 2-3 clarifying questions to understand your needs
2. Generate a comprehensive research report in 30-45 minutes
3. Save all outputs to `/RESEARCH/[topic]/`

## What You Get

- **Executive Summary** (1-2 pages) - Key findings and actionable insights
- **Full Research Report** (20-30 pages) - Comprehensive analysis with citations
- **Complete Bibliography** - All sources with quality ratings (A-E scale)
- **Verification Checklist** - 95%+ claim accuracy validation
- **Methodology Documentation** - Complete audit trail of research process

## System Architecture

### Required Files
```
CLAUDE.md                          # Main orchestration instructions
.claude/agents/                    # 6 specialized research agents
  ├── research-controller          # Master orchestrator
  ├── research-planner             # Research strategy design
  ├── multi-angle-researcher       # Multi-source data gathering
  ├── cod-synthesizer              # Chain-of-Density summarization
  ├── safe-verifier                # Fact-checking & validation
  └── report-finalizer             # Document formatting & polish
```

## How It Works

**From User Perspective:**
- **Input:** Research topic + optional preferences
- **Output:** Publication-ready research report
- **Hidden Complexity:** Graph of Thoughts orchestration with 6 specialized agents

**Technical Foundation:**
- **Graph of Thoughts (GoT):** Non-linear reasoning framework
- **Chain-of-Density (CoD):** Progressive summarization technique
- **SAFE Protocol:** Search-Augmented Factuality Evaluation
- **Multi-Source Strategy:** Web + Academic + Technical documentation

## Portability

Zero configuration required. Copy these files to any Claude Code project:

```bash
CLAUDE.md
.claude/agents/
```

The system is completely self-contained and works out of the box.

## Usage Examples

### Basic Research
```bash
Deep research CRISPR gene editing safety protocols
```

### Advanced Research with Preferences
```bash
Deep research: AI applications in medical diagnostics

Target audience: Healthcare executives
Depth: Comprehensive (25-30 pages)
Requirements: Case studies, ROI analysis, regulatory landscape
```

## Output Structure

```
RESEARCH/[topic]/
├── README.md                      # Research overview
├── executive_summary.md           # High-level findings
├── full_report.md                 # Complete analysis
├── sources/
│   ├── bibliography.md            # All sources cited
│   └── source_quality_table.md    # Quality assessment
└── appendices/
    ├── methodology.md             # Research approach
    └── verification_checklist.md  # Validation results
```

## Technical Details

**Framework:** Graph of Thoughts (GoT)
- Non-linear reasoning paths
- Parallel agent execution
- State-based orchestration

**Agents:** 6 specialized roles
- Controller: Master orchestration
- Planner: Research design
- Researcher: Multi-angle sourcing
- Synthesizer: CoD summarization
- Verifier: SAFE fact-checking
- Finalizer: Document production

**Validation:** Search-Augmented Factuality Evaluation
- Claim extraction
- Source verification
- Multi-source corroboration
- Confidence scoring

## Version Information

- **Version:** 1.0.0
- **Release Date:** 2025-10-29
- **Framework:** GoT + CoD + SAFE
- **Agent Count:** 6
- **Status:** Production Ready ✅

## Repository Structure

```
claude-research-orchestrator/
├── CLAUDE.md                      # Main system prompt
├── .claude/
│   └── agents/                    # 6 specialized agents
├── RESEARCH/                      # Generated reports (gitignored)
├── research_template_oldies/      # Development documentation
└── README.md                      # This file
```

## Requirements

- Claude Code CLI
- Internet connection (for web research)
- ~30-55 minutes per research session

## License

MIT

## Acknowledgments

Built on research in:
- Graph of Thoughts reasoning frameworks
- Chain-of-Density summarization
- Search-Augmented Factuality Evaluation
- Multi-agent orchestration systems
