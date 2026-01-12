```yaml
number: 11826
title: "Feature Request: Integrate Hook Support into the `uv` Command for Enhanced Project Management"
type: issue
state: open
author: pplmx
labels:
  - enhancement
assignees: []
created_at: 2025-02-27T09:34:15Z
updated_at: 2025-04-05T20:42:35Z
url: https://github.com/astral-sh/uv/issues/11826
synced_at: 2026-01-12T16:00:47Z
```

# Feature Request: Integrate Hook Support into the `uv` Command for Enhanced Project Management

---

_@pplmx_

### Summary

## Description

When the `uv` command is executed, it should automatically trigger specific hooks—similar to how npm executes `husky` and `lint-staged`.

While `pre-commit` offers comparable functionality, it cannot make the users install itself forcibly.

Integrating hooks directly into `uv` would streamline the workflow and ensure consistent project management without additional setup.

## Some Others

#9346
#9638
#9645


### Example

_No response_

---

_Label `enhancement` added by @pplmx on 2025-02-27 09:34_

---

_Comment by @konstin on 2025-02-27 17:59_

I'm not trying to preclude the discussion if such hooks should exist within uv itself, but another option here is using a task runner, either by adding it to uv itself or one of the many existing make-like programs.

---

_Comment by @pplmx on 2025-02-28 02:56_

I prefer the implementation within `uv` because `uv` can enforce it to some extent—similar to the relationship between `cargo` and `build.rs`, or `npm` and `husky`—whereas other make-like programs cannot achieve this level of enforcement.

---

_Comment by @rhuanbarreto on 2025-04-05 20:42_

Something like the postinstall hook from node would be interesting in uv IMO. We have some tasks that we need to run right after we run uv sync

---
