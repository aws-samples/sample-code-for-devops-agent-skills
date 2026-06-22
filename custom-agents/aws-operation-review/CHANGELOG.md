# Changelog

## 1.0.0

- Initial version
- System prompt with Goal/Approach/Constraints/Output structure
- Uses both `eks-operation-review` and `rds-operation-review` skills for domain knowledge
- Requires `use_aws` and `use_kubectl` tools for resource inspection
- Output includes executive summary, findings by category, priority matrix, and next steps
- Supports EKS clusters, RDS instances, and Aurora clusters
- Severity-based prioritization with effort/impact estimates
