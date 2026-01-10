---
number: 13137
title: "When downloading packages in CI action UV doesn't print which version it resolved to download."
type: issue
state: open
author: CompRhys
labels:
  - enhancement
assignees: []
created_at: 2025-04-27T18:24:56Z
updated_at: 2025-04-27T20:09:06Z
url: https://github.com/astral-sh/uv/issues/13137
synced_at: 2026-01-10T01:25:29Z
---

# When downloading packages in CI action UV doesn't print which version it resolved to download.

---

_Issue opened by @CompRhys on 2025-04-27 18:24_

### Summary

When the action completes sucessfully UV will print what is installed in the environment but if the action fails to complete it is unclear what version it was trying to install when the action failed.

### Example

For example here the version of `metatensor-torch` is causing an issue due to a recent release that has an inconsistent sub-dependency.  Given the action failed we can't see what version was downloaded in CI.

https://github.com/Radical-AI/torch-sim/actions/runs/14690691134/job/41225548805

---

_Label `enhancement` added by @CompRhys on 2025-04-27 18:24_

---

_Comment by @charliermarsh on 2025-04-27 18:40_

Hmm, I don't think that's a uv error. It looks like a runtime error, from trying to import the package? We don't have any control over the error message there.

---

_Comment by @CompRhys on 2025-04-27 20:09_

Sorry yes I wasn't clear there is no error with UV. The linked error is supposed to support the point that it would be nice if when UV prints downloading XYZ it gave the version in brackets as that would make it easier to diagnose which version caused the error in instances where the dependencies may be unpinned. 

---

_Referenced in [astral-sh/uv#12654](../../astral-sh/uv/issues/12654.md) on 2025-06-05 14:09_

---
