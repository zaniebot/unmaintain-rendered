```yaml
number: 8055
title: "chore(renovate): move out of deprecated options"
type: pull_request
state: merged
author: mkniewallner
labels:
  - internal
assignees: []
merged: true
base: main
head: chore/move-out-of-deprecated-renovate-options
created_at: 2024-10-09T19:38:45Z
updated_at: 2024-10-09T19:48:52Z
url: https://github.com/astral-sh/uv/pull/8055
synced_at: 2026-01-10T12:54:02Z
```

# chore(renovate): move out of deprecated options

---

_Pull request opened by @mkniewallner on 2024-10-09 19:38_

## Summary

Found out with `npx -p renovate renovate-config-validator`.

<details>
  <summary>Output</summary>

  ```console
   INFO: Validating .github/renovate.json5
   WARN: Config migration necessary
         "oldConfig": {
           "$schema": "https://docs.renovatebot.com/renovate-schema.json",
           "dependencyDashboard": true,
           "suppressNotifications": ["prEditedNotification"],
           "extends": ["config:recommended", "customManagers:githubActionsVersions"],
           "labels": ["internal"],
           "schedule": ["before 4am on Monday"],
           "semanticCommits": "disabled",
           "separateMajorMinor": false,
           "prHourlyLimit": 10,
           "enabledManagers": ["github-actions", "pre-commit", "cargo", "regex"],
           "cargo": {
             "rangeStrategy": "update-lockfile",
             "fileMatch": ["^crates/.*Cargo\\.toml$"]
           },
           "pre-commit": {"enabled": true},
           "packageRules": [
             {
               "matchPackagePatterns": ["zip"],
               "matchManagers": ["cargo"],
               "enabled": false
             },
             {
               "matchPaths": ["docs/**/*.md"],
               "commitMessageTopic": "documentation references to {{{depName}}}",
               "semanticCommitType": "docs",
               "semanticCommitScope": null,
               "additionalBranchPrefix": "docs-"
             },
             {
               "groupName": "Artifact GitHub Actions dependencies",
               "matchManagers": ["github-actions"],
               "matchDatasources": ["gitea-tags", "github-tags"],
               "matchPackagePatterns": ["actions/.*-artifact"],
               "description": "Weekly update of artifact-related GitHub Actions dependencies"
             },
             {
               "groupName": "GitHub runners",
               "matchManagers": ["github-actions"],
               "matchDatasources": ["github-runners"],
               "description": "Disable PRs updating GitHub runners (e.g. 'runs-on: macos-14')",
               "enabled": false
             },
             {
               "groupName": "pre-commit dependencies",
               "matchManagers": ["pre-commit"],
               "description": "Weekly update of pre-commit dependencies"
             },
             {
               "groupName": "Rust dev-dependencies",
               "matchManagers": ["cargo"],
               "matchDepTypes": ["devDependencies"],
               "description": "Weekly update of Rust development dependencies"
             },
             {
               "groupName": "pyo3",
               "matchManagers": ["cargo"],
               "matchPackagePatterns": ["pyo3"],
               "description": "Weekly update of pyo3 dependencies",
               "enabled": false
             }
           ],
           "customManagers": [
             {
               "customType": "regex",
               "fileMatch": ["^docs/.*\\.md$"],
               "matchStrings": [
                 "\\suses: (?<depName>[\\w-]+/[\\w-]+)(?<path>/.*)?@(?<currentValue>.+?)\\s"
               ],
               "datasourceTemplate": "github-tags",
               "versioningTemplate": "regex:^v(?<major>\\d+)$"
             }
           ],
           "vulnerabilityAlerts": {
             "commitMessageSuffix": "",
             "labels": ["internal", "security"]
           }
         },
         "newConfig": {
           "$schema": "https://docs.renovatebot.com/renovate-schema.json",
           "dependencyDashboard": true,
           "suppressNotifications": ["prEditedNotification"],
           "extends": ["config:recommended", "customManagers:githubActionsVersions"],
           "labels": ["internal"],
           "schedule": ["before 4am on Monday"],
           "semanticCommits": "disabled",
           "separateMajorMinor": false,
           "prHourlyLimit": 10,
           "enabledManagers": ["github-actions", "pre-commit", "cargo", "custom.regex"],
           "cargo": {
             "rangeStrategy": "update-lockfile",
             "fileMatch": ["^crates/.*Cargo\\.toml$"]
           },
           "pre-commit": {"enabled": true},
           "packageRules": [
             {
               "matchManagers": ["cargo"],
               "enabled": false,
               "matchPackageNames": ["/zip/"]
             },
             {
               "matchFileNames": ["docs/**/*.md"],
               "commitMessageTopic": "documentation references to {{{depName}}}",
               "semanticCommitType": "docs",
               "semanticCommitScope": null,
               "additionalBranchPrefix": "docs-"
             },
             {
               "groupName": "Artifact GitHub Actions dependencies",
               "matchManagers": ["github-actions"],
               "matchDatasources": ["gitea-tags", "github-tags"],
               "description": "Weekly update of artifact-related GitHub Actions dependencies",
               "matchPackageNames": ["/actions/.*-artifact/"]
             },
             {
               "groupName": "GitHub runners",
               "matchManagers": ["github-actions"],
               "matchDatasources": ["github-runners"],
               "description": "Disable PRs updating GitHub runners (e.g. 'runs-on: macos-14')",
               "enabled": false
             },
             {
               "groupName": "pre-commit dependencies",
               "matchManagers": ["pre-commit"],
               "description": "Weekly update of pre-commit dependencies"
             },
             {
               "groupName": "Rust dev-dependencies",
               "matchManagers": ["cargo"],
               "matchDepTypes": ["devDependencies"],
               "description": "Weekly update of Rust development dependencies"
             },
             {
               "groupName": "pyo3",
               "matchManagers": ["cargo"],
               "description": "Weekly update of pyo3 dependencies",
               "enabled": false,
               "matchPackageNames": ["/pyo3/"]
             }
           ],
           "customManagers": [
             {
               "customType": "regex",
               "fileMatch": ["^docs/.*\\.md$"],
               "matchStrings": [
                 "\\suses: (?<depName>[\\w-]+/[\\w-]+)(?<path>/.*)?@(?<currentValue>.+?)\\s"
               ],
               "datasourceTemplate": "github-tags",
               "versioningTemplate": "regex:^v(?<major>\\d+)$"
             }
           ],
           "vulnerabilityAlerts": {
             "commitMessageSuffix": "",
             "labels": ["internal", "security"]
           }
         }
  INFO: Config validated successfully
  ```
</details>

Relevant PRs:
- https://github.com/renovatebot/renovate/pull/22406
- https://github.com/renovatebot/renovate/pull/24079
- https://github.com/renovatebot/renovate/pull/30382

---

_Marked ready for review by @mkniewallner on 2024-10-09 19:42_

---

_@zanieb approved on 2024-10-09 19:48_

Thanks!

cc @AlexWaygood 

---

_Label `internal` added by @zanieb on 2024-10-09 19:48_

---

_Merged by @zanieb on 2024-10-09 19:48_

---

_Closed by @zanieb on 2024-10-09 19:48_

---

_Branch deleted on 2024-10-09 19:48_

---
