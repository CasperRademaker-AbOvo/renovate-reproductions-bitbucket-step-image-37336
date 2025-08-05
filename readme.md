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

## How are you running Renovate?

A Mend.io-hosted app 

## Snippet from logs

```
DEBUG: packageFiles with updates
{
  "baseBranch": "main"
  "config": {
    "bitbucket-pipelines": [
      {
        "deps": [
          {
            "autoReplaceStringTemplate": "{{depName}}{{#if newValue}}:{{newValue}}{{/if}}{{#if newDigest}}@{{newDigest}}{{/if}}",
            "currentValue": "24.4.1",
            "currentVersion": "24.4.1",
            "currentVersionAgeInDays": 12,
            "currentVersionTimestamp": "2025-07-24T06:39:52.325Z",
            "datasource": "docker",
            "depName": "node",
            "depType": "docker",
            "fixedVersion": "24.4.1",
            "isSingleVersion": true,
            "lookupName": "library/node",
            "packageName": "node",
            "registryUrl": "https://index.docker.io",
            "replaceString": "node:24.4.1",
            "sourceUrl": "https://github.com/nodejs/node",
            "versioning": "docker",
            "warnings": [],
            "updates": [
              {
                "bucket": "non-major",
                "newVersion": "24.5.0",
                "newValue": "24.5.0",
                "releaseTimestamp": "2025-08-04T12:40:19.660Z",
                "newVersionAgeInDays": 0,
                "newMajor": 24,
                "newMinor": 5,
                "newPatch": 0,
                "updateType": "minor",
                "isBreaking": false,
                "libYears": 0.030822784595383054,
                "branchName": "renovate/node-24.x"
              }
            ]
          }
        ],
        "packageFile": "bitbucket-pipelines.yml"
      }
    ]
  }
}
```