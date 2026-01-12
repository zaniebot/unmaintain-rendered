```yaml
number: 10357
title: "docs: add that uv sync and install include dev dependencies by default"
type: pull_request
state: closed
author: clintonsteiner
labels: []
assignees: []
base: main
head: mentionWhichDependenciesAreInstalledByDefault
created_at: 2025-01-07T12:03:18Z
updated_at: 2025-01-07T22:46:06Z
url: https://github.com/astral-sh/uv/pull/10357
synced_at: 2026-01-12T16:09:15Z
```

# docs: add that uv sync and install include dev dependencies by default

---

_@clintonsteiner_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Review requested from @zanieb by @konstin on 2025-01-07 12:19_

---

_Renamed from "docs: add that uv sync and install include dev dependencies by defaulta" to "docs: add that uv sync and install include dev dependencies by default" by @AlexWaygood on 2025-01-07 13:05_

---

_@zanieb reviewed on 2025-01-07 18:26_

---

_Review comment by @zanieb on `docs/concepts/projects/dependencies.md`:595 on 2025-01-07 18:26_

We already say the `dev` group is synced by default here.

---

_@zanieb reviewed on 2025-01-07 18:27_

---

_Review comment by @zanieb on `docs/concepts/projects/dependencies.md`:599 on 2025-01-07 18:27_

This command wouldn't work as-written.

I'm not sure the others make sense here either, as this section is specifically about the `dev` group and the rest are covered elsewhere.

---

_Review comment by @clintonsteiner on `docs/concepts/projects/dependencies.md`:595 on 2025-01-07 19:34_

When I read this - I wasn't aware that "sync" meant the same thing as install, happy to close though if others didn't have this misunderstanding

---

_@clintonsteiner reviewed on 2025-01-07 19:34_

---

_@clintonsteiner reviewed on 2025-01-07 19:35_

---

_Review comment by @clintonsteiner on `docs/concepts/projects/dependencies.md`:599 on 2025-01-07 19:35_

Was attempting to add a couple mentinons explaining how the dependency groups work, revising the command

---

_Comment by @zanieb on 2025-01-07 22:39_

Yeah sorry, but I'm not sure this is an improvement. Feel free to open an issue for discussion on the point of confusion if you want.

---

_Closed by @zanieb on 2025-01-07 22:39_

---

_@zanieb reviewed on 2025-01-07 22:39_

---

_Review comment by @zanieb on `docs/concepts/projects/dependencies.md`:600 on 2025-01-07 22:39_

Using `uv python install` won't include any groups — it just installs a Python version.

---

_@zanieb reviewed on 2025-01-07 22:40_

---

_Review comment by @zanieb on `docs/concepts/projects/dependencies.md`:595 on 2025-01-07 22:40_

We don't have a top level `uv install` command. Perhaps we could clarify what `sync` does, but I think it's mentioned elsewhere?

---

_@zanieb reviewed on 2025-01-07 22:40_

---

_Review comment by @zanieb on `docs/concepts/projects/dependencies.md`:601 on 2025-01-07 22:40_

Extras are distinct from groups and have a separate section in these docs

---

_@zanieb reviewed on 2025-01-07 22:40_

---

_Review comment by @zanieb on `docs/concepts/projects/dependencies.md`:603 on 2025-01-07 22:40_

These two would make more sense in the following "Dependency groups" section, e.g., after

> Once groups are defined, the `--group`, `--only-group`, and `--no-group` options can be used to
include or exclude their dependencies.

---

_Branch deleted on 2025-01-07 22:46_

---
