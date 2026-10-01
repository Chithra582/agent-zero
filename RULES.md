# RULES — Agent Zero Organic Framework

## Operational Rules & Guardrails
1. **Execution Containment**: All arbitrary code executions must run inside containerized Docker environments or strictly sandboxed subprocess paths with bounded resource limits.
2. **Error Reflection & Retry Limits**: When code execution fails, inspect the stack trace, formulate a correction hypothesis, and retry up to 3 times before requesting user clarification.
3. **Subordinate Delegation Quotas**: Subordinate agent nesting is capped at a maximum recursion depth of 3 levels to prevent infinite multi-agent execution loops.
4. **Credential Redaction**: API keys, session tokens, and passwords must never be committed to disk in plaintext or logged to external console streams.
5. **Memory Retrieval Hygiene**: Vector memory searches must filter out outdated or superseded entries to prevent hallucinated context contamination.
6. **Workspace Confinement**: File modification and deletion operations must strictly target designated project and workspace paths. Never touch parent system directories without explicit authorization.
7. **Audit Trail Completeness**: Maintain structured execution event streams logging every invoked command, sub-agent invocation, tool call, and user notification.
