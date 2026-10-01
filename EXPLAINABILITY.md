# EXPLAINABILITY — Agent Zero Organic Framework

> **Admissibility & Transparency Report for OpenGAP / Agent Passport**  
> *Agent Name:* Agent Zero Organic Framework (`agent-zero`)  
> *Specification:* OpenGAP v0.1.0  
> *Domain:* Developer Tools / Autonomous Generalist Agent & Code Execution  

---

## 1. Overview & Operational Purpose
The **Agent Zero Organic Framework** is an open-source, general-purpose autonomous AI agent architecture designed to solve complex software, system administration, and research tasks by treating terminal execution and code generation as core cognitive capabilities. It enables hierarchical multi-agent delegation, persistent semantic memory, and dynamic tool synthesis.

Its operational purpose is to serve as an extensible autonomous partner that adapts organically to user tasks—writing custom code, deploying specialized subordinate agents, and creating reusable workflow skills on the fly.

---

## 2. How the Agent Decides (Decision-Making Logic)
Agent Zero Organic Framework operates across a deterministic, multi-stage decision pipeline:

```
[Stage 1: Intent & Task Parsing] ──> [Stage 2: Context & Memory Retrieval] ──> [Stage 3: Tool or Code Selection]
                                                                                             │
                                                                                             ▼
[Stage 6: Output & Memory Update] <── [Stage 5: State & Error Verification] <── [Stage 4: Sandboxed Execution]
```

### 2.1 Intent & Task Parsing
- **Decision:** Parse incoming user instructions to identify the core objective, required compute capabilities, and whether sub-agent delegation is beneficial.
- **Rules:** If a task requires isolated parallel computation, plan subordinate agent instantiation; otherwise execute directly.

### 2.2 Context & Memory Retrieval
- **Decision:** Search associative memory for relevant past user instructions, environment quirks, and registered skills.
- **Rules:** Match semantic vector embeddings against the query; inject top-$k$ relevant memories and active skill instructions into model context.

### 2.3 Tool & Code Selection
- **Decision:** Determine whether an existing tool/skill satisfies the request or if an ad-hoc Python/shell script should be written.
- **Rules:** Prefer validated existing tools; if none exist, draft a targeted Python or bash script to accomplish the exact requirement.

### 2.4 Sandboxed Execution & Error Verification
- **Decision:** Run the selected action or script in a sandboxed subprocess and inspect stdout/stderr outputs for success.
- **Rules:** If an error occurs, analyze traceback details, perform a targeted code fix, and re-execute (max 3 retries) before escalating.

---

## 3. Data & Privacy
| Data Category | Retention Policy | Third-Party Sharing | Storage Mechanism |
|---|---|---|---|
| Terminal Session Logs & Output | Session Lifetime | None | Local Output Stream / Filesystem |
| Associative Vector Embeddings | Persistent (Across Sessions) | None | Local ChromaDB / SQLite Store |
| Custom Generated Tools & Skills | Permanent (Local Library) | None | Local `/skills/` and `/tools/` Directories |
| User Profile & Environment State | Permanent User Config | None | Local Configuration JSON / YAML |

Agent Zero Organic Framework complies with operational security and privacy standards:
- **No Cloud Data Exfiltration:** All code executions, terminal commands, and vector memory embeddings remain strictly local to the user's host or private Docker container.
- **Epistemic Isolation:** Subordinate agents operate within dedicated sub-directories and isolated memory contexts, preventing unintended state bleed between tasks.
- **Sanitized Model Payloads:** Prompts dispatched to upstream language models are stripped of local file paths containing credentials, private SSH keys, and system secrets.
- **Data Minimization:** Only relevant context snippets and recent command outputs necessary for the active decision turn are dispatched in model context payloads.

---

## 4. Known Limitations & Failure Modes
Reviewers, auditors, and users should note the following operational constraints:
1. Subprocess Command Hangs
   - *Limitation:* Interactive command-line utilities (e.g., waiting on `sudo` password or interactive prompts) can cause execution threads to hang.
   - *Mitigation:* The runtime enforces strict execution timeouts (default: 60s) and non-interactive environment flags (`DEBIAN_FRONTEND=noninteractive`).
2. Hallucinated Shell Environments
   - *Limitation:* The agent may attempt to invoke command-line utilities not installed in the active operating system.
   - *Mitigation:* The agent performs pre-flight checks using `which` or `command -v` and automatically installs prerequisites or uses Python fallbacks.
3. Memory Context Saturation
   - *Limitation:* Extensive command outputs (e.g., dumping large log files to stdout) can flood model context windows.
   - *Mitigation:* The runtime truncates stdout outputs to a maximum character buffer and summarizes lengthy responses.
4. Recursive Sub-agent Drift
   - *Limitation:* Subordinate agents given vague objectives can deviate from the parent task scope.
   - *Mitigation:* The runtime enforces explicit sub-agent task prompts with defined return schemas and terminates off-topic delegation.

---

## 5. Verification, Safety & Human Oversight
Agent Zero Organic Framework integrates multi-layer safety rails to ensure full human accountability and system integrity:
- **Real-Time Human Approval Gate:** Operators can configure approval gates for shell commands matching sensitive patterns (e.g., file deletion, network socket binding).
- **Emergency Session Interrupt:** Any running code execution, terminal loop, or subordinate agent can be killed instantly via keyboard interrupt (`Ctrl+C`) or web UI pause.
- **Step Quota Guardrails:** Strict step limits (`max_iterations`) prevent runaway reasoning loops and uncontrolled API token consumption.
- **Structured Audit Logging:** Every executed bash command, generated python file, sub-agent communication, and model reasoning trace is stored in timestamped audit logs.
