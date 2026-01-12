```yaml
number: 11532
title: Use project-relative path when calculating gitlab message fingerprint
type: pull_request
state: merged
author: furgoose
labels:
  - bug
  - breaking
assignees: []
merged: true
base: main
head: gitlab-relative-project-fingerprint
created_at: 2024-05-24T13:12:59Z
updated_at: 2024-05-27T10:05:52Z
url: https://github.com/astral-sh/ruff/pull/11532
synced_at: 2026-01-12T15:55:38Z
```

# Use project-relative path when calculating gitlab message fingerprint

---

_@furgoose_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Concurrent GitLab runners clone projects into separate directories, e.g. `{builds_dir}/$RUNNER_TOKEN_KEY/$CONCURRENT_ID/$NAMESPACE/$PROJECT_NAME`. Since the fingerprint uses the full path to the file, the fingerprints calculated by Ruff are different depending on which concurrent runner it executes on, so often an MR will appear to remove all existing issues and add them with new fingerprints.

I've adjusted the fingerprint function to use the project relative path, which fixes this. Unfortunately this will have a breaking change for any current users of this output - the fingerprints will change and appear in GitLab as all linting messages having been fixed and then created.

## Test Plan

<!-- How was it tested? -->
`cargo nextest run`

Running `ruff check --output-format gitlab` in a git repo, moving the repo and running again, verifying no diffs between the outputs


---

_Comment by @github-actions[bot] on 2024-05-24 13:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-05-26 18:09_

This seems correct to me.

---

_Merged by @charliermarsh on 2024-05-26 18:10_

---

_Closed by @charliermarsh on 2024-05-26 18:10_

---

_Label `bug` added by @charliermarsh on 2024-05-26 18:10_

---

_Comment by @dhruvmanila on 2024-05-27 04:27_

> I've adjusted the fingerprint function to use the project relative path, which fixes this. Unfortunately this will have a breaking change for any current users of this output - the fingerprints will change and appear in GitLab as all linting messages having been fixed and then created.

Then, we need to make sure to add this to `BREAKING_CHANGES.md` in the next release similar to https://github.com/astral-sh/ruff/blob/main/BREAKING_CHANGES.md#improved-gitlab-fingerprints-7203

---

_Label `breaking` added by @dhruvmanila on 2024-05-27 04:27_

---

_Comment by @charliermarsh on 2024-05-27 04:36_

Does that really qualify as a breaking change? I think it’s debatable. 

---

_Comment by @dhruvmanila on 2024-05-27 04:53_

I might have jumped the gun but reading through it carefully it does seem like it should be fine (?) although I'm not exactly sure. Regardless, we should highlight this in the changelog.

---

_Branch deleted on 2024-05-27 10:05_

---
