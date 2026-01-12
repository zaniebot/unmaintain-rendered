```yaml
number: 10063
title: Avoid duplicating backslashes in sysconfig parser
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/backslash
created_at: 2024-12-20T18:15:12Z
updated_at: 2024-12-20T18:52:44Z
url: https://github.com/astral-sh/uv/pull/10063
synced_at: 2026-01-12T16:09:06Z
```

# Avoid duplicating backslashes in sysconfig parser

---

_@charliermarsh_

## Summary

We had a bug in our handling of escape sequences that caused us to duplicate backslashes. If you installed repeatedly, we'd keep doubling them, leading to an exponential blowup.

Closes #10060.


---

_Marked ready for review by @charliermarsh on 2024-12-20 18:15_

---

_Label `bug` added by @charliermarsh on 2024-12-20 18:15_

---

_Review requested from @zanieb by @charliermarsh on 2024-12-20 18:15_

---

_Review requested from @konstin by @charliermarsh on 2024-12-20 18:15_

---

_Review requested from @dhruvmanila by @charliermarsh on 2024-12-20 18:15_

---

_@charliermarsh reviewed on 2024-12-20 18:15_

---

_Review comment by @charliermarsh on `crates/uv-python/src/sysconfig/parser.rs`:203 on 2024-12-20 18:15_

I don't think it's necessary to add parsing for these... We know this file doesn't contain these, we generate it.

---

_@charliermarsh reviewed on 2024-12-20 18:20_

---

_Review comment by @charliermarsh on `crates/uv-python/src/sysconfig/parser.rs`:180 on 2024-12-20 18:20_

I don't think it's necessary for us to handle all the cases here. We generate this file, we know it doesn't contain (e.g.) Unicode names.

---

_Review requested from @BurntSushi by @zanieb on 2024-12-20 18:21_

---

_@BurntSushi approved on 2024-12-20 18:49_

Nice!

---

_Merged by @charliermarsh on 2024-12-20 18:52_

---

_Closed by @charliermarsh on 2024-12-20 18:52_

---

_Branch deleted on 2024-12-20 18:52_

---
