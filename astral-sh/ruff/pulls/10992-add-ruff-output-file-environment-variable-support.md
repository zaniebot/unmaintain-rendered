```yaml
number: 10992
title: "Add `RUFF_OUTPUT_FILE` environment variable support"
type: pull_request
state: merged
author: sisp
labels:
  - configuration
assignees: []
merged: true
base: main
head: feat/env-ruff-output-file
created_at: 2024-04-17T10:34:24Z
updated_at: 2024-04-17T16:09:56Z
url: https://github.com/astral-sh/ruff/pull/10992
synced_at: 2026-01-10T22:37:01Z
```

# Add `RUFF_OUTPUT_FILE` environment variable support

---

_Pull request opened by @sisp on 2024-04-17 10:34_

## Summary

I've added support for configuring the `ruff check` output file via the environment variable `RUFF_OUTPUT_FILE` akin to #1731.

This is super useful when, e.g., generating a [GitLab code quality report](https://docs.gitlab.com/ee/ci/testing/code_quality.html#implement-a-custom-tool) while running Ruff as a pre-commit hook. Usually, `ruff check` should print its human-readable output to `stdout`, but when run through `pre-commit` _in a GitLab CI job_ it should write its output in `gitlab` format to a file. So, to override these two settings only during CI, environment variables come handy, and `RUFF_OUTPUT_FORMAT` already exists but `RUFF_OUTPUT_FILE` has been missing.

A (simplified) GitLab CI job config for this scenario might look like this:

```yaml
pre-commit:
  stage: test
  image: python
  variables:
    RUFF_OUTPUT_FILE: gl-code-quality-report.json
    RUFF_OUTPUT_FORMAT: gitlab
  before_script:
    - pip install pre-commit
  script:
    - pre-commit run --all-files --show-diff-on-failure
  artifacts:
    reports:
      codequality: gl-code-quality-report.json
```

## Test Plan

I tested it manually.

---

_Comment by @github-actions[bot] on 2024-04-17 10:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `configuration` added by @charliermarsh on 2024-04-17 15:42_

---

_@charliermarsh approved on 2024-04-17 15:42_

This seems reasonable to me.

---

_Merged by @charliermarsh on 2024-04-17 15:42_

---

_Closed by @charliermarsh on 2024-04-17 15:42_

---

_Branch deleted on 2024-04-17 16:09_

---
