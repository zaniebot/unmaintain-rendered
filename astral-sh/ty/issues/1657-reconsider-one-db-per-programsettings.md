---
number: 1657
title: "Reconsider one `Db` per `ProgramSettings`"
type: issue
state: open
author: MichaReiser
labels:
  - internal
assignees: []
created_at: 2025-11-27T18:04:37Z
updated_at: 2026-01-09T10:24:39Z
url: https://github.com/astral-sh/ty/issues/1657
synced_at: 2026-01-10T01:48:23Z
---

# Reconsider one `Db` per `ProgramSettings`

---

_Issue opened by @MichaReiser on 2025-11-27 18:04_

Today, there's exactly one `ProgramSettings` per `Db` instance. I think the main motivation for this design were:

* Persistent caching: We can cache at a program settings level. E.g., using different cache files for each Python version
* It's simpler. `ProgramSetting`'s is a singleton and it can be queried from anywhere


However, I'm starting to see more downsides to this approach. Most notably, in larger workspaces where it's common for each member to have their own `src.root`, but all members use the same Python version. 

* We parse every imported file once for each workspace member (in which it is imported)
* We rebuild the semantic index for every imported file for each workspace member, even if all of them use the same Python version
* We rebuild auto completions for each workspace member rather than sharing a single index (or one per Python version)

I'm not entirely sure how such a new design would look like but we'll have to include the `PythonVersion` or the `ProgramSettings` as part of the query arguments. I think where this is going is that we'd have a `ProgramFile(File, ProgramSettings)` salsa ingredient and maybe a `FileWithPythonVersion(File, PythonVersion)` that we pass along instead of passing `File` (we can still pass `File` everywhere where the query result doesn't depend on `ProgramSettings` or the `PythonVersion`. 


I think this will become more relevant as we start looking into how to better support mono repositories and uv workspaces.

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-27 18:13_

---

_Label `internal` added by @carljm on 2025-12-02 03:11_

---

_Comment by @MichaReiser on 2026-01-09 10:24_

Related to https://github.com/astral-sh/ty/issues/2410 and https://github.com/astral-sh/ty/issues/819

---
