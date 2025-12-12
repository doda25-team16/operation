# Extension Proposal: Automated Rollback Strategy

## 1. Release Engineering Shortcoming
**Issue:**
Our current project uses Istio for traffic splitting (90/10) to introduce new versions. However, there is no automated mechanism to handle failures. If the new version (receiving 10% of traffic) has errors, it continues to run until someone manually detects the issue and reverts the configuration.

**Effect:**
This lack of automation leads to prolonged user impact during bad releases. The recovery time depends entirely on human reaction speed and the need for someone to constantly monitor the system, which is a significant shortcoming in our Release Engineering process.

## 2. Proposed Extension
**Solution:**
Implement **Automated Rollback** using the **Flagger** Kubernetes operator.

This extension connects to our **Deployment and Experimentation** assignments. Instead of manually applying `VirtualService` weights, we will configure Flagger to:
1.  Monitor the Prometheus metrics we established.
2.  Automatically revert traffic to the stable version if the error rate exceeds a safety threshold.

This changes our deployment from a manual process to a managed workflow.

## 3. Implementation Plan
To implement this, we will:
1.  **Install Flagger:** Deploy the Flagger operator in the `istio-system` namespace.
2.  **Connect Metrics:** Configure Flagger to query our existing Prometheus service (`http://prometheus.monitoring:9090`).
3.  **Define Rollout Resource:** Replace our static `k8s/istio.yml` configuration with a Flagger resource that defines the rollback thresholds (e.g., `500ms` max latency).

## 4. References
* **AWS Builders' Library:** *Automating safe, hands-off deployments* - Describes strategies for removing manual intervention from deployments using automated safety checks and rollbacks. [https://aws.amazon.com/builders-library/automating-safe-hands-off-deployments/](https://aws.amazon.com/builders-library/automating-safe-hands-off-deployments/)
* **Flagger Documentation:** *Automated Rollbacks* - Technical details on how the rollback mechanism works with Istio. [https://docs.flagger.app/usage/rollback](https://docs.flagger.app/usage/rollback)