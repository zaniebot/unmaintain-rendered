```yaml
number: 8506
title: "fix: usage of `a deprecated Node.js version` in CI"
type: pull_request
state: merged
author: hamirmahal
labels:
  - testing
assignees: []
merged: true
base: main
head: fix/usage-of-a-deprecated-nodejs-version-in-ci
created_at: 2024-10-23T17:49:41Z
updated_at: 2024-10-24T02:42:25Z
url: https://github.com/astral-sh/uv/pull/8506
synced_at: 2026-01-10T12:54:11Z
```

# fix: usage of `a deprecated Node.js version` in CI

---

_Pull request opened by @hamirmahal on 2024-10-23 17:49_

fixes #8505.

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

[`ci.yml`](https://github.com/astral-sh/uv/blob/main/.github/workflows/ci.yml) uses [`gabrielfalcao/pyenv-action@v18`](https://github.com/gabrielfalcao/pyenv-action/), which [uses `a deprecated Node.js version`](https://github.com/astral-sh/uv/actions/runs/11483963555).

This pull request aims to remove any usage of `a deprecated Node.js version` from [`ci.yml`](https://github.com/astral-sh/uv/blob/main/.github/workflows/ci.yml).

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

I attempted to test this but [canceled the run on my fork](https://github.com/hamirmahal/uv/actions/runs/11484989508/job/31964058415) after it waited for a runner for over 10 minutes with no result.

<!-- How was it tested? -->

---

_@hamirmahal reviewed on 2024-10-23 17:50_

---

_Review comment by @hamirmahal on `.github/workflows/ci.yml`:1611 on 2024-10-23 17:50_

At the time of this writing, [`gabrielfalcao/pyenv-action@v18`](https://github.com/gabrielfalcao/pyenv-action/) hasn't been updated in about 10 months.

---

_@charliermarsh approved on 2024-10-24 02:19_

---

_Merged by @charliermarsh on 2024-10-24 02:28_

---

_Closed by @charliermarsh on 2024-10-24 02:28_

---

_Label `testing` added by @charliermarsh on 2024-10-24 02:28_

---

_Branch deleted on 2024-10-24 02:42_

---
