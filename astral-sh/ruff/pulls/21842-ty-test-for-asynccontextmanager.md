```yaml
number: 21842
title: "[ty] Test for `@asynccontextmanager`"
type: pull_request
state: closed
author: sharkdp
labels:
  - testing
  - ty
assignees: []
base: main
head: david/asynccontextmanager-test
created_at: 2025-12-08T12:00:08Z
updated_at: 2025-12-08T12:46:14Z
url: https://github.com/astral-sh/ruff/pull/21842
synced_at: 2026-01-12T15:57:35Z
```

# [ty] Test for `@asynccontextmanager`

---

_@sharkdp_

## Summary

A test for https://github.com/astral-sh/ty/issues/1804


---

_Review requested from @carljm by @sharkdp on 2025-12-08 12:00_

---

_Label `testing` added by @sharkdp on 2025-12-08 12:00_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-12-08 12:00_

---

_Review requested from @dcreager by @sharkdp on 2025-12-08 12:00_

---

_Label `ty` added by @sharkdp on 2025-12-08 12:00_

---

_@AlexWaygood approved on 2025-12-08 12:04_

You could possibly also add tests where `@asynccontextmanager` decorates functions annotated as returning `AsyncGeneratorType` and `AsyncIterator`. (`@asynccontextmanager` accepting functions returning `AsyncIterator` [is questionable](https://github.com/python/typeshed/issues/2772), but typeshed allows it for now for backwards compatibility.)

---

_@dhruvmanila reviewed on 2025-12-08 12:21_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/async.md`:101 on 2025-12-08 12:21_

Isn't this the same as https://github.com/astral-sh/ruff/blob/98d67bb830e73527a7fbf969c4786de23693f9b2/crates/ty_python_semantic/resources/mdtest/with/async.md#asynccontextmanager ?

---

_@sharkdp reviewed on 2025-12-08 12:32_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/async.md`:101 on 2025-12-08 12:32_

Wow. I swore I thought I had written that test before, but I am apparently too stupid to search properly. Fascinating that I wrote the exact same thing again.

Sorry!

---

_Closed by @sharkdp on 2025-12-08 12:32_

---

_@dhruvmanila reviewed on 2025-12-08 12:37_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/resources/mdtest/async.md`:101 on 2025-12-08 12:37_

Hehe, I had to update this test case while working on `ParamSpec` so it was fresh in my memory ;)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/async.md`:101 on 2025-12-08 12:46_

And I reviewed Dhruv's update of the test, and left a comment on that exact line! Can't believe it already fell out of _my_ memory 😆

---

_@AlexWaygood reviewed on 2025-12-08 12:46_

---
