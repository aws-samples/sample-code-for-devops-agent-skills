# AWS Pricing Reference for Investigation Cost Estimation

## Pricing API usage

You MUST query the AWS Pricing API at runtime to get accurate per-unit rates for the workload region. The Pricing API is free and runs only in us-east-1 or ap-south-1.

If the Pricing API fails (permissions, timeout), fall back to the floor estimates listed below and flag with ⚠️ in the visual plan.

## CloudWatch Logs Insights

```bash
aws pricing get-products \
  --service-code AmazonCloudWatch \
  --filters "Type=TERM_MATCH,Field=regionCode,Value=<WORKLOAD_REGION>" \
            "Type=TERM_MATCH,Field=usagetype,Value=<REGION_PREFIX>-DataScanned-Bytes" \
  --region us-east-1
```

- **Fallback floor**: $0.005/GB
- **Formula**: `scan_gb × regional_rate`

## CloudWatch GetMetricData

```bash
aws pricing get-products \
  --service-code AmazonCloudWatch \
  --filters "Type=TERM_MATCH,Field=regionCode,Value=<WORKLOAD_REGION>" \
            "Type=TERM_MATCH,Field=usagetype,Value=<REGION_PREFIX>-MetricMonitoring" \
  --region us-east-1
```

- **Fallback floor**: $0.01/1,000 metrics requested
- **Formula**: `(num_metrics × num_periods) / 1000 × regional_rate`

## X-Ray

```bash
aws pricing get-products \
  --service-code AWSXRay \
  --filters "Type=TERM_MATCH,Field=regionCode,Value=<WORKLOAD_REGION>" \
            "Type=TERM_MATCH,Field=usagetype,Value=<REGION_PREFIX>-TracesScanned" \
  --region us-east-1
```

- **Fallback floor**: $0.50/1,000,000 traces
- **Formula**: `estimated_traces / 1,000,000 × regional_rate`

## CloudWatch Contributor Insights

```bash
aws pricing get-products \
  --service-code AmazonCloudWatch \
  --filters "Type=TERM_MATCH,Field=regionCode,Value=<WORKLOAD_REGION>" \
            "Type=TERM_MATCH,Field=usagetype,Value=<REGION_PREFIX>-ContributorInsights-MatchingEvents" \
  --region us-east-1
```

- **Fallback floor**: $0.02/rule/1,000 matching events
- **Formula**: `num_rules × (matching_events / 1000) × regional_rate`

## CloudWatch Live Tail

```bash
aws pricing get-products \
  --service-code AmazonCloudWatch \
  --filters "Type=TERM_MATCH,Field=regionCode,Value=<WORKLOAD_REGION>" \
            "Type=TERM_MATCH,Field=usagetype,Value=<REGION_PREFIX>-LiveTail-SessionTime" \
  --region us-east-1
```

- **Fallback floor**: $0.01/minute/session
- **Formula**: `session_minutes × regional_rate`

## Cross-Region Data Transfer

- **Price**: $0.02/GB (inter-region, consistent across region pairs)
- **What's billed**: only the **result bytes returned** across regions, NOT the scanned volume
- **Estimation**: depends on query type — aggregations/stats return ~nothing (~1% of scan, or a few MB); raw event/trace fetches can return up to ~100% of matched bytes. When the query shape is unknown, use `scan_gb × 0.15` as a rough UPPER-BOUND and flag with ⚠️
- **Formula**: `returned_data_gb × $0.02` (use actual returned size when known; 0.15 factor is a fallback only)

No Pricing API call needed — data transfer pricing is consistent.

## Free APIs (no cost, no lookup needed)

- `logs:DescribeLogGroups` — free
- `logs:FilterLogEvents` — free
- `cloudtrail:LookupEvents` — free
- `EC2/ECS/RDS Describe*` calls — free
- `cloudwatch:GetMetricStatistics` — negligible ($0.01 per 1,000 requests)

## Region prefix mapping

The `<REGION_PREFIX>` in usagetype filters follows this pattern:

> **us-east-1 caveat:** most us-east-1 usage types carry **no prefix** (e.g., `DataScanned-Bytes`), but some — often newer ones — use a **`USE1-`** prefix (e.g., `USE1-DataScanned-Bytes`). AWS is inconsistent here. For us-east-1, query the **unprefixed** usage type first; if the Pricing API returns no results, **retry with the `USE1-` prefix** before falling back to the floor rate. This avoids a silent floor-fallback when only the prefixed form exists.

| Region | Prefix |
|---|---|
| us-east-1 | (usually none; try `USE1-` if no match) |
| us-east-2 | USE2 |
| us-west-1 | USW1 |
| us-west-2 | USW2 |
| eu-west-1 | EU |
| eu-west-2 | EUW2 |
| eu-west-3 | EUW3 |
| eu-central-1 | EUC1 |
| eu-north-1 | EUN1 |
| ap-southeast-1 | APS1 |
| ap-southeast-2 | APS2 |
| ap-northeast-1 | APN1 |
| ap-northeast-2 | APN2 |
| ap-south-1 | APS3 |
| sa-east-1 | SAE1 |
| ca-central-1 | CAN1 |
| me-south-1 | MES1 |
| af-south-1 | AFS1 |

## Documentation

- [CloudWatch Pricing](https://aws.amazon.com/cloudwatch/pricing/)
- [CloudTrail Pricing](https://aws.amazon.com/cloudtrail/pricing/)
- [X-Ray Pricing](https://aws.amazon.com/xray/pricing/)
- [AWS Data Transfer Pricing](https://aws.amazon.com/ec2/pricing/on-demand/#Data_Transfer)
- [AWS Pricing API](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/price-changes.html)
