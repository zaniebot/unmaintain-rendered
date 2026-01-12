```yaml
number: 16430
title: Update docs to reflect new signal forwarding semantics
type: pull_request
state: merged
author: bdentino
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2025-10-24T04:32:14Z
updated_at: 2025-10-24T19:52:08Z
url: https://github.com/astral-sh/uv/pull/16430
synced_at: 2026-01-12T16:12:15Z
```

# Update docs to reflect new signal forwarding semantics

---

_@bdentino_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
I am a new uv user and I was reading the docs to better understand the project scope & best practices. The section on [signal forwarding](https://docs.astral.sh/uv/concepts/projects/run/#signal-handling) with `uv run` caught my eye because I've used tools that use SIGHUP to trigger config reloads or SIGUSR1/2 to enable debugging/profiling/etc so I was a little concerned about using a runner that might block those signals. After some searching in issues/PRs, I found that this behavior was actually [changed earlier this year](https://github.com/astral-sh/uv/pull/13017) to forward additional signals (awesome!) and thought I would update the docs and save the next person/llm from thinking their tool won't work as expected if it uses custom signal handling. 

Thanks for all your great work!

P.S. If you think it makes more sense to explicitly list all forwarded signals as opposed to just the exclusions, I'm happy to edit.



---

_@zanieb approved on 2025-10-24 19:51_

---

_Comment by @zanieb on 2025-10-24 19:51_

Thanks!

---

_Label `documentation` added by @zanieb on 2025-10-24 19:52_

---

_Merged by @zanieb on 2025-10-24 19:52_

---

_Closed by @zanieb on 2025-10-24 19:52_

---
