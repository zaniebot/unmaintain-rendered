---
number: 4103
title: No build isolation option for bluejay commands
type: issue
state: closed
author: konstin
labels: []
assignees: []
created_at: 2024-06-06T14:48:14Z
updated_at: 2024-08-22T01:52:03Z
url: https://github.com/astral-sh/uv/issues/4103
synced_at: 2026-01-10T01:23:34Z
---

# No build isolation option for bluejay commands

---

_Issue opened by @konstin on 2024-06-06 14:48_

Locking huggingface currently fails because it depends on [natten](https://pypi.org/project/natten/) which only hast source dists and can only be built without build isolation. `uv run`, `uv lock` and `uv sync` should support a `--no-build-isolation` flag.

---

_Label `preview` added by @konstin on 2024-06-06 14:48_

---

_Comment by @zanieb on 2024-06-06 14:56_

Is there an alternative design for `--no-build-isolation` we should consider? Like specifying extra build dependencies per package? Forcing the user to install build dependencies manually and dropping isolation seems like a bad outcome.

---

_Comment by @charliermarsh on 2024-06-06 16:07_

It's a good question to ask. Answering it requires understanding why these projects require `--no-build-isolation`. Often, it's because they require PyTorch, and my understanding is that people don't want to require that you reinstall PyTorch independently for each package, or something.

---

_Comment by @charliermarsh on 2024-06-06 16:07_

(Also note that `--no-build-isolation` doesn't mean that we combine the virtualenv with the build dependencies. It means the build dependencies are entirely ignored, i.e., it's assumed that the virtualenv has everything it needs.)

---

_Comment by @hmc-cs-mdrissi on 2024-06-06 20:17_

The most common reason is that there's build compatibility constraints not encodable in existing metadata today. https://pypackaging-native.github.io/key-issues/abi/ is probably best resource on this. At it's core installing separate pytorch  versions for each build can lead to conflicts that will make overall application unusable.

> people don't want to require that you reinstall PyTorch independently for each package, or something.

The not want here is not motivated by build speed, but compatibility and if you pick separate ones you may crash in the end.

You may be able to handle build dependencies if certain libraries can be marked as must be consistent across environment for all builds. If two libraries depend on library X at build time it is possible for the two to need the build time version of X to be same.

edit: So I think of it not as treating each build as independent is problematic assumption. Managing builds for users and having build dependencies installed is fine if build dependencies are locked together in consistent manner. One extra complication is some build dependencies are sensitive to this, while others are not and free to be inconsistent across builds.

edit 2: This issue is not unique to PyTorch. Any library with native build related abi constraints can make this an issue. If libraries build with numpy for example there exists oldest-supported-numpy as a trick related to this.

---

_Comment by @charliermarsh on 2024-07-28 00:43_

We may want to add this, since we haven't invested time in an alternative design yet.

---

_Label `preview` removed by @zanieb on 2024-08-20 18:23_

---

_Closed by @charliermarsh on 2024-08-22 01:52_

---
