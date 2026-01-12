```yaml
number: 14844
title: Run zizmor in CI, and fix most warnings
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
  - security
assignees: []
merged: true
base: main
head: alex/zizmor
created_at: 2024-12-08T23:50:25Z
updated_at: 2024-12-10T08:40:12Z
url: https://github.com/astral-sh/ruff/pull/14844
synced_at: 2026-01-12T15:55:49Z
```

# Run zizmor in CI, and fix most warnings

---

_@AlexWaygood_

## Summary

A [recent exploit](https://github.com/advisories/GHSA-7x29-qqmq-v6qc) brought attention to how easy it can be for attackers to use template expansion in GitHub Actions workflows to inject arbitrary code into a repository. That vulnerability [would have been caught by the zizmor linter](https://blog.yossarian.net/2024/12/06/zizmor-ultralytics-injection), which looks for potential security vulnerabilities in GitHub Actions workflows. This PR adds [zizmor](https://github.com/woodruffw/zizmor) as a pre-commit hook and fixes the high- and medium-severity warnings flagged by the tool.

All the warnings fixed in this PR are related to this zizmor check: https://woodruffw.github.io/zizmor/audits/#artipacked. The summary of the check is that `actions/checkout` will by default persist git configuration for the duration of the workflow, which can be insecure. It's unnecessary unless you actually need to do things with `git` later on in the workflow. None of our workflows do except for `publish-docs.yml` and `sync-typeshed.yml`, so I set `persist-credentials: true` for those two but `persist-credentials: false` for all other uses of `actions/checkout`.

Unfortunately there are several warnings in `release.yml`, including four high-severity warnings. However, this is a generated workflow file, so I have deliberately excluded this file from the check. These are the findings in `release.yml`:

<details>
<summary>release.yml findings</summary>

```
warning[artipacked]: credential persistence through GitHub Actions artifacts
  --> /Users/alexw/dev/ruff/.github/workflows/release.yml:62:9
   |
62 |         - uses: actions/checkout@v4
   |  _________-
63 | |         with:
64 | |           submodules: recursive
   | |_______________________________- does not set persist-credentials: false
   |
   = note: audit confidence → Low

warning[artipacked]: credential persistence through GitHub Actions artifacts
   --> /Users/alexw/dev/ruff/.github/workflows/release.yml:124:9
    |
124 |         - uses: actions/checkout@v4
    |  _________-
125 | |         with:
126 | |           submodules: recursive
    | |_______________________________- does not set persist-credentials: false
    |
    = note: audit confidence → Low

warning[artipacked]: credential persistence through GitHub Actions artifacts
   --> /Users/alexw/dev/ruff/.github/workflows/release.yml:174:9
    |
174 |         - uses: actions/checkout@v4
    |  _________-
175 | |         with:
176 | |           submodules: recursive
    | |_______________________________- does not set persist-credentials: false
    |
    = note: audit confidence → Low

warning[artipacked]: credential persistence through GitHub Actions artifacts
   --> /Users/alexw/dev/ruff/.github/workflows/release.yml:249:9
    |
249 |         - uses: actions/checkout@v4
    |  _________-
250 | |         with:
251 | |           submodules: recursive
252 | |       # Create a GitHub Release while uploading all files to it
    | |_______________________________________________________________- does not set persist-credentials: false
    |
    = note: audit confidence → Low

error[excessive-permissions]: overly broad workflow or job-level permissions
  --> /Users/alexw/dev/ruff/.github/workflows/release.yml:17:1
   |
17 | / permissions:
18 | |   "contents": "write"
...  |
39 | | # If there's a prerelease-style suffix to the version, then the release(s)
40 | | # will be marked as a prerelease.
   | |_________________________________^ contents: write is overly broad at the workflow level
   |
   = note: audit confidence → High

error[template-injection]: code injection via template expansion
  --> /Users/alexw/dev/ruff/.github/workflows/release.yml:80:9
   |
80 |          - id: plan
   |   _________^
81 |  |         run: |
   |  |_________^
82 | ||           dist ${{ (inputs.tag && inputs.tag != 'dry-run' && format('host --steps=create --tag={0}', inputs.tag)) || 'plan' }} --out...
83 | ||           echo "dist ran successfully"
84 | ||           cat plan-dist-manifest.json
85 | ||           echo "manifest=$(jq -c "." plan-dist-manifest.json)" >> "$GITHUB_OUTPUT"
   | ||__________________________________________________________________________________^ this step
   | ||__________________________________________________________________________________^ inputs.tag may expand into attacker-controllable code
   |
   = note: audit confidence → Low

error[template-injection]: code injection via template expansion
  --> /Users/alexw/dev/ruff/.github/workflows/release.yml:80:9
   |
80 |          - id: plan
   |   _________^
81 |  |         run: |
   |  |_________^
82 | ||           dist ${{ (inputs.tag && inputs.tag != 'dry-run' && format('host --steps=create --tag={0}', inputs.tag)) || 'plan' }} --out...
83 | ||           echo "dist ran successfully"
84 | ||           cat plan-dist-manifest.json
85 | ||           echo "manifest=$(jq -c "." plan-dist-manifest.json)" >> "$GITHUB_OUTPUT"
   | ||__________________________________________________________________________________^ this step
   | ||__________________________________________________________________________________^ inputs.tag may expand into attacker-controllable code
   |
   = note: audit confidence → Low

error[template-injection]: code injection via template expansion
  --> /Users/alexw/dev/ruff/.github/workflows/release.yml:80:9
   |
80 |          - id: plan
   |   _________^
81 |  |         run: |
   |  |_________^
82 | ||           dist ${{ (inputs.tag && inputs.tag != 'dry-run' && format('host --steps=create --tag={0}', inputs.tag)) || 'plan' }} --out...
83 | ||           echo "dist ran successfully"
84 | ||           cat plan-dist-manifest.json
85 | ||           echo "manifest=$(jq -c "." plan-dist-manifest.json)" >> "$GITHUB_OUTPUT"
   | ||__________________________________________________________________________________^ this step
   | ||__________________________________________________________________________________^ inputs.tag may expand into attacker-controllable code
   |
   = note: audit confidence → Low
```

</details>

## Test Plan

`uvx pre-commit run -a`


---

_Label `ci` added by @AlexWaygood on 2024-12-08 23:50_

---

_Label `security` added by @AlexWaygood on 2024-12-08 23:50_

---

_Review comment by @AlexWaygood on `.pre-commit-config.yaml`:99 on 2024-12-08 23:53_

I know we prefer to keep configuration in separate files rather than putting it in pre-commit, but it doesn't seem possible to specify these in zizmor's configuration file right now (https://woodruffw.github.io/zizmor/configuration/#settings)

---

_Review comment by @AlexWaygood on `.pre-commit-config.yaml`:104 on 2024-12-08 23:54_

This is just some more extra linting for our workflows. It's unrelated to the other changes in the PR. It doesn't have any quarrels with any of our workflows currently, but I figured it might be a good idea to add it as well.

---

_@AlexWaygood reviewed on 2024-12-08 23:54_

---

_Comment by @github-actions[bot] on 2024-12-08 23:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@charliermarsh approved on 2024-12-09 00:13_

---

_Merged by @AlexWaygood on 2024-12-09 00:42_

---

_Closed by @AlexWaygood on 2024-12-09 00:42_

---

_Branch deleted on 2024-12-09 00:42_

---

_@hugovk reviewed on 2024-12-10 08:40_

---

_Review comment by @hugovk on `.pre-commit-config.yaml`:99 on 2024-12-10 08:40_

I noticed this too. Tip: open an issue for zizmor and I bet William will implement it :)

---
