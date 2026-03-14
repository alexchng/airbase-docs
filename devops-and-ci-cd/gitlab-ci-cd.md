# GitLab CI / CD

## Introduction

This guide will help configure projects to get started with DevOps deployment on Airbase Containers:

* On the `main` branch, automatically deploy to production (default).
* Automatically deploy merge requests to preview environments
  * Integrates with GitLab’s Environments to produce a “View Deployment” button
  * When the merged request is closed/merged, the preview environment would be automaticaly destroyed

### Setup

#### Airbase Team and Project

Take note of the team and project handle used during project creation.

#### Generate Airbase Credentials for use in CI / CD

Generate Airbase credentials that would be used in the CI / CD pipeline

Credentials can be created in the Airbase Console under **Settings** -> **Credentials**

#### Configuring Gitlab CI / CD Variables

Create the following Gitlab CI / CD variables

| Type     | Name                     | Description                                     | Example Value                                                        |
| -------- | ------------------------ | ----------------------------------------------- | -------------------------------------------------------------------- |
| File     | `AIRBASE_RC`             | Airbase Credentials                             | `<br>AIRBASE_ACCESS_KEY_ID=xxx<br>AIRBASE_SECRET_ACCESS_KEY=yyy<br>` |
| Variable | `AIRBASE_PROJECT_HANDLE` | Project handle in the format `<Team>/<Project>` | `demo/demo-days`                                                     |

#### Application Environment Variables

Environment variables required for your application should be set as CI / CD variables, and not be checked in to your repository.

Create the following Gitlab CI / CD variables

| Type | Name                     | Example Value                                                                                     |
| ---- | ------------------------ | ------------------------------------------------------------------------------------------------- |
| File | `AIRBASE_ENV_LOCAL_FILE` | `<br># Env variables for your app, e.g.<br>NEXTAUTH_SECRET=mysupersecret<br>DATABASE_URL=...<br>` |

#### Configure GitLab CI pipeline

Add these lines into `.gitlab-ci.yml` to enable GitLab CI pipelines using our recommended template.

```
include:
  - project: innersource/sgts/runtime/airbase/airbase-pipeline
    ref: v1
    file: baseline.gitlab-ci.yml

review:
  extends: .airbase-deploy
  stage: test
  rules:
    - if: $CI_PIPELINE_SOURCE == "merge_request_event"
  variables:
    AIRBASE_ENVIRONMENT: $CI_COMMIT_REF_SLUG
  environment:
    name: review/$AIRBASE_ENVIRONMENT
    deployment_tier: testing
    url: $DYNAMIC_ENVIRONMENT_URL
    on_stop: stop_review
    auto_stop_in: 1 day

stop_review:
  extends: .airbase-destroy
  stage: test
  rules:
    - if: $CI_PIPELINE_SOURCE == "merge_request_event"
  needs:
    - review
  variables:
    AIRBASE_ENVIRONMENT: $CI_COMMIT_REF_SLUG
  environment:
    name: review/$AIRBASE_ENVIRONMENT
    deployment_tier: testing
    action: stop

production:
  extends: .airbase-deploy
  stage: deploy
  rules:
    - if: $CI_COMMIT_BRANCH == $CI_DEFAULT_BRANCH
  variables:
    AIRBASE_ENVIRONMENT: default
  environment:
    name: production
    deployment_tier: staging
    url: $DYNAMIC_ENVIRONMENT_URL
    on_stop: stop_production

stop_production:
  extends: .airbase-destroy
  stage: deploy
  rules:
    - if: $CI_COMMIT_BRANCH == $CI_DEFAULT_BRANCH
  needs:
    - production
  variables:
    AIRBASE_ENVIRONMENT: default
  environment:
    name: production
    deployment_tier: staging
    action: stop
```

### Conclusion

You should now be able to **View Deployment** on new Merge Requests and Branches in GitLab.

Click on **Operate > Environments** to view all the branch environments.
