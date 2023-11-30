# Setup from scratch

To setup the project from scratch, there are a few steps to take and a few prerequirements. 

## Prerequirements

- A pulumi account with an organization
- Locally installed pulumi cli tool
- AWS account with configured local access for cli tools

## The steps

- Create a new pulumi access token to be used in the github actions
- In the github organization settings, create a new secret named `PULUMI_ACCESS_TOKEN` and paste the pulumi access token there
  - Settings -> Security -> Secrets and Variables -> New organization secret
- In the github organization settings, create a new secret named `AWS_REGION` and paste the intended default AWS region there
  - Settings -> Security -> Secrets and Variables -> New organization secret
- `VFD_PROJECTS_PAT`, TODO: what is this?
- In each repository (phase 1 and/or phase 2) github page, create a new deployment environment that is named to match the intented stack name (eg. dev, staging)
  - Settings -> Environments -> New environment
- In the infrastructure repository with a command line tool, create a new stack that matches the name of the deployment environment:
    - pulumi stack init <pulumi-organization>/<stack-name>, eg. `pulumi stack init virtual-finland/dev`
- In the infrastructure repository with a command line tool, manually deploy the stack:
    - `pulumi up`
- In the projects repository github actions page run the phase 1 or phase 2 deployment.