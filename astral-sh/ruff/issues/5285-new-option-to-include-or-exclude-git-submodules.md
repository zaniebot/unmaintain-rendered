```yaml
number: 5285
title: New option to include or exclude git submodules
type: issue
state: closed
author: tgross35
labels:
  - question
  - configuration
assignees: []
created_at: 2023-06-22T06:17:01Z
updated_at: 2023-06-23T00:35:15Z
url: https://github.com/astral-sh/ruff/issues/5285
synced_at: 2026-01-10T11:09:47Z
```

# New option to include or exclude git submodules

---

_Issue opened by @tgross35 on 2023-06-22 06:17_

Would it be possible to add a configuration option to automatically opt git submodules in or out?

It's fairly common to have submodules be read-only within a parent repository, meaning that any `ruff` errors need to be fixed in that submodule, rather than in the parent project. When there are multiple (or potentially changing) submodules, it would be nicer to just be able to exclude them all, rather than needing to manually track their paths in the configuration file.

Related: #4920

---

_Label `question` added by @charliermarsh on 2023-06-22 13:10_

---

_Label `configuration` added by @charliermarsh on 2023-06-22 13:10_

---

_Comment by @charliermarsh on 2023-06-23 00:35_

I think I'd be open to this if we were supported by ripgrep's `ignore` crate, but given that they [closed this as not-implemented](https://github.com/BurntSushi/ripgrep/issues/23), I'd rather just defer to the existing `exclude` behavior for now.



---

_Closed by @charliermarsh on 2023-06-23 00:35_

---
