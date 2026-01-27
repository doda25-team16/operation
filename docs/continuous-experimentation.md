# Continuous Experimentation

## Experiment Overview
<!-- Describe what you're testing -->

## Changes Implemented
- **Stable Version (0.0.9)**: <!-- What this version does -->
- **Canary Version (0.0.8)**: <!-- What changed || I think yellow button-->
- **Traffic Split**: 90% stable / 10% canary

## Hypothesis
<!-- What is expected to happen? -->

**Falsifiable**: <!-- How can this be proven wrong? -->

## Relevant Metrics
1. **[Metric Name]**: <!-- What is the measured value -->
2. **[Metric Name]**: <!-- What is the measured value -->

## Experiment Setup
- **Duration**: <!-- How long? -->
- **Access**: `http://team16-sms.local` <!--Is there a way to force the canary to load?-->
- **Monitoring**: Prometheus (1s scrape) + Grafana dashboard

## Decision Process

### Decision Criteria:
| Metric | Success Criteria |
|--------|------------------|
| <!-- metric --> | <!-- target --> |

### Decision:
- **Accept**: <!-- When? -->
- **Reject**: <!-- When? -->

### Rollback (Manual): <!-- The experimentation doesnt auto fallback right? If so imidiatly message Job so he can change the extension.md-->
Update `values.yaml` → change `canaryWeight: 0` → run `helm upgrade`

## Screenshot
<!-- Add Grafana dashboard screenshot here -->
