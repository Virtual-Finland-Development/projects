# Setup from scratch

To setup the project from scratch, there are a few steps to take and a few pre-requirements. 

## Pre-requirements

- A pulumi account with an organization
- Locally installed pulumi cli tool
- AWS account with configured local access for cli tools

## The steps

### CI/CD Pipeline

- Create a new pulumi access token to be used in the github actions
- In the github organization settings, create a new secret named `PULUMI_ACCESS_TOKEN` and paste the pulumi access token there
  - Settings -> Security -> Secrets and Variables -> New organization secret
- In the github organization settings, create a new secret named `AWS_REGION` and paste the intended default AWS region there
  - Settings -> Security -> Secrets and Variables -> New organization secret
- Create a new github personal access token (PAT) to be used in the github actions
  - Settings -> Developer settings -> Personal access tokens -> Generate new token
  - Give the token a name and select the `repo` scope
  - The intent of this token is to grant access to the projects repository composite actions to use github cli tool to initiate deployments on other organization repositories
- In the [projects](https://github.com/Virtual-Finland-Development/projects) repository settings page, create a repository secret named `VFD_PROJECTS_PAT` and paste the github personal access token (PAT) there
- In each repository ([Phase 1](./Virtual-Finland-MVP-phase-1.md) and/or [Phase 2](./Virtual-Finland-MVP-phase-2.md)) github page, create a new deployment environment that is named to match the intented stack name (eg. dev, staging)
  - Settings -> Environments -> New environment

### Infrastructure

- In the [infrastructure](https://github.com/Virtual-Finland-Development/infrastructure) repository with a command line tool, create a new stack that matches the name of the deployment environment:
    - pulumi stack select <pulumi-organization>/<stack-name> --create
      - eg. `pulumi stack select virtual-finland/dev --create`
- In the [infrastructure](https://github.com/Virtual-Finland-Development/infrastructure) repository with a command line tool, manually deploy the stack:
    - `pulumi up`
    - this will create the AWS resources needed for the CI/CD pipeline to work for the rest of the repositories

### Projects

- In the [projects](https://github.com/Virtual-Finland-Development/projects) repository github actions page run the [Phase 1](./Virtual-Finland-MVP-phase-1.md) or [Phase 2](./Virtual-Finland-MVP-phase-2.md) deployment.
- The `Phase 2` includes the `Phase 1`, so there is no need to run the `Phase 1` deployment if the `Phase 2` is run.

#### Phase 2 specifics

  - If there is a custom domain name configured with the [Access Finland MVP](https://github.com/Virtual-Finland-Development/access-finland) deployments live environment, the initial deployment flow of the Access Finland MVP app will fail at the step `Initial deployment domain check` as the created SSL-certificates need verification that takes some time to actualize. 
  - To continue the `Phase 2` github actions-flow, re-run the flow (for speed, choose the "failed jobs only" option) only after the SSL-certificates have been verified by AWS. Read more at [af-mvp app deployment instructions](https://github.com/Virtual-Finland-Development/access-finland/blob/main/docs/README.af-mvp.deployment.md).
  - After that, deploy the [infrastructure](https://github.com/Virtual-Finland-Development/infrastructure)-project once more to finish the email service domain name related configurations. Read more at [infrastructure email instructions](https://github.com/Virtual-Finland-Development/infrastructure/blob/main/Docs/README.email-setup.md).

### Post-deployment

- In the [monitoring](https://github.com/Virtual-Finland-Development/monitoring) repository reconfigure alerts etc. and redeploy as need be.
- In AWS Console, configure the access credentials to the AWS Cognito user and indentity pool created by the [Access Finland MVP](https://github.com/Virtual-Finland-Development/access-finland) app. Read more at mvp-app [deployment instructions](https://github.com/Virtual-Finland-Development/access-finland/blob/main/docs/README.af-mvp.deployment.md).