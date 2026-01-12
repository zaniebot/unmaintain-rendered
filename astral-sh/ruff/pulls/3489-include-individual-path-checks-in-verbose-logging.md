```yaml
number: 3489
title: Include individual path checks in --verbose logging
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/log
created_at: 2023-03-13T20:03:56Z
updated_at: 2023-03-13T21:13:49Z
url: https://github.com/astral-sh/ruff/pull/3489
synced_at: 2026-01-12T15:55:13Z
```

# Include individual path checks in --verbose logging

---

_@charliermarsh_

This modifies `ruff /path/to/file.py --verbose` to include a message for each file that's checked. `--verbose` is, in truth, more like `--debug`, but this generally fits with what we include in verbose logging.

Closes #3423.

---

_Review requested from @konstin by @charliermarsh on 2023-03-13 20:07_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-03-13 20:07_

---

_@charliermarsh reviewed on 2023-03-13 20:11_

---

_Review comment by @charliermarsh on `crates/ruff/src/resolver.rs`:131 on 2023-03-13 20:11_

Separately: pretty sure we want to be using `.display` when constructing user-facing paths?

---

_Comment by @github-actions[bot] on 2023-03-13 20:15_

✅ ecosystem check detected no changes.

<!-- thollander/actions-comment-pull-request "ecosystem-results" -->

---

_@MichaReiser approved on 2023-03-13 20:49_

Does the logging add any noticeable performance penalty?

---

_Comment by @charliermarsh on 2023-03-13 20:50_

When running with `--verbose`, or without?

---

_Comment by @MichaReiser on 2023-03-13 21:02_

Mainly without. I'm not really concerned about performance when using verbose.

---

_Comment by @charliermarsh on 2023-03-13 21:10_

It does seem noticeably slower by a few ms (on a ~214ms baseline). I don't understand why or how though. Doesn't `path.display()` only execute when the log level is set to debug or higher?

---

_Comment by @charliermarsh on 2023-03-13 21:13_

Hmm, actually, this just looks like noise. I need a more reliable benchmark.

---

_Merged by @charliermarsh on 2023-03-13 21:13_

---

_Closed by @charliermarsh on 2023-03-13 21:13_

---

_Branch deleted on 2023-03-13 21:13_

---
