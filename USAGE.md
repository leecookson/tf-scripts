# Command Usage Reference

All commands source `tfcommon`, which handles folder/env detection, backend setup, and credential acquisition before the terraform commands run.

The first positional argument is always `<folder>` (or `<folder>/<env>` for environment-specific configs). When a slash is present it is split into `folder` and `env`, and the corresponding `envs/<env>/` files are layered on top.

---

## tfplan

Run `terraform fmt --check` and `terraform plan`, saving the plan to `plan.tfplan`.

```
tfplan <folder[/env]>
```

| Argument | Required | Description |
|---|---|---|
| `folder[/env]` | yes | Path to the Terraform config folder, optionally including an env subfolder |
| `--reconfigure` | no | Pass `-reconfigure` to `terraform init` instead of `-migrate-state` |

**Examples**

```sh
tfplan aws
tfplan azure/staging
tfplan gcp --reconfigure
```

---

## tfapply

Run `terraform plan` then immediately `terraform apply` the resulting plan.

```
tfapply <folder[/env]> [--target <resource>] ...
```

| Argument | Required | Description |
|---|---|---|
| `folder[/env]` | yes | Path to the Terraform config folder, optionally including an env subfolder |
| `--target <resource>` | no | Limit plan/apply to a specific resource or module. Repeatable. |
| `--reconfigure` | no | Pass `-reconfigure` to `terraform init` instead of `-migrate-state` |

**Examples**

```sh
tfapply aws
tfapply azure/prod --target module.networking
tfapply aws --target aws_s3_bucket.assets --target aws_iam_role.lambda
```

---

## tfdestroy

Run `terraform plan -destroy` then `terraform apply` the resulting destroy plan.

```
tfdestroy <folder[/env]>
```

| Argument | Required | Description |
|---|---|---|
| `folder[/env]` | yes | Path to the Terraform config folder, optionally including an env subfolder |
| `--reconfigure` | no | Pass `-reconfigure` to `terraform init` instead of `-migrate-state` |

**Examples**

```sh
tfdestroy aws
tfdestroy azure/staging
```

---

## tfimport

Import an existing cloud resource into Terraform state.

```
tfimport <folder[/env]> <resource_path> <resource_id>
```

| Argument | Required | Description |
|---|---|---|
| `folder[/env]` | yes | Path to the Terraform config folder, optionally including an env subfolder |
| `resource_path` | yes | Terraform resource address to import into (e.g. `aws_s3_bucket.my_bucket`) |
| `resource_id` | yes | Cloud provider resource ID to import (e.g. `my-bucket-name`) |

**Examples**

```sh
tfimport aws aws_s3_bucket.assets my-assets-bucket
tfimport azure/prod azurerm_resource_group.main /subscriptions/00000000/resourceGroups/my-rg
```

---

## tfoutput

Read all output values from the current Terraform state.

```
tfoutput <folder[/env]>
```

| Argument | Required | Description |
|---|---|---|
| `folder[/env]` | yes | Path to the Terraform config folder, optionally including an env subfolder |

**Examples**

```sh
tfoutput aws
tfoutput azure/prod
```

---

## tfstate

Inspect and manipulate Terraform state.

```
tfstate <folder[/env]> <state_cmd> [<resource_address>] [<target_address>]
```

| Argument | Required | Description |
|---|---|---|
| `folder[/env]` | yes | Path to the Terraform config folder, optionally including an env subfolder |
| `state_cmd` | yes | State sub-command: `list`, `show`, `rm`, or `mv` |
| `resource_address` | conditional | Resource address — required for `show`, `rm`, and `mv` |
| `target_address` | conditional | Destination address — required for `mv` only |

**Examples**

```sh
tfstate aws list
tfstate aws show aws_instance.web
tfstate azure/prod rm aws_instance.old
tfstate gcp mv google_storage_bucket.old google_storage_bucket.new
```
