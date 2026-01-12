```yaml
number: 3558
title: Make isolated a global argument
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - cli
assignees: []
merged: true
base: main
head: charlie/iso
created_at: 2024-05-13T17:45:34Z
updated_at: 2024-05-13T19:43:27Z
url: https://github.com/astral-sh/uv/pull/3558
synced_at: 2026-01-12T16:05:42Z
```

# Make isolated a global argument

---

_@charliermarsh_

Closes https://github.com/astral-sh/uv/issues/3557.

---

_Review requested from @zanieb by @charliermarsh on 2024-05-13 17:45_

---

_@charliermarsh reviewed on 2024-05-13 17:45_

---

_Review comment by @charliermarsh on `crates/uv/src/cli.rs`:93 on 2024-05-13 17:45_

We needed `global = true`.

---

_Label `bug` added by @charliermarsh on 2024-05-13 17:45_

---

_Label `cli` added by @charliermarsh on 2024-05-13 17:45_

---

_Marked ready for review by @charliermarsh on 2024-05-13 17:45_

---

_@zanieb approved on 2024-05-13 17:46_

<3

---

_Merged by @charliermarsh on 2024-05-13 17:51_

---

_Closed by @charliermarsh on 2024-05-13 17:51_

---

_Branch deleted on 2024-05-13 17:51_

---

_Comment by @bluss on 2024-05-13 19:07_

the global options open the secret door where uv tells you about all you can really do. Don't know any other way to get it to spill the beans.

```
uv --preview
error: 'uv' requires a subcommand but one was not provided
  [subcommands: pip, venv, virtualenv, v, cache, self, clean, run, sync, lock,
version, generate-shell-completion, --generate-shell-completion, help]
```

---

_Comment by @charliermarsh on 2024-05-13 19:10_

Hahaha the hidden subcommands?

---

_Comment by @bluss on 2024-05-13 19:13_

Yep :slightly_smiling_face:  I guess, the shell completion is pretty open about them too though. Not `--generate-shell-completion` with dashes, that one is secret and `v` too.

---

_Comment by @zanieb on 2024-05-13 19:43_

Funny that seems like a Clap bug.

They're not too secret though :) just not ready for primetime.

---
