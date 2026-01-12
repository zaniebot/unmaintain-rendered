```yaml
number: 9976
title: "Expand `asyncio-dangling-task` (`RUF006`) to include `new_event_loop`"
type: pull_request
state: merged
author: tyilo
labels:
  - rule
assignees: []
merged: true
base: main
head: improve-ruf006
created_at: 2024-02-13T17:59:51Z
updated_at: 2024-02-13T23:51:59Z
url: https://github.com/astral-sh/ruff/pull/9976
synced_at: 2026-01-12T15:55:30Z
```

# Expand `asyncio-dangling-task` (`RUF006`) to include `new_event_loop`

---

_@tyilo_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes #9974

## Test Plan

I added some new test cases.


---

_Comment by @github-actions[bot] on 2024-02-13 18:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-02-13 18:16_

Thanks! Addressed the TODO.

---

_Label `bug` added by @charliermarsh on 2024-02-13 18:16_

---

_Renamed from "Improve RUF006 detection" to "Expand `asyncio-dangling-task` (`RUF006`) to include `new_event_loop`" by @charliermarsh on 2024-02-13 18:16_

---

_Label `bug` removed by @charliermarsh on 2024-02-13 18:16_

---

_Label `rule` added by @charliermarsh on 2024-02-13 18:16_

---

_Review comment by @tyilo on `crates/ruff_linter/src/rules/ruff/rules/asyncio_dangling_task.rs`:96 on 2024-02-13 18:17_

I don't know if ruff's typing analysis is good enough for this, but it would be better to check that the type of `value` is `asyncio.AbstractEventLoop`, so that the following would also be flagged:

```python
import asyncio


def create_event_loop() -> asyncio.AbstractEventLoop:
  return asyncio.new_event_loop()


loop = create_event_loop()
loop.create_task(asyncio.sleep(1))
```

---

_Merged by @charliermarsh on 2024-02-13 18:28_

---

_Closed by @charliermarsh on 2024-02-13 18:28_

---

_Comment by @tyilo on 2024-02-13 20:48_

@charliermarsh What do you think of my comments?

---

_Comment by @charliermarsh on 2024-02-13 21:14_

@tyilo - Which comments specifically? Sorry if I missed something.

---

_Review comment by @tyilo on `crates/ruff_linter/src/rules/ruff/rules/asyncio_dangling_task.rs`:107 on 2024-02-13 21:16_

I think `loop` would be a better fallback:
```suggestion
                        expr: compose_call_path(value).unwrap_or_else(|| "loop".to_string()),
```

---

_@tyilo reviewed on 2024-02-13 21:16_

---

_Comment by @tyilo on 2024-02-13 21:17_

> @tyilo - Which comments specifically? Sorry if I missed something.

Ah, sorry I haven't used GitHub's review functionality before, so my comments weren't published 😅

They should be visible now...

---

_@charliermarsh reviewed on 2024-02-13 23:51_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/asyncio_dangling_task.rs`:96 on 2024-02-13 23:51_

Yeah we don't quite support that yet unfortunately.

---

_@charliermarsh reviewed on 2024-02-13 23:51_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/asyncio_dangling_task.rs`:107 on 2024-02-13 23:51_

Will change, though I think in practice this should never happen. I wish it were encoded in the type system, but e.g. `value` here has to be a `Name`, and `compose_call_path` always returns `Some` given a name.

---
