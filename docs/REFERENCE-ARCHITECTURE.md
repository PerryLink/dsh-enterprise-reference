# Reference architecture (diagram)

```mermaid
flowchart LR
  U[User] -->|SSO| CP[Control plane]
  CP -->|policy, approvals| RT[DSH runtime pool]
  RT --> MG[Model gateway]
  RT --> TG[Tool/MCP gateway]
  TG --> IS[Internal systems]
  RT --> SS[Session store]
  RT --> AS[Artifact store]
  CP --> AU[Audit sink / SIEM]
  RT --> AU
  MG --> AU
  TG --> AU
```

## Responsibilities

| Component | Owns | Does not own |
|---|---|---|
| Control plane | identity, RBAC, policy, plugin allowlist, version pinning | prompt content |
| Runtime | agent loop, tools, sandbox | long-term retention |
| Model gateway | provider routing, quotas, logging | user identity |
| Tool/MCP gateway | credentials, rate limits, kill switch | model choice |
| Audit sink | immutability, retention, export | policy decisions |
