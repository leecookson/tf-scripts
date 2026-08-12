## tf-scripts

Projects that use tf-scripts (`tfplan`, `tfapply`, `tfdestroy`, `tfimport`, `tfstate`, `tfoutput`) should be aware of the following.

### Command syntax

All commands take `<folder>` or `<folder/env>` as the first argument. Run `<command> --help` for full usage.

```
tfplan   <folder[/env]> [--target <resource>] ... [--reconfigure]
tfapply  <folder[/env]> [--target <resource>] ... [--reconfigure]
tfdestroy <folder[/env]> [--reconfigure]
tfimport <folder[/env]> <resource_path> <resource_id>
tfstate  <folder[/env]> list|show|rm|mv [<resource_address>] [<target_address>]
tfoutput <folder[/env]>
```

### plan.txt and apply.txt

After running `tfplan` or `tfapply`, ANSI-stripped human-readable output is saved to the work folder:

- `work/plan.txt` — output of the terraform plan step, written by both `tfplan` and `tfapply`
- `work/apply.txt` — output of the terraform apply step, written by `tfapply` only

Both files end with a summary line in the form:
```
PLAN RESULT: <folder/env> SUCCESS at <timestamp>
APPLY RESULT: <folder/env> FAILED (exit code 1) at <timestamp>
```

When asked to review, explain, or act on a plan or apply, read these files rather than re-running terraform.

### Work folder caching

`setupworkfolder_cached` preserves the `.terraform/` provider cache between runs to avoid re-downloading providers. Source `.tf` files are always refreshed. `terraform init` is re-run automatically only when `.terraform.lock.hcl` changes.
