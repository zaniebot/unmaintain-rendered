```yaml
number: 13171
title: Reproducible uv build backend across operating systems
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
  - windows
  - preview
assignees: []
merged: true
base: main
head: konsti/test-determinism-across-machines
created_at: 2025-04-28T08:46:21Z
updated_at: 2025-05-06T16:51:57Z
url: https://github.com/astral-sh/uv/pull/13171
synced_at: 2026-01-10T11:10:40Z
```

# Reproducible uv build backend across operating systems

---

_Pull request opened by @konstin on 2025-04-28 08:46_

The goal of this PR is to support reproducible builds and best-effort platform-independent builds. Previously, while the build backend would build the same source dist and wheel on the same machine, they would look different across different operating systems. This PR fixes the platform-dependent walk dir order by sorting and removes platform-specific permissions from the source dist that had caused those differences.

The reproducibility goal does not extend to platform-dependent filesystem features, such as permissions and links, especially in interaction with Git. Since most users share code across platforms through Git, we're focusing on cross-platform behavior under Git. One of those caveats is intentional: If a file, such as a bash script, has an executable bit, we preserve it. This means that E.g. builds of Git checkout of a repository with an executable shell script in the sources will have different archives on Unix and Windows. Another relevant case are symlinks: By default, Git on Windows replaces symlinks with a file that contains the path to the target file (https://stackoverflow.com/q/5917249/3549270). (This example comes from Cargo, where it means that the package archive is different on Windows when symlinking license from the repository root to a workspace package)

Best reviewed commit-by-commit

---

_Label `enhancement` added by @konstin on 2025-04-28 08:46_

---

_Label `preview` added by @konstin on 2025-04-28 08:46_

---

_Renamed from "Test build backend determinism across machines" to "Reproducible uv build backend across operating systems" by @konstin on 2025-05-03 22:08_

---

_Review requested from @BurntSushi by @konstin on 2025-05-03 23:07_

---

_Label `windows` added by @konstin on 2025-05-03 23:09_

---

_@BurntSushi approved on 2025-05-06 15:59_

Makes sense to me! Thanks for the commit-by-commit PR. :-)

---

_Merged by @konstin on 2025-05-06 16:51_

---

_Closed by @konstin on 2025-05-06 16:51_

---

_Branch deleted on 2025-05-06 16:51_

---
