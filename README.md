# Terraform Helper Scripts

# tf-scripts

### Opinionated helper scripts to orchestrate terraform use

tf-scripts handles multi-folder (mono-repo) approach to deploying multiple projects to AWS, Azure and GCP, using OIDC-based authentication.

### Prerequisites

A backend based on AWS S3 and Dynamo.  See: https://registry.terraform.io/modules/abhisheksr01/s3-backend/aws/latest

### Supported commands

- tfplan
- tfapply
- tfdestroy
- tfstate
- tfimport

The core logic shared by all of these is in the tfcommon file

### job.vars options:
 - `AWS_DEPLOY_PROFILE` : profile to use to deploy from this environment
 - `TERRAFORM_AWS_BACKEND_PROFILE` : profile to use to access AWS S3 Backend
 - `TERRAFORM_BACKEND_BUCKET` : S3 bucket used to store backend state
 - `INCLUDE_ZIPS` : Copy *.zip files from project and env folders to "work" folder

To include scripts used for CF Functions, lambdas or CloudWatch canary functions, place them in a folder in the main project folder.

### Install

After cloning the repo, the ./init command will install in /usr/local/bin if writable or $HOME/bin if the

## CircleCI Orb Template

[See src/README.md](src/README.md)
