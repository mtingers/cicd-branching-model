# cicd-branching-model

A brain dump on CI/CD and how it relates to different Git branching workflows.

## Trunk Based Branching CI/CD Model

References:
- [trunkbaseddevelopment.com](https://trunkbaseddevelopment.com)
- [How Microsoft develops with DevOps](https://learn.microsoft.com/en-us/devops/develop/how-microsoft-develops-devops)
- [Continuous deployment or continuous delivery](https://about.gitlab.com/topics/continuous-delivery/)


Overview:
- The `main` or `master` branch is considered the `staging` branch (aka dev branch).
- The `main` or `master` is assigned to the `staging` CI/CD pipeline.
- Release branches are assigned to `review` and `production` CI/CD pipelines.
- Release branches can follow a branch naming convention similar to `release/1.0.0`.
- Other branches are not assigned to a CI/CD pipeline and should be short lived (often only locally,
  rebased, etc).
- Commits pushed to `origin` `main`/`master` trigger the `staging` CI/CD pipeline automatically.
  Deployments are automatic to the `staging` targets (e.g. Kubernetes cluster).
- The `staging` pipeline runs a subset of `production` tests (unit tests, linting, etc).
- New release branches trigger the CI/CD pipeline automatically.
  Deployments are partially automatic to a special environment named `review`.
  Once the `review` environment is deployed, the pipeline pauses.
- Upon reviewing the `review` environment (tests, log checks, etc), the `production` pipeline can
  continue on and manually release to the `production` target cluster.
- `staging` fits more in the continuous deployment classification since it automatically deploys.
- `production` is closer to continuous delivery since it is manually deployed.


![Trunk CICD](/trunk-cicd.png)


Other notes:
- Some of these concepts are inspired by Gitlab autodevops, but modified to fit trunk
  based development branching models and include mono repos that may have multiple images built or
  multiple deployment targets.
- There are a ton of other options and refinements that would be done in real life CI/CD, tuned to
  specific needs of the project (especially for multi-app mono-repos). But this gives a generalized
  starting point that can be extended as needed.
- Not all repos/projects need a CD part, like libraries or images referenced from other projects via
  package/container registries or submodules.
