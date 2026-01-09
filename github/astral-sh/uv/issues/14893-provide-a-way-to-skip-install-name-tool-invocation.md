---
number: 14893
title: Provide a way to skip install_name_tool invocation?
type: issue
state: open
author: dae
labels:
  - configuration
assignees: []
created_at: 2025-07-25T12:43:19Z
updated_at: 2025-12-19T07:44:26Z
url: https://github.com/astral-sh/uv/issues/14893
synced_at: 2026-01-07T13:12:19-06:00
---

# Provide a way to skip install_name_tool invocation?

---

_Issue opened by @dae on 2025-07-25 12:43_

### Summary

Our app (https://apps.ankiweb.net) bundles uv and uses it to bootstrap/upgrade itself. The app is targeted at end users, who won't have development tools installed. Uv is currently unconditionally calling install_name_tool, which results in a pop-up prompting the user to install development tools:

<img width="583" height="100" alt="Image" src="https://github.com/user-attachments/assets/9b0c6834-7efa-4c2e-b32c-f27e8714be29" />

https://github.com/astral-sh/uv/blob/1146f3f62d88bcfe6d81a45987b1fd4745b3eeb4/crates/uv-python/src/managed.rs#L601

Is my understanding correct that the install_name_tool call is actually optional for this use case, where users won't be building modules against the managed Python instance? And if that's the case, would you be willing to accept a PR that makes this step opt-out with an env var or similar so the pop-up can be prevented?

### Platform

macOS 15.5

### Version

0.7.13

### Python version

3.13

---

_Label `bug` added by @dae on 2025-07-25 12:43_

---

_Label `bug` removed by @zanieb on 2025-07-25 12:52_

---

_Label `configuration` added by @zanieb on 2025-07-25 12:52_

---

_Comment by @zanieb on 2025-07-25 12:53_

Related to

- https://github.com/astral-sh/uv/pull/14869
- https://github.com/astral-sh/uv/issues/14843

This seems like further motivation to replace use of this tool with a manual patch. @geofft would you be able to prioritize that?

---

_Comment by @geofft on 2025-07-25 13:08_

Yup, lemme put something together!

---

_Referenced in [ankitects/anki#4227](../../ankitects/anki/issues/4227.md) on 2025-07-25 16:35_

---

_Referenced in [astral-sh/python-build-standalone#749](../../astral-sh/python-build-standalone/issues/749.md) on 2025-09-02 15:48_

---

_Comment by @MagerValp on 2025-12-19 07:44_

I'm running into this issue today with 0.9.18 where we're also distributing a tool to end users. Commit 06f9d41 mentions that it should "solve the problem for now", but we're still seeing the developer tools popup. Am I missing something?

---
