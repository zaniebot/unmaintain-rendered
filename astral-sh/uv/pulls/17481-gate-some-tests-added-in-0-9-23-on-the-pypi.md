```yaml
number: 17481
title: Gate some tests added in 0.9.23 on the pypi feature
type: pull_request
state: open
author: musicinmybrain
labels: []
assignees: []
base: main
head: pypi-0.9.23
created_at: 2026-01-15T07:31:04Z
updated_at: 2026-01-15T07:31:04Z
url: https://github.com/astral-sh/uv/pull/17481
synced_at: 2026-01-15T07:48:40Z
```

# Gate some tests added in 0.9.23 on the pypi feature

---

_@musicinmybrain_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Makes it easier to run integration tests in offline environments by gating four new tests on the `pypi` feature. All of these tests fail in offline environments when they try to make HTTPS requests to PyPI.
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->
Applied as a patch to Fedora’s [`uv` package](https://src.fedoraproject.org/rpms/uv); built and ran integration tests in an offline environment.

---
