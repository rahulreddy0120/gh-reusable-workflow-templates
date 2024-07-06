# GitHub Reusable Workflows

Production-ready reusable GitHub Actions workflows for DevSecOps CI/CD pipelines. Uses **OIDC federation** for AWS authentication — no static access keys.

## Workflows

| Workflow | Description |
|----------|-------------|
| **docker-build-scan-push** | Build, Trivy scan, push to ECR. OIDC auth. |
| **terraform-plan-apply** | Checkov scan, plan, approval gate, apply. OIDC auth. |
| **sast-dast-scan** | Semgrep SAST + ZAP DAST + Gitleaks + Trivy deps |
| **aws-tag-compliance** | Audit EC2/RDS/S3/EKS/Lambda for required tags. OIDC auth. |

## Quick Start

```yaml
permissions:
  id-token: write
  contents: read

jobs:
  docker:
    uses: rahulreddy0120/gh-reusable-workflow-templates/.github/workflows/docker-build-scan-push.yml@main
    with:
      image_name: my-app
      registry: 123456789012.dkr.ecr.us-gov-west-1.amazonaws.com
      role_arn: arn:aws-us-gov:iam::123456789012:role/github-ecr-role

  security:
    uses: rahulreddy0120/gh-reusable-workflow-templates/.github/workflows/sast-dast-scan.yml@main
    with:
      language: python

  infra:
    needs: [docker, security]
    uses: rahulreddy0120/gh-reusable-workflow-templates/.github/workflows/terraform-plan-apply.yml@main
    with:
      environment: prod
      role_arn: arn:aws-us-gov:iam::123456789012:role/github-terraform-role
```

## Authentication

All AWS workflows use GitHub OIDC federation. Create an IAM role with trust policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Principal": {
      "Federated": "arn:aws-us-gov:iam::ACCOUNT:oidc-provider/token.actions.githubusercontent.com"
    },
    "Action": "sts:AssumeRoleWithWebIdentity",
    "Condition": {
      "StringLike": {
        "token.actions.githubusercontent.com:sub": "repo:your-org/your-repo:*"
      }
    }
  }]
}
```

## Security

- No static credentials — OIDC federation only
- SARIF reports uploaded to GitHub Security tab
- Scan gates block pipelines on critical/high findings
- Manual approval gates for Terraform apply

## License

MIT

<!-- updated: 2024-10-18 -->

<!-- updated: 2024-12-05 -->

<!-- updated: 2025-01-22 -->

<!-- updated: 2025-03-10 -->

<!-- updated: 2025-05-28 -->

<!-- updated: 2025-07-15 -->

<!-- updated: 2025-09-02 -->

<!-- updated: 2025-11-18 -->

<!-- 2024-02-14T14:25:00 -->

<!-- 2024-03-18T10:40:00 -->

<!-- 2024-05-06T15:55:00 -->

<!-- 2024-06-24T11:10:00 -->

<!-- 2024-08-12T09:25:00 -->

<!-- 2024-10-28T14:40:00 -->

<!-- 2024-12-09T10:55:00 -->

<!-- 2025-02-17T16:10:00 -->

<!-- 2025-04-07T11:25:00 -->

<!-- 2025-06-23T09:40:00 -->

<!-- 2025-09-08T14:55:00 -->

<!-- 2025-11-24T10:10:00 -->

<!-- 2024-02-14T14:25:00 -->

<!-- 2024-03-18T10:40:00 -->

<!-- 2024-05-06T15:55:00 -->

<!-- 2024-06-24T11:10:00 -->

<!-- 2024-08-12T09:25:00 -->

<!-- 2024-10-28T14:40:00 -->

<!-- 2024-12-09T10:55:00 -->

<!-- 2025-02-17T16:10:00 -->

<!-- 2025-04-07T11:25:00 -->

<!-- 2025-06-23T09:40:00 -->

<!-- 2025-09-08T14:55:00 -->

<!-- 2025-11-24T10:10:00 -->

<!-- 2024-01-23T14:25:00 -->

<!-- 2024-02-14T10:40:00 -->

<!-- 2024-03-18T15:55:00 -->

<!-- 2024-06-24T11:10:00 -->

<!-- 2024-06-25T09:25:00 -->

<!-- 2024-10-28T14:40:00 -->

<!-- 2024-12-09T10:55:00 -->

<!-- 2025-02-17T16:10:00 -->

<!-- 2025-07-23T11:25:00 -->

<!-- 2025-07-24T09:40:00 -->

<!-- 2025-11-24T14:55:00 -->

<!-- 2024-01-31T14:25:00 -->

<!-- 2024-02-20T10:40:00 -->

<!-- 2024-04-09T15:55:00 -->

<!-- 2024-06-18T11:10:00 -->

<!-- 2024-06-19T09:25:00 -->

<!-- 2024-10-15T14:40:00 -->

<!-- 2024-12-17T10:55:00 -->

<!-- 2025-02-04T16:10:00 -->

<!-- 2025-07-15T11:25:00 -->

<!-- 2025-07-16T09:40:00 -->

<!-- 2025-12-23T14:55:00 -->

<!-- 2024-03-22T10:04:00 -->

<!-- 2024-06-26T16:58:00 -->

<!-- 2024-07-30T13:05:00 -->

<!-- 2024-09-03T13:58:00 -->

<!-- 2024-11-16T11:37:00 -->

<!-- 2025-04-19T08:37:00 -->

<!-- 2025-04-23T13:42:00 -->

<!-- 2025-05-08T11:09:00 -->

<!-- 2025-06-12T08:38:00 -->

<!-- 2025-08-23T09:22:00 -->

<!-- 2024-01-13T12:01:00 -->

<!-- 2024-01-24T12:53:00 -->

<!-- 2024-02-05T12:02:00 -->

<!-- 2024-04-23T16:08:00 -->

<!-- 2024-06-19T15:31:00 -->

<!-- 2024-07-06T15:18:00 -->

<!-- 2024-08-06T11:42:00 -->

<!-- 2024-11-06T09:50:00 -->

<!-- 2024-11-16T13:21:00 -->

<!-- 2025-02-11T15:39:00 -->

<!-- 2025-03-05T08:07:00 -->

<!-- 2025-07-25T09:05:00 -->

<!-- 2025-08-12T12:51:00 -->

<!-- 2026-04-08T08:43:00 -->

<!-- 2024-01-13T12:01:00 -->

<!-- 2024-01-24T12:53:00 -->

<!-- 2024-02-05T12:02:00 -->

<!-- 2024-04-23T16:08:00 -->

<!-- 2024-06-19T15:31:00 -->

<!-- 2024-07-06T15:18:00 -->

<!-- 2024-08-06T11:42:00 -->

<!-- 2024-11-06T09:50:00 -->

<!-- 2024-11-16T13:21:00 -->

<!-- 2025-02-11T15:39:00 -->

<!-- 2025-03-05T08:07:00 -->

<!-- 2025-07-25T09:05:00 -->

<!-- 2025-08-12T12:51:00 -->

<!-- 2026-04-08T08:43:00 -->

<!-- 2024-01-13T12:01:00 -->

<!-- 2024-01-24T12:53:00 -->

<!-- 2024-02-05T12:02:00 -->

<!-- 2024-04-23T16:08:00 -->

<!-- 2024-06-19T15:31:00 -->

<!-- 2024-07-06T15:18:00 -->
