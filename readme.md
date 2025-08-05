# 12260

Reproduction for [Renovate issue 12260](https://github.com/renovatebot/renovate/issues/12260).

## Current behavior

Docker images specified in Bitbucket pipeline (`bitbucket-pipelines.yml`) [step options](https://support.atlassian.com/bitbucket-cloud/docs/step-options/#Docker-images) are not recognized by Renovate.

Created PRs by Renovate only update global image versions and do not update step image versions. 

```yaml
image: node:24.4.1 # <- recognized correctly by Renovate

definitions:
  steps:
    - step: &check-license-attribution
        image: node:24.4.1 # <- NOT recognized by Renovate
        name: Check if license attribution is up to date
        script:
          - npx generate-attribution
          - git diff HEAD --exit-code ./oss-attribution 

pipelines:
  branches:
    '{main}':
      - step: *check-license-attribution

```

## Expected behavior

Version of image defined in step options is also updated. 
