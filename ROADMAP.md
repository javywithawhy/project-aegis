# Project Aegis ISSO Roadmap

## Purpose

This roadmap defines the order in which Project Aegis will be converted from a general cybersecurity homelab into a simulated government information system managed through the Risk Management Framework.

Work will proceed incrementally. Each milestone must produce usable documentation, evidence, and lessons learned before the project advances.

---

## Phase 1 — System Understanding

### Objective

Define what Project Aegis is, what it supports, what belongs inside the authorization boundary, and how information moves through the environment.

### Deliverables

- [x] Project README and system identity
- [ ] System description
- [ ] Asset inventory
- [ ] Network diagram
- [ ] Authorization-boundary statement and diagram
- [ ] Data-flow diagram
- [ ] External-services and dependencies list

### Exit Criteria

- System purpose and scope are unambiguous.
- In-scope and out-of-scope components are identified.
- All active assets are inventoried.
- Network and data flows are documented.
- The authorization boundary is defensible.

---

## Phase 2 — Security Documentation

### Objective

Create the core documentation needed to describe system operation, users, information types, and security responsibilities.

### Deliverables

- [ ] System Security Plan
- [ ] User-role and privilege matrix
- [ ] Hardware inventory
- [ ] Software inventory
- [ ] Security categorization
- [ ] Data-classification guide
- [ ] Rules of behavior

### Exit Criteria

- System components and responsibilities are documented.
- Information types are identified.
- Confidentiality, integrity, and availability impact levels are justified.
- Privileged and nonprivileged roles are separated.

---

## Phase 3 — Risk Management

### Objective

Identify credible threats, vulnerabilities, mission impacts, and treatment decisions.

### Deliverables

- [ ] Risk-assessment report
- [ ] Risk register
- [ ] Threat assessment
- [ ] Business Impact Analysis
- [ ] Risk-rating methodology
- [ ] Risk-acceptance criteria

### Exit Criteria

- Risks are traceable to assets, threats, and vulnerabilities.
- Likelihood and impact ratings are justified.
- Recommended treatments and ownership are assigned.

---

## Phase 4 — Security Controls

### Objective

Implement and document selected security controls and map them to NIST SP 800-53.

### Control Areas

- [ ] Account management
- [ ] Password and authentication policies
- [ ] Least privilege
- [ ] Administrative separation
- [ ] Logging and audit review
- [ ] Multifactor authentication where practical
- [ ] Firewall configuration
- [ ] Network segmentation
- [ ] Secure remote administration
- [ ] Patch management
- [ ] Backup and recovery
- [ ] Secure configuration baselines

### Deliverables

- [ ] Control-implementation matrix
- [ ] Baseline-configuration documents
- [ ] Control evidence
- [ ] Control-test procedures
- [ ] Exceptions and compensating-control records

### Exit Criteria

- Selected controls have implementation statements.
- Claims are supported by evidence.
- Control gaps are entered in the risk register or POA&M.

---

## Phase 5 — Vulnerability Management

### Objective

Establish a repeatable process for discovering, validating, prioritizing, remediating, and verifying vulnerabilities.

### Deliverables

- [ ] Nessus Essentials deployment
- [ ] Vulnerability-management plan
- [ ] Scan policy
- [ ] Initial authenticated and unauthenticated scans
- [ ] Findings-validation records
- [ ] CVE and risk analysis
- [ ] False-positive documentation
- [ ] Remediation records
- [ ] Verification scans
- [ ] POA&M

### Exit Criteria

- Findings are validated rather than copied directly from a scanner.
- Remediation priority considers technical severity and system impact.
- Closed findings have verification evidence.
- Accepted findings have documented rationale.

---

## Phase 6 — Continuous Monitoring

### Objective

Maintain awareness of system security posture over time.

### Deliverables

- [ ] Continuous-monitoring strategy
- [ ] Weekly review checklist
- [ ] Monthly security review
- [ ] Quarterly control review
- [ ] Annual assessment plan
- [ ] Security metrics and thresholds
- [ ] Recurring reporting template

### Exit Criteria

- Monitoring activities have owners and frequencies.
- Metrics support meaningful risk decisions.
- Changes, vulnerabilities, and control status are tracked over time.

---

## Phase 7 — Incident Response

### Objective

Exercise the ability to identify, analyze, contain, eradicate, recover from, and learn from security incidents.

### Planned Scenarios

- [ ] Ransomware
- [ ] Phishing
- [ ] Lost or stolen laptop
- [ ] Insider threat
- [ ] Malware infection
- [ ] Unauthorized administrator account
- [ ] Data exfiltration

### Required Outputs for Each Exercise

- [ ] Incident report
- [ ] Timeline
- [ ] Containment and recovery actions
- [ ] Root Cause Analysis
- [ ] Evidence list
- [ ] Lessons learned
- [ ] Corrective actions

---

## Phase 8 — Change Management

### Objective

Ensure major system modifications are reviewed, tested, approved, and validated.

### Deliverables

- [ ] Change-management plan
- [ ] Change-request template
- [ ] Security-impact analysis template
- [ ] Testing-plan template
- [ ] Rollback-plan template
- [ ] Approval record
- [ ] Post-change validation record

### Exit Criteria

- Major changes have complete records.
- Security impacts are considered before implementation.
- Validation confirms the change produced the intended result.

---

## Phase 9 — Mock Audit

### Objective

Assess the system as though reviewed by an independent security-control assessor.

### Deliverables

- [ ] Security Assessment Plan
- [ ] Evidence request list
- [ ] Control interviews
- [ ] Technical test results
- [ ] Assessment findings
- [ ] Corrective-action records
- [ ] Security Assessment Report

### Exit Criteria

- Findings are evidence-based.
- Deficiencies are corrected or formally accepted.
- Documentation and technical configuration agree.

---

## Phase 10 — Mock Authorization

### Objective

Present system security posture and residual risk to a simulated Authorizing Official.

### Deliverables

- [ ] Executive summary
- [ ] Final System Security Plan
- [ ] Security Assessment Report
- [ ] POA&M
- [ ] Residual-risk summary
- [ ] Continuous-monitoring strategy
- [ ] Authorization recommendation
- [ ] Mock authorization decision

### Possible Decisions

- Authorization to Operate
- Authorization to Operate with conditions
- Interim authorization
- Denial of authorization

---

## Current Work

**Current phase:** Phase 1 — System Understanding  
**Current milestone:** Repository transition and system definition  
**Next deliverable:** `docs/01-system-understanding/system-description.md`
