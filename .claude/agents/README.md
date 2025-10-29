# Deep Research Agents - Graph of Thoughts Implementation

This folder contains 6 specialized agents that implement a complete Graph of Thoughts (GoT) deep research system.

## Agent Overview

### 🎯 Controller Agent

**`research-controller.md`** (18KB)
- **Purpose:** Orchestrates the entire GoT research process
- **Triggers:** "deep research [topic]", "graph of thoughts", "research orchestration"
- **Tools:** Task, Write, Read, TodoWrite
- **Function:** Maintains graph state, deploys transformation agents, implements pruning and scoring

### 📋 Planning Agent

**`Planner.md`** (12KB)
- **Purpose:** Decomposes topics into structured research plans
- **Triggers:** "research planning", "topic decomposition", "research strategy"
- **Tools:** Write, Read
- **Function:** Creates 5-7 research questions, hierarchical outline, resource estimates

### 🔍 Research Agent

**`MultiAngleResearcher.md`** (10KB)
- **Purpose:** Execute Generate transformation with multi-angle sourcing
- **Triggers:** "research", "source gathering", "multi-angle sourcing"
- **Tools:** WebSearch, WebFetch, Bash, Write
- **Function:** Dynamic query rewriting, multi-domain searches, A-E source quality rating

### 🔗 Synthesis Agent

**`CoDSynthesizer.md`** (9.6KB)
- **Purpose:** Aggregate transformation with Chain-of-Density
- **Triggers:** "synthesis", "merge thoughts", "chain-of-density", "aggregate"
- **Tools:** Pure LLM (no external tools needed)
- **Function:** 5-iteration CoD algorithm, citation preservation, contradiction resolution

### ✅ Verification Agent

**`SAFEVerifier.md`** (13KB)
- **Purpose:** Fact-checking with Search-Augmented Factuality Evaluation
- **Triggers:** "fact checking", "claim verification", "hallucination prevention"
- **Tools:** WebSearch, WebFetch
- **Function:** Atomic claim extraction, adversarial query generation, 95% accuracy threshold

### 📄 Finalization Agent

**`ReportFinalizer.md`** (14KB)
- **Purpose:** Transform verified research into final formatted report
- **Triggers:** "report assembly", "citation formatting", "bibliography generation"
- **Tools:** Read, Write, Bash
- **Function:** Sentence-level citations, executive summary, comprehensive bibliography

