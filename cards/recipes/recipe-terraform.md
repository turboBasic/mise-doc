---
id: recipe-terraform
title: "Recipe: Terraform with mise"
category: recipes
tags: [recipe, terraform, infrastructure]
source: https://github.com/jdx/mise/blob/main/docs/mise-cookbook/terraform.md
related: [backend-aqua, env-variables, task-toml-tasks]
---

## Summary

Managing Terraform versions per-project with mise, including environment-specific configs and task automation.

## Examples

### Basic Terraform project

```toml
# mise.toml
[tools]
terraform = "1.7"
# or from aqua (preferred):
"aqua:hashicorp/terraform" = "1.7"

[env]
TF_PLUGIN_CACHE_DIR = "{{env.HOME}}/.terraform.d/plugin-cache"

[tasks.init]
run = "terraform init"

[tasks.plan]
run = "terraform plan -out=tfplan"
depends = ["init"]

[tasks.apply]
run = "terraform apply tfplan"
depends = ["plan"]

[tasks.fmt]
run = "terraform fmt -recursive"

[tasks.validate]
run = "terraform validate"
depends = ["init"]
```

### Multi-environment Terraform

```toml
# mise.toml
[tools]
"aqua:hashicorp/terraform" = "1.7"
"aqua:gruntwork-io/terragrunt" = "0.55"

[tasks.plan]
run = "terraform plan -var-file=envs/{{env.MISE_ENV}}.tfvars"

[tasks.apply]
run = "terraform apply -var-file=envs/{{env.MISE_ENV}}.tfvars"
```

```sh
# Plan for staging
MISE_ENV=staging mise run plan

# Apply to production
MISE_ENV=production mise run apply
```

## Details

Benefits of managing Terraform with mise:
- Pin Terraform version per-project (avoids state file version conflicts)
- Different Terraform versions across projects without manual switching
- Task automation for common terraform workflows
- Environment-specific variable files via `MISE_ENV`

## See Also

- backend-aqua
- env-variables
- config-environments
- task-toml-tasks
