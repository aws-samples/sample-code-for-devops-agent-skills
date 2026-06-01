# AWS DevOps Agent Skills

![License: MIT-0](https://img.shields.io/badge/License-MIT--0-blue)
![AWS DevOps Agent](https://img.shields.io/badge/AWS-DevOps%20Agent-orange?logo=amazonaws)

Open-source skills for [AWS DevOps Agent](https://aws.amazon.com/devops-agent/) that extend its capabilities for incident response, root cause analysis, and operational troubleshooting.

Each skill provides domain-specific knowledge, decision trees, and step-by-step runbooks that the agent follows during investigations. Use them as-is to enhance your agent, or as templates for writing your own custom skills.

> **Note:** If you intend to deploy these skills in production, start with a non-production environment first.

## What Are AWS DevOps Agent Skills?

AWS DevOps Agent skills are structured instruction sets that teach the agent how to investigate specific operational scenarios. Skills follow the open [Agent Skills specification](https://agentskills.io/home) and can be uploaded to your DevOps Agent deployment to extend its knowledge beyond built-in capabilities.

Skills enable DevOps Agent to:

- Investigate incidents using domain-specific runbooks and decision trees
- Query AWS APIs (Support, Health, EKS, CloudWatch) with targeted filtering strategies
- Correlate findings across multiple data sources during root cause analysis
- Surface historical patterns, prior resolutions, and recurring issues
- Provide structured recommendations based on best practices

## Available Skills

| Skill | Description | Agent Types | Author | Docs |
|-------|-------------|-------------|--------|------|
| [aws-health-events](skills/aws-health-events/) | Retrieves and analyzes AWS Health events (service issues, scheduled changes, account notifications) to identify AWS-side events that correlate with operational issues | Chat tasks, Incident RCA | [udid-aws](https://github.com/udid-aws) | [README](skills/aws-health-events/README.md) |
| [support-cases](skills/support-cases/) | Searches and analyzes AWS Support cases to find historical incidents with similar symptoms, proven remediations, and recurring patterns | Chat tasks, Incident RCA | [udid-aws](https://github.com/udid-aws) | [README](skills/support-cases/README.md) |
| [eks-operation-review](skills/eks-operation-review/) | Performs comprehensive Amazon EKS operational reviews aligned with the AWS EKS Best Practices Guide covering security, reliability, networking, and scalability | Chat tasks, Prevention | [yakiratz-aws](https://github.com/yakiratz-aws) | [README](skills/eks-operation-review/README.md) |

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/aws-samples/sample-code-for-devops-agent-skills.git
cd sample-code-for-devops-agent-skills
```

### 2. Choose a skill

Browse the skills table above and read the skill's `README.md` for details on its purpose, prerequisites, and sample prompts.

### 3. Upload to AWS DevOps Agent

Zip the skill directory (see the zip command in each skill's README) and [upload it via the AWS DevOps Agent Operator Web App](https://docs.aws.amazon.com/devopsagent/latest/userguide/about-aws-devops-agent-devops-agent-skills.html#creating-skills). Detailed instructions are in each skill's README.

## Skill Directory Structure

Each skill follows a consistent structure based on the [Agent Skills specification](https://agentskills.io/home):

```
skills/<skill-name>/
├── SKILL.md          # Main skill instructions with frontmatter (required)
├── README.md         # Documentation, prerequisites, and upload guide
├── CHANGELOG.md      # Version history
├── evals/            # Evaluation queries and benchmarks
├── assets/           # Images, diagrams, data files (optional)
└── references/       # Supplementary reference docs (optional)
```

The `SKILL.md`, `references/`, and `assets/` directories are what AWS DevOps Agent reads at runtime. Everything else supports development, testing, and documentation.

## Writing Your Own Skills

Want to create custom skills for your operational workflows? See the [AWS DevOps Agent skills documentation](https://docs.aws.amazon.com/devopsagent/latest/userguide/about-aws-devops-agent-devops-agent-skills.html) for the full guide, or use the skills in this repository as templates.

Key principles for effective skills:

- Write step-by-step instructions with clear decision trees
- Include specific AWS API calls and CLI commands the agent should use
- Define filtering strategies and relevance scoring criteria
- Provide expected outputs and success criteria
- Test with the [Agent Skill Eval](https://github.com/aws-samples/sample-agent-skill-eval) framework

## Contributing

We welcome contributions of new skills and improvements to existing ones. See [CONTRIBUTING](CONTRIBUTING.md) for guidelines.

## References

### AWS Documentation

- [AWS DevOps Agent product page](https://aws.amazon.com/devops-agent/)
- [AWS DevOps Agent User Guide](https://docs.aws.amazon.com/devopsagent/latest/userguide/about-aws-devops-agent.html)
- [AWS DevOps Agent API Reference](https://docs.aws.amazon.com/devopsagent/latest/APIReference/Welcome.html)
- [AWS DevOps Agent Skills — Creating and uploading skills](https://docs.aws.amazon.com/devopsagent/latest/userguide/about-aws-devops-agent-devops-agent-skills.html)

### Blog Posts and Articles

- [Extend AWS DevOps Agent with Custom Skills for Your Operational Workflows](https://builder.aws.com/content/3BDdQAFY2bSmtjecZC7vbOQGSEV/extend-aws-devops-agent-with-custom-skills-for-your-operational-workflows)
- [Building an End-to-End Agentic SRE Using AWS DevOps Agent](https://aws.amazon.com/blogs/devops/building-an-end-to-end-agentic-sre-using-aws-devops-agent/)
- [Best Practices for Deploying AWS DevOps Agent in Production](https://aws.amazon.com/blogs/devops/best-practices-for-deploying-aws-devops-agent-in-production/)
- [Leverage Agentic AI for Autonomous Incident Response with AWS DevOps Agent](https://aws.amazon.com/blogs/devops/leverage-agentic-ai-for-autonomous-incident-response-with-aws-devops-agent/)

### Specifications and Tools

- [Agent Skills specification](https://agentskills.io/home) — the open standard this project follows
- [Agent Skill Eval](https://github.com/aws-samples/sample-agent-skill-eval) — evaluation framework for testing skills

## License

This library is licensed under the MIT-0 License. See the LICENSE file.
