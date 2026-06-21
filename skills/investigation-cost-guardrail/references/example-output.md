# Example Output

The default threshold is **$10.00**: investigations proceed automatically when the estimate is under it, and cancel (requesting approval) when it is exceeded. A missing time window always cancels regardless of cost. Setting the threshold to `$0` is the most conservative mode (always cancel and show cost before any paid query).

## Contents
- PROCEED — estimate within threshold
- CANCEL — estimate exceeds threshold
- CANCEL — no time window provided
- Conservative mode ($0 threshold)
- Partial capability (APIs blocked)

## PROCEED — estimate within threshold

```
📋 INVESTIGATION COST ESTIMATE
═══════════════════════════════════════════════════════════
Target:        ECS task crashes on payments-api
Time Window:   2026-06-20 14:00–14:30 UTC (30 min)
Region:        us-east-1 (same as Agent Space, no transfer)
Threshold:     $10.00 (default)

Step                                      Data        Cost
Logs Insights  /aws/ecs/payments-api      6.4 GB      $0.0320
Logs Insights  /aws/ecs/payments-worker   2.1 GB      $0.0105
GetMetricData  6 metrics × 30 periods     180 req     $0.0018
X-Ray traces   ~18,000                    18,000      $0.0090
───────────────────────────────────────────────────────────
TOTAL ESTIMATED COST: $0.0533

✅ PROCEEDING — $0.05 is within the $10.00 threshold.
```

## CANCEL — estimate exceeds threshold

```
📋 INVESTIGATION COST ESTIMATE
═══════════════════════════════════════════════════════════
Target:        Intermittent platform latency, last 7 days
Time Window:   2026-06-13 → 2026-06-20 (7 days)
Region:        eu-west-1 workload, us-east-1 Agent Space
Threshold:     $10.00 (default)

Step                                      Data        Cost
Logs Insights  5 log groups, 7-day window 2,850 GB    $14.2500
GetMetricData  40 metrics × 2,016 periods 80,640 req  $0.8064
X-Ray traces   ~3,200,000                 3.2M        $1.6000
Cross-region   results us-east-1←eu-west-1 ~85 GB     $1.7100
───────────────────────────────────────────────────────────
TOTAL ESTIMATED COST: $18.37

🚫 CANCELLED — estimated cost ($18.37) exceeds the $10.00 threshold.

💡 Suggestions to reduce cost:
  → Narrow to a 1-hour window around the spike → ~$0.12
  → Target the specific service instead of all 5 log groups
  → Provide the exact error/latency signature → use FilterLogEvents (free)
  → "Same region only" → removes the $1.71 cross-region transfer

Approve to proceed at $18.37, or provide a narrower scope.
```

## CANCEL — no time window provided

```
📋 INVESTIGATION COST ESTIMATE
═══════════════════════════════════════════════════════════
Target:        "Investigate why the API is slow" (no time window)
Region:        us-east-1
Threshold:     $10.00 (default)

Step                                      Data (stored)   Cost (worst case)
Logs Insights  /aws/ecs/prod-api          45.2 GB         $0.2260
Logs Insights  /aws/ecs/prod-worker       89.7 GB         $0.4485
GetMetricData  4 metrics × 60 periods     240 req         $0.0024
───────────────────────────────────────────────────────────
TOTAL WORST-CASE COST: $0.6769

🚫 CANCELLED — no time window provided (worst-case scan: 134.9 GB).
A missing time window always cancels, regardless of cost.

❓ What's missing to make this cheaper:
  → Exact time the incident happened (e.g., "14:15 UTC today") → reduces scan 90%+
  → Which service is affected (e.g., "ECS prod-api") → narrows to one log group
  → The error message you saw → enables free FilterLogEvents instead of Logs Insights
```

## Conservative mode ($0 threshold)

With the threshold set to `$0`, the guardrail always shows the cost and cancels pending approval before running any paid query, even when the estimate is tiny:

```
📋 INVESTIGATION COST ESTIMATE
═══════════════════════════════════════════════════════════
Target:        ECS crash on prod-api, 14:10–14:15 UTC
Region:        us-east-1
Threshold:     $0.00 (conservative mode)

Step                                      Data        Cost
Logs Insights  /aws/ecs/prod-api          0.041 GB    $0.000205
GetMetricData  4 metrics × 5 periods      20 req      $0.000200
───────────────────────────────────────────────────────────
TOTAL ESTIMATED COST: $0.000405

🚫 CANCELLED — threshold is $0.00; awaiting operator approval.
Raise the threshold (e.g., $10.00) to let low-cost investigations proceed automatically.
```

## Partial capability (APIs blocked)

```
📋 INVESTIGATION COST ESTIMATE
═══════════════════════════════════════════════════════════
Target:        Lambda "order-processor" errors
Time Window:   2026-06-20 14:00 → 15:00 UTC (1 hour)
Region:        us-east-1 (same as Agent Space, no transfer)
Threshold:     $10.00 (default)

┌────────────────────────────────┬──────────┬─────────────┐
│ STEP                           │ DATA     │ COST        │
├────────────────────────────────┼──────────┼─────────────┤
│ CloudWatch Logs Insights       │ 0.52 GB  │ $0.0026     │
│   /aws/lambda/order-processor  │          │             │
├────────────────────────────────┼──────────┼─────────────┤
│ CloudWatch GetMetricData       │ 120 req  │ $0.0012     │
│   4 metrics × 30 periods       │          │             │
├────────────────────────────────┼──────────┼─────────────┤
│ X-Ray Trace Scan               │ ~5,000   │ $0.0025     │
│   (extrapolated from sample)   │          │             │
├────────────────────────────────┼──────────┼─────────────┤
│ CloudTrail LookupEvents        │ N/A      │ $0.00       │
│   BLOCKED BY PLATFORM          │          │ (free API)  │
├────────────────────────────────┼──────────┼─────────────┤
│ Pricing API                    │ N/A      │ N/A         │
│   BLOCKED — using fallback rate│          │             │
└────────────────────────────────┴──────────┴─────────────┘

TOTAL ESTIMATED COST:  $0.0063
THRESHOLD:             $10.00
DATA TRANSFER:         $0.00 (same region)

⚠️ Limitations:
• Pricing rates are fallback estimates; actual regional rates may differ
• CloudTrail access blocked; investigation cannot analyze infrastructure changes

✅ PROCEEDING — cost ($0.0063) is within the $10.00 threshold.
```
