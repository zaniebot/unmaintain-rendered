```yaml
number: 17426
title: Assume tar.gz for packages without extension
type: pull_request
state: open
author: ppalucha
labels: []
assignees: []
base: main
head: pp/assume-tgz
created_at: 2026-01-12T21:08:12Z
updated_at: 2026-01-12T21:13:45Z
url: https://github.com/astral-sh/uv/pull/17426
synced_at: 2026-01-12T21:26:06Z
```

# Assume tar.gz for packages without extension

---

_@ppalucha_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

This fixes issue with installing packages that contain URLs without explicit file extensions 

The example are GitHub releases that can be references with URLs like https://api.github.com/repos/pyro-ppl/pyro/tarball/dev (real example at https://github.com/pyro-ppl/pyro-api/blob/master/setup.py#L36).

If extension is not specified, we assume it is a tar.gz file, as it's a common pattern for GitHub.

Fixes: https://github.com/astral-sh/uv/issues/17425
Co-authored-by: Copilot Agent

## Test Plan

<!-- How was it tested? -->

I added test with installing a package without file extension..


---
