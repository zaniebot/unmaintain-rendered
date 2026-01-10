```yaml
number: 12935
title: Generate artifact attestations for release binaries, too
type: pull_request
state: closed
author: scop
labels: []
assignees: []
base: main
head: release-artifact-attestations
created_at: 2025-04-17T06:44:10Z
updated_at: 2025-11-26T20:59:42Z
url: https://github.com/astral-sh/uv/pull/12935
synced_at: 2026-01-10T05:49:14Z
```

# Generate artifact attestations for release binaries, too

---

_Pull request opened by @scop on 2025-04-17 06:44_


<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Motivation for this is to have ability to verify the artifacts downloaded from GH release, just like it is already possible for container images.

Tools such as [aqua](https://aquaproj.github.io) and [mise](https://mise.jdx.dev) ([likely, later](https://github.com/jdx/mise/discussions/4531)) can make use of these and verify downloads automatically.

## Test Plan

<!-- How was it tested? -->

This is untested by me. The CI setup is quite elaborate, not sure if I could get it to run properly in my fork with reasonable effort. But I suppose this _could_ Just Work anyway :)

---

_Comment by @konstin on 2025-04-17 09:43_

Have you seen #11357?

---

_Comment by @scop on 2025-04-17 11:34_

Nope, hadn't seen that, glad to hear this is being worked on, subscribed there. I don't know if there's anything in this one that would add value to the other (probably not), feel free to close this one if it doesn't seem so.

---

_Comment by @konstin on 2025-04-17 12:12_

Thanks, closing in favor of https://github.com/astral-sh/uv/pull/11357

---

_Closed by @konstin on 2025-04-17 12:12_

---

_Branch deleted on 2025-11-26 20:59_

---
