# EXPLAINABILITY.md

This document explains the internal mechanisms, data lineage, operational boundaries, and governance framework of **Agent Zero Organic Framework** (`agent-zero`) in accordance with the **OpenGAP v0.1.0** specification for the **HiDevs GitAgent Passport** clearance pipeline.

> **Agent Name:** Agent Zero Organic Framework (`agent-zero`)  
> **Specification:** OpenGAP v0.1.0  
> **Category / Domain:** Developer Tools / Autonomous Generalist Agent & Code Execution  
> **Compliance Standard:** OpenGAP Checkpoint 2 (Explainability & Decision Governance), OWASP LLM Top 10, MITRE ATLAS  

---

## How the Agent Decides

The Agent Zero Organic Framework is an open-source, general-purpose autonomous AI agent architecture designed to solve complex software, system administration, and research tasks by treating terminal execution and code generation as core cognitive capabilities. It enables hierarchical multi-agent delegation, persistent semantic memory, and dynamic tool synthesis. Its operational purpose is to serve as an extensible autonomous partner that adapts organically to user tasks—writing custom code, deploying specialized subordinate agents, and creating reusable workflow skills on the fly.

### 1. Decision Architecture

The user prompt intake, hierarchical delegation, dynamic tool synthesis, and sandboxed execution loop operates across a deterministic, five-stage architecture:

```
User Task Directive / Objective (System Admin Task / Software Engineering Goal / Data Extraction)
    │
    ▼
[Stage 1: Intent Parsing & Subordinate Delegation Planning]
    │  - Decomposes high-level objectives into modular technical sub-problems
    │  - Determines whether to execute locally or spawn a subordinate agent persona
    │  - Allocates token quotas, tool access privileges, and execution contexts
    ▼
[Stage 2: Semantic Memory Retrieval & Context Synthesis]
    │  - Queries persistent FAISS / Chroma semantic memory for relevant past experiences
    │  - Retrieves learned tool scripts, system configurations, and past debugging solutions
    │  - Injects verified technical context into the active working prompt
    ▼
[Stage 3: Dynamic Tool Synthesis & Code Generation]
    │  - Writes custom Python scripts or shell one-liners to solve ad-hoc tasks
    │  - Validates script syntax, dependency requirements, and parameter boundaries
    │  - Caches successful utility scripts into persistent skill libraries for reuse
    ▼
[Stage 4: Sandboxed Terminal Execution & Output Evaluation]
    │  - Executes code and commands within isolated Docker or local terminal subprocesses
    │  - Monitors execution exit codes, stdout streams, and error tracebacks
    │  - Iterates through corrective self-debugging loops if execution fails (up to 3 cycles)
    ▼
[Stage 5: Memory Reinforcement & Trajectory Archive]
    │  - Commits successful solutions and learned procedural patterns to semantic memory
    │  - Scrubs private user credentials, local paths, and environment tokens
    │  - Emits clean deliverables and transparent step-by-step logs for developer review
    ▼
Validated Generalist Task Deliverable & Auditable Execution Trajectory Record
```

### 2. Decision Logic & Autonomous Execution Formulations

Agent Zero evaluates subagent delegation, tool synthesis utility, and self-debugging confidence using deterministic mathematical models:

1. **Subagent Delegation Index ($D_{\text{subagent}}$)**:
   $$D_{\text{subagent}} = (w_c \cdot C_{\text{complexity}}) + (w_i \cdot I_{\text{isolation}}) + (w_d \cdot D_{\text{domain}})$$
   where:
   - $C_{\text{complexity}} \in [0, 1]$ represents problem decomposition depth.
   - $I_{\text{isolation}} \in \{0, 1\}$ indicates whether subtask requires isolated memory or credentials.
   - $D_{\text{domain}} \in [0, 1]$ represents need for a specialized persona (e.g., hacker, researcher).
   - Weights: $w_c = 0.40, w_i = 0.35, w_d = 0.25$ ($\sum w_i = 1.0$).

2. **Self-Debugging Convergence Index ($C_{\text{debug}}$)**:
   $$C_{\text{debug}} = 1 - \frac{N_{\text{attempts}}}{\text{max\_retries}}$$
   When $C_{\text{debug}} \le 0$, the agent suspends execution deterministically and requests human developer intervention.

### 3. Thresholding & Refusal Decision Criteria

Agent Zero Organic Framework enforces strict operational safety and integrity boundaries:
- **Refusal to Execute Unaudited Destructive Host Commands**: Commands attempting to format storage partitions, execute destructive file deletions outside the workspace, or disable system security controls are deterministically blocked with code `ERR_DESTRUCTIVE_COMMAND_PROHIBITED`.
- **Refusal to Store Plaintext Credentials in Semantic Memory**: Memory entries containing API keys or private tokens are automatically sanitized before indexing (`ERR_CREDENTIAL_STORAGE_REFUSED`).
- **Turn Ceiling Enforcement**: Autonomous execution loops enforce a hard ceiling of `max_turns: 25` to eliminate runaway recursion (`WARN_TURN_BUDGET_REACHED`).
- **Container Sandbox Confinement**: Commands attempting to break out of Docker execution containers or access host sockets are terminated (`ERR_SANDBOX_ESCAPE_PROHIBITED`).

### 4. Fallback Decision Mechanism

Continuous operational problem-solving is maintained through multi-tier fault recovery:
- **Model Cascade Failover**: When the primary foundation model experiences latency spikes or HTTP 429 rate limits, the orchestrator cascades automatically between `claude-3-5-sonnet`, `gpt-4o`, and `gemini-2.0-flash`.
- **Pre-Compiled Tool Fallback**: If dynamic code generation fails to synthesize a working script, the agent falls back to verified standard command-line utilities.
- **Graceful Subordinate Eviction**: If a spawned subordinate agent encounters an unrecoverable exception, the parent agent terminates the child thread cleanly and reassumes control.

### 5. Human-in-the-Loop Governance

Human developers retain complete supervisory direction over agent actions:
- **Explicit Operator Approval Gates**: Applying git commits, writing new files, running terminal installations, or deploying builds requires explicit human confirmation.
- **Emergency Session Kill Switch**: Operators can halt agent execution loops instantly via standard `Ctrl+C` interrupt signals.
- **Transparent Terminal Visibility**: Every terminal command, subprocess exit code, stdout stream, and internal thought trace is displayed in real time for developer oversight.

---

## The Data It Uses

Agent Zero operates under strict privacy, data minimization, and local workspace isolation standards.

### 1. Ingested Input Data

The agent processes only operational assets necessary to fulfill generalist tasks:
- **User Directives**: Natural language goals, shell administration requests, and technical specifications.
- **Terminal Execution Streams**: Standard output, standard error, and exit codes from command invocations.
- **Local Filesystem Assets**: Source code, log files, configuration manifests, and datasets scoped to the workspace.

### 2. Configuration & Reference Data

- **Agent Persona Definitions**: System prompts, behavioral traits, and tool permissions for lead and subordinate agents.
- **Persistent Tool Library**: Cached Python utility scripts and reusable function definitions.
- **Vector Memory Stores**: Embedded vector databases storing verified past experiences and technical snippets.

### 3. Base Model & Inference Lineage

- **Deterministic Execution Engines**: Subprocess runners, Docker container managers, and FAISS vector indices executed natively in Python (100% deterministic with zero LLM variance).
- **Foundation LLMs**: High-capability frontier models (`claude-3-5-sonnet`, `gpt-4o`, `gemini-2.0-flash`) utilized for complex code reasoning, traceback deduction, and procedural planning.
- **Zero Training on Developer Data**: Proprietary terminal sessions, local source code, and private memory stores are never transmitted to external cloud servers or used for model training.

### 4. Data Privacy, Storage, and Retention

- **OWASP LLM & MITRE ATLAS Hardened**: Defended against prompt injection via untrusted command output, privilege escalation, and memory poisoning.
- **Local-Only Working Storage**: All memory stores, generated tools, and execution trajectories reside exclusively on the user's filesystem.
- **Automated PII & Secret Scrubbing**: Environment variables, authentication keys, and user credentials are scrubbed from generation logs.
- **Zero Commercial Monetization**: Developer specifications, scaffolded codebases, and architectural inquiries are never monetized, aggregated, or shared with third parties.

---

## Limitations

Understanding the operational boundaries and technical constraints of Agent Zero is essential for effective deployment.

### 1. Complex GUI-Only Desktop Application Control
- **Limitation**: While powerful in terminal and headless browser environments, driving opaque desktop GUI applications without API hooks is outside core scope.
- **Mitigation**: The agent focuses on CLI, API, and containerized tools, providing clean REST/RPC wrappers where desktop interaction is needed.

### 2. Unbounded Subordinate Delegation Depth
- **Limitation**: Allowing subordinate agents to recursively spawn further subordinates can rapidly compound token expenditures.
- **Mitigation**: The framework enforces a maximum delegation depth ceiling (default: 2 levels) to maintain tight budget control.

### 3. Environment Dependency Conflicts in Dynamic Scripting
- **Limitation**: Dynamically synthesized Python scripts can require external pip libraries not pre-installed in the active environment.
- **Mitigation**: Agent Zero detects missing packages and installs them into isolated virtual environments with operator sign-off.

### 4. Long-Running Daemon Process Monitoring
- **Limitation**: Managing indefinite background server daemons through standard terminal commands can lead to orphan process accumulation.
- **Mitigation**: The runtime assigns process group IDs and automatically terminates background subprocesses upon session exit.

### 5. Multi-User Shared Workspace Concurrency
- **Limitation**: Multiple concurrent users sharing a single Agent Zero instance can experience workspace file collision.
- **Mitigation**: The framework supports session-scoped workspace directories that isolate file modifications per user session.

---

## Summary & Compliance Checklist

| Checkpoint 2 Requirement | Corresponding Section | Status |
| :--- | :--- | :---: |
| **How the agent decides** | [How the Agent Decides](#how-the-agent-decides) | **Covered** |
| - Decision architecture & 5-stage pipeline | Section 1 | Verified |
| - Decision logic & autonomous execution formulations | Section 2 | Verified |
| - Thresholding & refusal decision criteria | Section 3 | Verified |
| - Fallback decision mechanism | Section 4 | Verified |
| - Human-in-the-loop governance & oversight | Section 5 | Verified |
| **The data it uses** | [The Data It Uses](#the-data-it-uses) | **Covered** |
| - Ingested user directives, terminal streams & files | Section 1 | Verified |
| - Configuration, persona definitions & tool libraries | Section 2 | Verified |
| - Base model lineage & deterministic engines | Section 3 | Verified |
| - Data privacy, retention lifecycle & MITRE/OWASP | Section 4 | Verified |
| **Its limitations** | [Limitations](#limitations) | **Covered** |
| - Complex GUI-only desktop application control | Section 1 | Verified |
| - Unbounded subordinate delegation depth | Section 2 | Verified |
| - Environment dependency conflicts in dynamic scripting | Section 3 | Verified |
| - Long-running daemon process monitoring | Section 4 | Verified |
| - Multi-user shared workspace concurrency | Section 5 | Verified |
