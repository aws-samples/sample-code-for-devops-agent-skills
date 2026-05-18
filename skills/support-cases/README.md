# Support Cases Skill

This skill enables the AWS DevOps Agent to retrieve and analyze AWS Support cases during incident investigation, root cause analysis, and operational troubleshooting.

## Purpose

When an operational incident occurs, historical support cases often contain valuable context — prior root causes, proven remediations, and recurring patterns. This skill searches past AWS Support cases by service, time window, severity, and error keywords to surface relevant history that informs the current investigation.

## Key Capabilities

- Retrieve open and resolved AWS Support cases via the Support API
- Search and filter cases by service, severity, time range, and keywords
- Review case communications for root cause statements and remediation steps
- Correlate historical findings with current incident symptoms
- Identify recurring patterns that may require permanent fixes

## Prerequisites

- AWS account with Business, Enterprise On-Ramp, or Enterprise Support plan
- Agent permissions for `support:DescribeCases` and `support:DescribeCommunications`

## Limitations

- Support case data is only available for up to 24 months after creation

## Agent Types

This skill is used by the following agent types:

- **Chat tasks** — conversational support case lookup and analysis
- **Incident RCA** — automated root cause analysis during active incidents

## Uploading to AWS DevOps Agent

To deploy this skill to your Agent Space:

1. Zip the `support-cases/` directory:

   ```bash
   cd skills
   zip -r support-cases.zip support-cases/
   ```

2. In the AWS DevOps Agent Operator Web App, navigate to the **Skills** page.
3. Click **Add skill** → **Upload skill**.
4. Drag and drop the `support-cases.zip` file (max 6 MB).
5. Select the agent types: **Chat tasks** and **Incident RCA**.
6. Click **Upload**.

For more details, see [Uploading a skill](https://docs.aws.amazon.com/devopsagent/latest/userguide/about-aws-devops-agent-devops-agent-skills.html#creating-skills) in the AWS DevOps Agent User Guide.

## How to Use This Skill

This skill is most suitable for chat and investigation. Below are sample prompts for each use-case

### Chat

- "Show me all support cases opened in the last 30 days for RDS."
- "What was the resolution for case-123456789010-muen-2024?"
- "Are there any open critical-severity cases in this account?"
- "Summarize the communications on our most recent EC2 support case."
- "Have we filed any support cases related to Lambda throttling this quarter?"

### Investigation

- "We're seeing 5xx errors on our ALB — can you investigate?"
- "RDS connections are maxing out and queries are timing out. What's going on?"
- "There's a latency spike on our ECS service starting around 3am UTC."
- "Our last deployment triggered elevated error rates across multiple endpoints."
- "The same DynamoDB throttling alarm fired again. Help me figure out why this keeps happening."
