Terraform Common Scripts
===

Create `terraform` AWS IAM deploy user
---
Create inline role using: docs/terraform_iam_role.json

Create user credential and install 1password AWS CLI plugin:

https://developer.1password.com/docs/cli/shell-plugins/aws/

For maximum security, do not use static credentials in `~/.aws/credentials`

Add terraform scripts to PATH
===
Run `./init` from the root of this repo to add the scripts to your bin folder
- `tfplan`
- `tfapply`
- `tfdestroy`
- `tfcommon`
