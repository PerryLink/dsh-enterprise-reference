# Enterprise DSH Checklist (80 items)

> Score each item: 0 = absent, 1 = partial, 2 = implemented and evidenced.
> Target: 80/160 for pilot, 120/160 for production, 150/160 for regulated.

## A. Architecture & boundaries

| # | Check | Score (0/1/2) | Evidence |
|---|---|---|---|
| A1 | DSH runtime is isolated from the host network by default (deny egress, explicit allowlist). |  |  |
| A2 | Each team/project gets a separate profile/workspace; no shared home directory. |  |  |
| A3 | Control plane (policy/RBAC/audit) is separated from the data plane (runtime/sessions). |  |  |
| A4 | Model traffic goes through an approved gateway, not directly from the runtime. |  |  |
| A5 | Session and artifact storage are encrypted at rest and backed up. |  |  |
| A6 | Plugin execution boundary is documented (same process vs worker vs container). |  |  |
| A7 | Resource quotas (CPU/memory/disk/PIDs) are set per runtime. |  |  |
| A8 | RPO/RTO for sessions and artifacts are defined and tested. |  |  |

## B. Identity & access

| # | Check | Score (0/1/2) | Evidence |
|---|---|---|---|
| B1 | SSO via OIDC or SAML; no shared local accounts. |  |  |
| B2 | SCIM provisioning and deprovisioning are automated. |  |  |
| B3 | RBAC roles are defined: admin, platform-maintainer, plugin-author, user, auditor. |  |  |
| B4 | Service accounts are per project and rotated. |  |  |
| B5 | MFA is enforced for admin and maintainer roles. |  |  |
| B6 | Session lifetime and refresh policy are enforced. |  |  |
| B7 | Offboarding removes access within 24 hours. |  |  |
| B8 | Break-glass access exists and is reviewed after every use. |  |  |

## C. Permissions & sandbox

| # | Check | Score (0/1/2) | Evidence |
|---|---|---|---|
| C1 | Default policy is workspace-write + ask, not full access. |  |  |
| C2 | Filesystem paths are allowlisted; home and system paths are denied. |  |  |
| C3 | Destructive commands (delete/format/force-push) require explicit approval. |  |  |
| C4 | Network access is deny-by-default with per-domain allowlist. |  |  |
| C5 | Secrets are redacted from logs and tool output. |  |  |
| C6 | Each plugin declares a capability manifest; capabilities are shown at install. |  |  |
| C7 | Capability diffs are shown on plugin upgrade. |  |  |
| C8 | Sandbox escape tests are part of the security review. |  |  |

## D. Audit & evidence

| # | Check | Score (0/1/2) | Evidence |
|---|---|---|---|
| D1 | Every tool call logs actor, session, tool, args hash, result hash, approval id. |  |  |
| D2 | Model provider/model version is logged per request. |  |  |
| D3 | Plugin install/update/uninstall events are logged. |  |  |
| D4 | Approval decisions and approver identity are logged. |  |  |
| D5 | Logs are append-only or WORM; tampering is detectable. |  |  |
| D6 | Logs export to SIEM or a central platform. |  |  |
| D7 | Retention policy is defined (>=180 days recommended as a baseline). |  |  |
| D8 | Auditors can reconstruct a session without user cooperation. |  |  |

## E. Data & privacy

| # | Check | Score (0/1/2) | Evidence |
|---|---|---|---|
| E1 | Data classification (public/internal/confidential/restricted) is defined. |  |  |
| E2 | PII is redacted or minimized before it reaches the model. |  |  |
| E3 | Contract prohibits training on customer data (or documents opt-in). |  |  |
| E4 | Data residency/region is configured and documented. |  |  |
| E5 | Backup and restore are tested quarterly. |  |  |
| E6 | Delete/export-on-request workflow exists. |  |  |
| E7 | Secrets live in a vault, not in config files or prompts. |  |  |
| E8 | DLP checks run on egress and file writes. |  |  |

## F. MCP & integrations

| # | Check | Score (0/1/2) | Evidence |
|---|---|---|---|
| F1 | MCP servers are allowlisted and version-pinned. |  |  |
| F2 | Each MCP server has its own identity and credentials (no shared token). |  |  |
| F3 | Tool schemas are reviewed before enabling a server. |  |  |
| F4 | Rate limits and quotas are set per server/tool. |  |  |
| F5 | Timeouts and circuit breakers are configured. |  |  |
| F6 | MCP processes run outside the DSH host process where possible. |  |  |
| F7 | A per-server kill switch exists. |  |  |
| F8 | Integration changes go through change management. |  |  |

## G. Plugin supply chain

| # | Check | Score (0/1/2) | Evidence |
|---|---|---|---|
| G1 | Plugin signature or hash pinning is enforced. |  |  |
| G2 | Install scripts are blocked by default; exceptions are reviewed. |  |  |
| G3 | New plugins pass a review checklist (permissions, network, data). |  |  |
| G4 | An internal registry mirror is used for production. |  |  |
| G5 | Plugin CVEs are monitored and patched on a defined SLA. |  |  |
| G6 | Vendor security questionnaires are collected for critical plugins. |  |  |
| G7 | A malicious-plugin incident response plan exists. |  |  |
| G8 | Unused plugins are removed quarterly. |  |  |

## H. Deployment & operations

| # | Check | Score (0/1/2) | Evidence |
|---|---|---|---|
| H1 | Reference hardware/VM sizing is documented. |  |  |
| H2 | Container or VM isolation is used for the runtime. |  |  |
| H3 | Health checks and readiness probes are configured. |  |  |
| H4 | Metrics, logs, and traces are collected. |  |  |
| H5 | Backup schedule and restore drill are documented. |  |  |
| H6 | Disaster recovery runbook is tested. |  |  |
| H7 | Change management covers config and plugin changes. |  |  |
| H8 | Capacity planning is reviewed monthly. |  |  |

## I. Upgrade & rollback

| # | Check | Score (0/1/2) | Evidence |
|---|---|---|---|
| I1 | DSH version is pinned per team; production does not auto-update. |  |  |
| I2 | A compatibility matrix is checked before upgrade. |  |  |
| I3 | Canary ring (team → department → company) is used. |  |  |
| I4 | Session migration is dry-run on a copy. |  |  |
| I5 | A snapshot/image of the previous version is retained. |  |  |
| I6 | Plugin compatibility gates the rollout. |  |  |
| I7 | Maintenance window and communication plan are defined. |  |  |
| I8 | Post-upgrade verification checklist is executed and signed. |  |  |

## J. Compliance & procurement

| # | Check | Score (0/1/2) | Evidence |
|---|---|---|---|
| J1 | Data processing agreement / terms are reviewed. |  |  |
| J2 | A security questionnaire pack is ready for procurement. |  |  |
| J3 | SOC2/等保/ISO mapping is documented where applicable. |  |  |
| J4 | Audit reports and penetration test summaries are available under NDA. |  |  |
| J5 | Subprocessor list is maintained. |  |  |
| J6 | Incident notification SLA is defined. |  |  |
| J7 | Export control/sanctions review is completed for the deployment region. |  |  |
| J8 | An internal AI usage policy covers DSH specifically. |  |  |

## Scoring

- **Pilot**: >=80/160, no item in C/D/G at 0.
- **Production**: >=120/160, all C/D/G items >=1.
- **Regulated**: >=150/160, all items >=1 and critical items =2.

Critical items: A1, A3, C1, C3, D1, D5, G1, G2, I2, I5.
