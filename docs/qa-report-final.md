# Final Sprint QA Report — Cybersecurity & Governance Track

- **Author:** Emmanuel Mekhail
- **Tester:** Emmanuel Mekhail
- **Environment:** Local VM (Ubuntu 26.04.1, single-node k3s, OpenTofu)
- **Status:** Week 11 Kickoff — Test Cases Drafted (Execution in Weeks 12–13)

## Test Cases

| Test ID | Control / Criterion | Preconditions | Test Steps | Expected Result | Actual Result | Status |
| --- | --- | --- | --- | --- | --- | --- |
| QA-01 | Criterion 1: Control Matrix completeness | `controls.csv` exists in `capstone/cybersecurity-governance/` | Count rows and verify every row has ID, area, control, test, expected, status, and a valid path to an existing evidence file across all 6 areas. | At least 15 rows; 0 missing fields; 100% of evidence paths exist on disk. | Pending (8 rows drafted in Week 11) | Pending |
| QA-02 | Criterion 2: Image vulnerability reduction | `trivy-image-before.json` saved; updated `week-4-web:v3` image built and imported | Run `trivy image --format json` on `week-4-web:v3` and compare HIGH/CRITICAL counts against `trivy-image-before.json` using `jq`. | HIGH + CRITICAL vulnerability count decreases by at least 1; any remaining CRITICAL findings are logged in `risk-register.md`. | Pending | Pending |
| QA-03 | Criterion 3: Full-history secrets scan | `gitleaks` installed on host | Run `gitleaks` with `--redact` across the entire repository history and inspect JSON report length. | 0 unresolved secret findings; any historical/false-positive findings documented without real secret values. | 0 findings in baseline scan | Pass |
| QA-04 | Criterion 4: RBAC `kubectl auth can-i` verification | Cluster running with dedicated `web` ServiceAccount and `prometheus-server` ServiceAccount | Run at least 8 `kubectl auth can-i` checks across both identities (e.g., `get secrets`, `create pods`, `delete deployments`, `list pods`) and compare against written predictions. | 100% of actual outputs match written predictions; neither identity has unnecessary cluster-admin or secret-read permissions. | Pending | Pending |
| QA-05 | Criterion 4: NetworkPolicy isolation verification | `db-clients` (and/or default-deny) NetworkPolicy applied via OpenTofu | Launch temporary throwaway pods in `default` and another namespace without `app=web` label and attempt TCP connection to `db:5432`; delete test pods afterward. | Both unauthorized connection attempts time out / are blocked; `web` pod connection to `db:5432` succeeds. | Pending | Pending |
| QA-06 | Criterion 5: Repeatable evidence script execution | All scanner tools installed; Prometheus port-forward active | Run `time ./capstone/cybersecurity-governance/collect-evidence.sh` from repo root. | Script completes in under 5 minutes without aborting on non-zero scanner exit codes and creates a new timestamped evidence folder. | Pending | Pending |
| QA-07 | Rebuild Verification (Week 13) | Fresh Ubuntu VirtualBox VM with cloned repo | Follow only top-level `README.md` and `RUNBOOK.md` to stand up k3s, import images, deploy OpenTofu, and run `collect-evidence.sh`. | All services come up healthy, `collect-evidence.sh` generates a fresh timestamped folder, and all `controls.csv` statuses match live results. | Pending | Pending |
