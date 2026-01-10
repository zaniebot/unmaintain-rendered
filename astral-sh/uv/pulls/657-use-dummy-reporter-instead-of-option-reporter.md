```yaml
number: 657
title: Use dummy reporter instead of option reporter
type: pull_request
state: closed
author: konstin
labels: []
assignees: []
base: main
head: konstin/dummy-reporter
created_at: 2023-12-15T09:33:05Z
updated_at: 2024-02-01T04:50:10Z
url: https://github.com/astral-sh/uv/pull/657
synced_at: 2026-01-10T15:39:02Z
```

# Use dummy reporter instead of option reporter

---

_Pull request opened by @konstin on 2023-12-15 09:33_

I'm experimenting with getting with of the `if let Some(reporter) = self.reporter {...}` and `with_reporter()` methods. Added bonus is that we see where we're currently loosing information, as we currently do in `BuildDispatch::install`.

---

_@charliermarsh reviewed on 2023-12-15 17:01_

This looks reasonable, though maybe we should have two separate constructors...? Wdyt?

---

_Comment by @konstin on 2023-12-17 04:20_

You mean adding two separate constructors to types that take a reporter? I kinda like making it explicit where we're dropping messages that we would otherwise report.

---

_Comment by @charliermarsh on 2024-02-01 04:50_

Let's close for now, can re-open if we choose to revisit.

---

_Closed by @charliermarsh on 2024-02-01 04:50_

---
