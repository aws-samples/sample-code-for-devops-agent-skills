# CRM Production Investigation Guidelines Skill (Sample)

This is a **sample skill** demonstrating how to write production investigation guidelines for the AWS DevOps Agent **Incident Triage** agent type. It shows how to encode application-specific troubleshooting knowledge — architecture details, incident isolation rules, and investigation procedures — into a skill that guides the agent during production incidents.

> **Note:** This skill is specific to a fictional CRM application and cannot be reused as-is. Use it as a template for writing similar investigation guideline skills for your own applications.

## Purpose

When a production incident occurs, investigation agents might need application-specific context that goes beyond generic AWS troubleshooting: what services make up the application, what the common failure modes are, how to isolate unrelated incidents, and what observability tools to use. This sample skill demonstrates how to encode that knowledge so the Incident Triage agent can conduct thorough, structured investigations without repeated human guidance.

## Key Capabilities (Demonstrated)

- Defining investigation isolation rules to prevent conflating unrelated incidents
- Specifying known incident titles and their corresponding failure domains
- Providing a structured investigation approach (symptoms → logs → audit trail → correlation → root cause → remediation)
- Documenting application architecture so the agent understands service relationships
- Listing common root cause patterns with specific AWS API calls to check

## Prerequisites

- AWS DevOps Agent space
- The target application's observability stack (CloudWatch Logs, CloudWatch Metrics, CloudTrail)
- IAM permissions for the agent to access CloudWatch, CloudTrail, and application-specific resources

## Limitations

- This skill is a sample for a fictional CRM application — it must be adapted to your own architecture before use
- Investigation guidance is only as effective as the observability data available to the agent
- The incident title matching logic assumes a fixed set of known alert titles

## Agent Types

This skill is designed for:

- **Incident Triage** — automated incident investigation and root cause analysis triggered by alert webhooks

## Uploading to AWS DevOps Agent

To deploy this skill (or your adapted version) to your Agent Space:

1. Zip the skill directory (only including allowed extensions):

   ```bash
   cd skills
   zip -r crm-production-investigation-guidelines.zip crm-production-investigation-guidelines/ -i '*.md' '*.txt' '*.json' '*.yaml' '*.yml' '*.xml' '*.csv' '*.tsv' '*.html' '*.htm' '*.png' '*.jpg' '*.jpeg' '*.gif' '*.svg' '*.webp' '*.pdf' -x '*/.claude/*' '*/scripts/*' '*/README.md' '*/.skilleval.yaml' '*/.skilleval.yml' '*/CHANGELOG.md' '*/evals/*'
   ```

2. In the AWS DevOps Agent Operator Web App, navigate to the **Skills** page.
3. Click **Add skill** → **Upload skill**.
4. Drag and drop the zip file (max 6 MB).
5. Select the agent type: **Incident Triage**.
6. Click **Upload**.

For more details, see [Uploading a skill](https://docs.aws.amazon.com/devopsagent/latest/userguide/about-aws-devops-agent-devops-agent-skills.html#creating-skills) in the AWS DevOps Agent User Guide.

## How to Use This Skill

### As a Template

1. Copy the `SKILL.md` file as a starting point for your own application
2. Replace the CRM architecture section with your application's architecture
3. Replace the incident titles with your actual alert titles
4. Update the investigation approach with your organization's runbook procedures
5. Add common root cause patterns specific to your application

### Sample Incident Triage Prompts (for this demo application)

- "Investigate the SQS Message Backlog Spike alert"
- "We received an alert for High Lambda Error Rate, all invocations failing"
- "Triage the RDS Read Latency/CPU Utilization High incident"
- "What changed in the CRM Lambda function in the last hour?"
- "Check CloudTrail for IAM permission changes affecting the CRM application"
