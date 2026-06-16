name: Enterprise AI Skill Operating Framework (EASOF)
description: Design, govern, operate, secure, measure, and scale AI skills across a federated enterprise portfolio.

# Enterprise AI Skill Operating Framework (EASOF)

## Goal
Create and manage a portfolio of skills that are:
- **Federated & Scalable**: Designed for multi-BU governance, domain-based routing, and cross-regional deployment.
- **Testable & Observable**: Equipped with automated harnesses, deep telemetry, and SRE-grade reliability practices.
- **Governable & Risk-Aware**: Enforced through strict Quality Gates, multidimensional risk scoring, and dimensional security.
- **Financially Accountable**: Governed by AI FinOps with chargeback/showback, ROI tracking, and budget controls.
- **Business-Aligned**: Directly mapped to enterprise business capabilities and organizational structure.

---

## Part 1: Enterprise Architecture & Federated Topology

### 1.1 Hierarchical Routing
1. **Composable Skill**: Atomic, single-responsibility.
2. **Domain Router**: Routes intents within a specific business domain (e.g., "HR Router", "Finance Router").
3. **Portfolio/Global Router**: Top-level entry point. Classifies high-level intent and delegates to Domain Routers.
4. **Orchestrator Skill**: Chains multiple Composable Skills into a DAG.
5. **Governance/Evaluator Skill**: "LLM-as-a-judge". Audits output, security, or compliance.

### 1.2 Multi-BU Federation
For organizations with multiple Business Units (BUs):
- **BU Autonomy**: Each BU can define its own Domain Routers and Composable Skills within guardrails.
- **Central Governance**: A central Architecture Board defines global standards, security policies, and shared Portfolio Routers.
- **Cross-BU Discovery**: A federated Skill Registry allows BUs to discover and reuse skills from other BUs (with approval).
- **Cross-Region Deployment**: Skills can be deployed in multiple regions with region-specific compliance tags (e.g., "EU-GDPR", "US-SOC2").

---

## Part 2: Skill Maturity & Dimensional Model

### 2.1 Maturity Levels
| Level | Name | Scope | Governance Requirement |
| :--- | :--- | :--- | :--- |
| **L1** | Draft | Personal/Local | Manual testing. No PII. |
| **L2** | Operational | Team/Department | Automated harness. Basic prompt shielding. |
| **L3** | Managed | Cross-Department / BU | CI/CD integrated. Full security & risk review. SRE Runbook required. |
| **L4** | Enterprise | Global Organization / Multi-BU | High Availability. FinOps chargeback. Architecture Board approval. |
| **L5** | Strategic | Mission-Critical | Disaster Recovery. Continuous Compliance Audits. C-Level Sponsor. |

### 2.2 Dimensional Model (For L3+ Skills)
- **Security Posture**: `Public` | `Internal` | `Confidential` | `Restricted`
- **Compliance Scope**: `None` | `GDPR` | `SOC2` | `HIPAA` | `PCI-DSS` | `ISO27001`
- **Availability Tier**: `Tier 1 (Critical/HA)` | `Tier 2 (Standard)` | `Tier 3 (Best Effort/Batch)`
- **Deployment Region**: `Global` | `EU` | `US` | `APAC` | `Region-Specific`

### 2.3 Realistic Latency Targets (P95/P99)
- **Interactive**: P95 < 3s, P99 < 5s.
- **Near Real-Time**: P95 < 10s, P99 < 20s.
- **Batch/Async**: No latency SLA. Define **Throughput SLA** (e.g., 1000 docs/hour).

---

## Part 3: SRE & Reliability Engineering

### 3.1 SLOs and Error Budgets
Every L3+ skill must define:
- **SLO (Service Level Objective)**: Target reliability (e.g., 99.9% successful executions over 30 days).
- **Error Budget**: The acceptable failure margin (e.g., 0.1% = ~43 minutes of downtime/month).
- **Error Budget Policy**: 
  - If budget > 50%: Normal feature development.
  - If budget < 50%: Freeze non-critical changes, focus on reliability.
  - If budget exhausted: Halt all non-emergency deployments until reliability improves.

### 3.2 Incident Management Metrics
- **MTTD (Mean Time to Detect)**: How quickly is a skill failure detected? (Target: < 5 min for L4/L5).
- **MTTR (Mean Time to Recover)**: How quickly is the skill restored? (Target: < 1h for L4/L5).
- **Incident Severity Matrix**:
  - **Sev-1**: Complete outage of L5 skill. All hands. CEO notified.
  - **Sev-2**: Degraded performance of L4/L5 skill. SRE + Dev on-call.
  - **Sev-3**: L3 skill failure or L4 non-critical issue. Dev team notified.
  - **Sev-4**: Minor issue, no user impact. Tracked for next sprint.

### 3.3 Observability & Telemetry
Must emit structured JSON logs: `skill_id`, `execution_trace`, `token_usage`, `estimated_cost_usd`, `latency_ms`, `trigger_confidence_score`, `error_codes`, `slo_contribution` (success/failure for SLO calculation).

---

## Part 4: Multidimensional Risk Scoring

### 4.1 Risk Dimensions (Mandatory for L2+)
Calculate a **Composite Risk Score** across 4 dimensions (each scored 1-5):
- **Business Risk**: Financial/reputational damage if the skill fails or hallucinates.
- **Security Risk**: Likelihood and impact of prompt injection, data leakage, or unauthorized access.
- **Compliance Risk**: Regulatory violations (GDPR, HIPAA, etc.) if the skill mishandles data.
- **Operational Risk**: Complexity of the skill, dependency on external APIs, likelihood of edge-case failures.

### 4.2 Composite Score Calculation
- **Criticality** = (Business × 2) + (Security × 2) + Compliance + Operational
  - *Score 1-20*: Low Risk (Standard review).
  - *Score 21-50*: Medium Risk (Security team review required).
  - *Score 51-80*: High Risk (CISO/Architecture Board approval required).

---

## Part 5: AI FinOps & Economic Model

### 5.1 Cost Metrics & Chargeback/Showback
- **Cost per Execution**: Track average and P95 cost per invocation.
- **Monthly Cost per User/BU**: Aggregate costs by Business Unit for chargeback.
- **Showback Dashboard**: Provide BUs with visibility into their AI spend without direct billing.
- **Chargeback Model** (for L4+): Directly bill BUs for their AI consumption based on usage.

### 5.2 Budget Controls & ROI
- **Budget Caps**: Hard caps per skill, per BU, and globally. Automated alerts at 80%, hard stop at 100%.
- **ROI Tracking**: Compare `Total Monthly Spend` against the `Value Metric` (from Capability Mapping).
- **Optimization**: Quarterly reviews for prompt compression, caching, or model downgrading.

---

## Part 6: Capability Model & Business Alignment
- **Capability Domain** (e.g., "Human Resources") → **Business Capability** (e.g., "Employee Onboarding") → **Skill(s)** → **Business Owner** (BU-level) → **Value Metric** (e.g., "Hours saved/month").

---

## Part 7: Formal Quality Gates (Q-Gates)
- **Gate 1: Architecture & Design Gate**: Requires approved `SKILL.md`, Capability Mapping, Risk Score, and Multi-BU impact assessment. (Approver: Tech Lead / Product Owner / BU Architect).
- **Gate 2: Security & Compliance Gate**: Requires completed threat model, prompt injection testing, PII validation, and regional compliance tags. (Approver: SecOps / DPO / Regional Compliance Officer).
- **Gate 3: Benchmark & Performance Gate**: Requires 100% pass rate on Automated Test Harness, meeting all P95 latency, precision/recall, and SLO targets. (Approver: QA / SRE).
- **Gate 4: Production Release Gate**: Requires finalized Runbooks/Playbooks, FinOps chargeback configured, Error Budget policy defined, and successful staging deployment. (Approver: Release Manager / Architecture Board for L4/L5).

---

## Part 8: The Core Lifecycle

### 1. Discovery & Portfolio Check
Query the **Federated Skill Registry**. Define Target Maturity, Dimensional Tags, Hierarchical Placement, and Multi-BU impact.

### 2. Advanced Trigger Engineering
Design a comprehensive trigger matrix:
- **Positive Triggers**: Ground-truth inputs that MUST activate the skill.
- **Negative Triggers**: Semantically similar inputs that MUST NOT activate the skill.
- **Ambiguous Triggers**: Inputs requiring user clarification.
- **Competing Skills Matrix**: Identify overlaps and define explicit **Conflict Resolution Rules**.

### 3. Architecture & Observability Design
Define execution flow, SLOs, Error Budgets, and telemetry schema.

### 4. Test Design & Automated Harness
Generate an executable `test_harness.py` running against `EVALS.json`. Minimum 15 trigger + 10 workflow evaluations.

### 5. Benchmarking & Iteration
Run the Automated Harness. **No simulated data allowed.** Iterate on failures. Avoid overfitting.

---

## Part 9: Complete Lifecycle & Knowledge Management
1. **Deprecation**: Mark as `Deprecated` in Registry. Emit warnings via Router. 90-day sunset.
2. **Migration**: Provide automated mapping to the successor skill.
3. **Archival**: Revoke API keys, move logs to cold storage, delete compute.
4. **Succession**: `OWNERSHIP.md` must name primary and secondary owners. If an owner leaves, the skill drops to L1 (Read-Only) until reassigned.
5. **Knowledge Management**: Maintain living documentation: `RUNBOOK.md` (SRE procedures), `PLAYBOOK.md` (incident response scenarios).

---

## Part 10: Portfolio Governance
- **Federated Skill Registry**: Centralized catalog with BU-level views, searchable by capability, owner, risk score, maturity, and region.
- **Deduplication**: Quarterly automated scans for skills with >80% semantic overlap across BUs.
- **FinOps Review**: Monthly chargeback/showback reviews; quarterly optimization reviews.
- **Architecture Review**: Bi-annual review of L4/L5 skills to assess business value, decommission orphaned skills, and upgrade maturity levels.

---

## Part 11: Packaging & Release Checklist
Before promoting to L2 or higher:
- [ ] Architectural pattern, Maturity Level, Hierarchical placement, and Multi-BU impact defined.
- [ ] Business Capability mapped and Business Owner (BU-level) assigned.
- [ ] Multidimensional Risk Score calculated and Q-Gate 1 & 2 approvals obtained.
- [ ] FinOps chargeback/showback configured and budget caps set.
- [ ] SLOs and Error Budget policy defined.
- [ ] Advanced Trigger Matrix documented and tested.
- [ ] Automated Test Harness (`test_harness.py`) passes all Q-Gate 3 targets.
- [ ] Observability/Telemetry schema validated (including SLO contribution tracking).
- [ ] Security review complete (Prompt injection, PII handling, Least Privilege, Regional compliance).
- [ ] Succession plan and Knowledge Management artifacts documented.

## Final Deliverables
1. `SKILL.md` (Core definition & instructions)
2. `EVALS.json` (Standardized test cases)
3. `test_harness.py` (Executable automated testing script)
4. `TRIGGERS.md` (Positive, Negative, Ambiguous, and Competing matrices)
5. `RISK_ASSESSMENT.md` (Multidimensional risk calculation: Business, Security, Compliance, Operational)
6. `FINOPS_CONFIG.json` (Budget caps, chargeback rules, cost alerts, model routing rules)
7. `SLO_CONFIG.json` (SLO targets, Error Budget policy, alerting thresholds)
8. `RUNBOOK.md` (L3+: SRE operational procedures, rollback, log inspection, MTTR targets)
9. `PLAYBOOK.md` (L4+: Incident response scenarios for specific failure modes, severity matrix)
10. `CHANGELOG.md`, `RELEASE_NOTES.md`, `OWNERSHIP.md`