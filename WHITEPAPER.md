# Running DSH in the Enterprise: A Reference Architecture (v0.1)

## 1. Executive summary

DSH's design center is **"everything is a plugin"**: the model adapter, tools, session log, sandbox, agent loop and UI are all replaceable. That is a strength for customization and a challenge for governance. An enterprise deployment must add four things the open-source core does not, by design, guarantee:

1. **Identity and permissions** — who can invoke which tool, on which path, with which network.
2. **Audit and evidence** — a tamper-evident record of what the agent did and who approved it.
3. **Integration boundary** — MCP servers and internal APIs with least privilege, rate limits, and kill switches.
4. **Upgrade and rollback** — a controlled path through a fast-moving preview with breaking changes.

This paper describes a vendor-neutral reference for those four pillars. It is intentionally tool-agnostic: you can implement it with an internal platform team, a systems integrator, or a commercial distribution.

## 2. Why DSH is different

| Dimension | Traditional coding assistant | DSH in an enterprise |
|---|---|---|
| Execution | Editor plugin, narrow scope | Agent loop that can read/write files, run commands, call tools |
| Extension | Fixed feature set | Every part is a plugin; third-party plugins are first-class |
| Compatibility | Stable releases | Developer preview; frequent breaking changes |
| Data path | Mostly prompt/response | Files, shell output, MCP tools, session logs, plugin code |
| Trust boundary | The editor | The host process + plugin code + sandbox + network |

The practical consequence: **the plugin is part of your attack surface, and the session log is part of your evidence trail.** Treat both accordingly.

## 3. Reference architecture

```
[ User / SSO ]
      |
[ Control plane ]  policy, RBAC, approvals, plugin registry, audit sink
      |
[ DSH runtime pool ]  per-team workspace, pinned version, sandbox profile
   |        |         |
[ Model ] [ Tools ] [ MCP gateway ] -> internal systems
   |        |         |
[ Session store ] [ Artifact store ] [ Audit log / SIEM ]
```

### Components

- **Control plane**: SSO/SCIM, RBAC, approval policy, plugin allowlist, version pinning, audit configuration.
- **Runtime pool**: one DSH instance/profile per team or project; pinned image; no shared home directory; explicit CPU/memory/disk quotas.
- **Model gateway**: routes to approved providers; logs model/provider/version; supports air-gapped local models.
- **Tool/MCP gateway**: allowlisted MCP servers, per-server credentials, rate limits, timeouts, circuit breakers.
- **Session/artifact store**: encrypted at rest; retention policy; export for e-discovery.
- **Audit sink**: append-only or WORM storage; export to SIEM; links approvals to actions.

## 4. Four pillars

### 4.1 Identity and permissions

- SSO (OIDC/SAML) + SCIM; no local shared accounts.
- Roles: `admin`, `platform-maintainer`, `plugin-author`, `user`, `auditor`.
- Default policy: `workspace-write` + `ask`; deny network by default; explicit allowlists.
- Per-tool capability manifests; destructive commands require a second approval.
- Break-glass account with mandatory post-hoc review.

### 4.2 Audit and evidence

- Log every tool call with actor, session, tool, arguments hash, result hash, approval id.
- Log plugin install/update/uninstall and capability diffs.
- Store logs append-only; export to SIEM; retention per policy (180 days minimum is a common starting point).
- Be able to answer: *what did the agent read, what did it write, who approved it, which model/plugin version was used?*

### 4.3 MCP and integration boundary

- MCP servers are **services**, not plugins: give them their own identity, network policy, and quotas.
- Allowlist servers; pin versions; review tool schemas; rate-limit; timeout; circuit-break.
- Run MCP processes outside the DSH host process where possible.
- Provide a kill switch per server and per tool.

### 4.4 Upgrade and rollback

- Pin a DSH version per team; do not auto-update production.
- Maintain a compatibility matrix (see the HCB project) before upgrading.
- Canary ring: one team → department → company.
- Dry-run session migration; snapshot before upgrade; keep the previous image.
- Gate on plugin compatibility; have a rollback runbook with a target RTO.

## 5. Deployment models

| Model | Best for | Key trade-off |
|---|---|---|
| **Single team** | pilot, 5–20 users | low cost; weak isolation |
| **Team platform** | 20–200 users, multiple projects | needs control plane; moderate cost |
| **Air-gapped** | regulated, defense, finance | local models, offline registry, manual updates |
| **Managed service** | no platform team | vendor trust, data residency |

## 6. Adoption roadmap

1. **Week 0–2: scope** — one workflow, one data source, one integration; pick a success metric.
2. **Week 2–4: guardrails** — SSO, RBAC, network deny, audit sink, plugin allowlist.
3. **Week 4–6: pilot** — 5–10 users; weekly review; collect failures.
4. **Week 6–8: evidence** — case study, cost per task, incident log, go/no-go.
5. **Month 3+: scale** — canary ring, compatibility gate, internal plugin registry.

## 7. What "good" looks like

- A user can explain, in one screen, which tools and data the agent can access.
- An auditor can reconstruct any session from logs without asking the user.
- A plugin upgrade cannot reach production without a compatibility check.
- A failed upgrade can be rolled back within the target RTO.

## 8. Non-goals and disclaimer

This document is not a certification, not legal advice, and not an official DeepSeek roadmap. It is a community reference. Verify everything against your own threat model and regulatory context.
