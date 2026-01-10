```yaml
number: 4891
title: "uv/tests: fix tool_install_home test"
type: pull_request
state: merged
author: BurntSushi
labels:
  - testing
assignees: []
merged: true
base: main
head: ag/fix-tool-install-home
created_at: 2024-07-08T12:47:50Z
updated_at: 2024-07-08T13:39:29Z
url: https://github.com/astral-sh/uv/pull/4891
synced_at: 2026-01-10T13:42:52Z
```

# uv/tests: fix tool_install_home test

---

_Pull request opened by @BurntSushi on 2024-07-08 12:47_

This test was, I believe, relying on the XDG_DATA_HOME environment
variable not being set. When it is set, as is the case in my
environment, `uv tool run` will respect it and install `black` for this
particular test into my actual XDG_DATA_HOME directory. We fix this by
setting `XDG_DATA_HOME` explicitly.


---

_@charliermarsh approved on 2024-07-08 12:51_

---

_Merged by @BurntSushi on 2024-07-08 13:12_

---

_Closed by @BurntSushi on 2024-07-08 13:12_

---

_Branch deleted on 2024-07-08 13:12_

---

_Label `testing` added by @charliermarsh on 2024-07-08 13:24_

---

_Comment by @zanieb on 2024-07-08 13:39_

Ah thanks this confused me as well! I need to set up a shared fixture for these tests.

---
