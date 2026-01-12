```yaml
number: 11869
title: "Auto-exec compatible uv version when required-version doesn't match"
type: pull_request
state: closed
author: aarondobbing
labels: []
assignees: []
base: main
head: required-version-fix
created_at: 2025-02-28T21:33:52Z
updated_at: 2025-05-27T16:33:36Z
url: https://github.com/astral-sh/uv/pull/11869
synced_at: 2026-01-12T16:10:01Z
```

# Auto-exec compatible uv version when required-version doesn't match

---

_@aarondobbing_

 Implements #11065


## Summary

  When a project has a required-version constraint in its pyproject.toml
  that doesn't match the current uv version, automatically use `uv tool run`
  to execute a compatible version instead of exiting with an error.

  This allows developers with newer uv versions to still work with projects
  that need older versions, without having to manually use `uv tool run`
  for each command to ensure a consistent development experience.

:chef_kiss:

## Test Plan

Unit tests covering the cases described in issue #11065 

Verified locally in sample python projects matching the described issues outlined by @mjpieters 

---

_Assigned to @zanieb by @zanieb on 2025-03-01 17:29_

---

_Comment by @zanieb on 2025-03-01 17:30_

Cool! I'll give this a look.

Did you explore calling into the `tool_run` implementation directly instead of spawning a subprocess?

---

_Comment by @aarondobbing on 2025-03-01 20:57_

I didn't explore it - Not familiar with the uv project in general, was just looking to pick a first issue that would support me in my day to day :sweat_smile: 

Totally makes sense as an approach - Will give it a spin locally and throw in an update up if I can get something tenable together later.

Thanks for feedback :pray: 

---

_Comment by @zanieb on 2025-04-16 15:26_

Hey! Just wondering if you're still poking at this?

---

_Comment by @zanieb on 2025-05-27 16:33_

Closing as stale, feel free to pick this back up!

---

_Closed by @zanieb on 2025-05-27 16:33_

---
