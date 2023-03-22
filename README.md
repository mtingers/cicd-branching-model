# cicd-branching-model

A brain dump on CI/CD and how it relates to different Git branching workflows.

## Trunk Based Branching CI/CD Model

References:
- [trunkbaseddevelopment.com](https://trunkbaseddevelopment.com)
- [How Microsoft develops with DevOps](https://learn.microsoft.com/en-us/devops/develop/how-microsoft-develops-devops)
- [Continuous deployment or continuous delivery](https://about.gitlab.com/topics/continuous-delivery/)

Overview:
- The `main` or `master` branch is considered the `staging` branch (aka dev branch).
- The `main` or `master` branch is classified as the `staging` CI/CD pipeline.
- Release branches are classified as `review` and `production` CI/CD pipelines.
- Release branches can follow a branch naming convention like `release/1.0.0`.
- Other branches are ignored from CI/CD and should be short lived (often only locally, rebased, etc).
- Commits pushed to `origin` `main`/`master` trigger the `staging` CI/CD pipeline automatically.
  Deployments are automatic to the `staging` targets (e.g. Kubernetes cluster).
- The `staging` pipeline runs a subset of `production` tests (e.g. unit tests, linting, etc).
- New release branches trigger the CI/CD pipeline automatically.
  Deployments are partially automatic to a special environment named `review`.
  Once the `review` environment is deployed, the pipeline pauses.
- Upon reviewing the `review` environment (tests, log checks, etc), the `production` pipeline can
  continue on and manually release to the `production` target cluster.
- `staging` fits more in the continuous deployment classification since it automatically deployed.
- `production` is closer to continuous delivery since it is manually deployed.

![Trunk CICD](/trunk-cicd.png)


Other notes:
- Some of these concepts are inspired by Gitlab autodevops, but modified to fit trunk
  based development branching models and include mono repos that may have multiple images built or
  multiple deployment targets.
