```yaml
number: 15568
title: Run linux-cross GitHub Action on an ARM processor
type: pull_request
state: closed
author: cclauss
labels:
  - ci
assignees: []
base: main
head: patch-2
created_at: 2025-01-18T08:00:37Z
updated_at: 2025-01-18T15:01:36Z
url: https://github.com/astral-sh/ruff/pull/15568
synced_at: 2026-01-12T15:55:51Z
```

# Run linux-cross GitHub Action on an ARM processor

---

_@cclauss_

Speed up a GitHub Action by running it on an ARM processor. https://docs.github.com/en/actions/using-github-hosted-runners/using-github-hosted-runners/about-github-hosted-runners#supported-runners-and-hardware-resources
> WARNING: The requested image's platform (linux/amd64) does not match the detected host platform (linux/arm64/v8) and no specific platform was requested

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Comment by @MichaReiser on 2025-01-18 14:06_

Thanks for looking into this. It seems the ecosystem hasn't catched up with the new runners and there's some weird mismatch error. Are you planning to look more into resolving the build error?

---

_Label `ci` added by @MichaReiser on 2025-01-18 14:06_

---

_Comment by @cclauss on 2025-01-18 15:01_

Let’s allow the ecosystem to catch up before trying again.

---

_Closed by @cclauss on 2025-01-18 15:01_

---

_Branch deleted on 2025-01-18 15:01_

---
