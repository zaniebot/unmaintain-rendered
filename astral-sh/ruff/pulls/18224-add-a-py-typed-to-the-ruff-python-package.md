```yaml
number: 18224
title: "add a `py.typed` to the `ruff` python package"
type: pull_request
state: closed
author: jorenham
labels: []
assignees: []
base: main
head: py.typed
created_at: 2025-05-20T15:39:12Z
updated_at: 2025-05-20T16:57:08Z
url: https://github.com/astral-sh/ruff/pull/18224
synced_at: 2026-01-10T18:51:02Z
```

# add a `py.typed` to the `ruff` python package

---

_Pull request opened by @jorenham on 2025-05-20 15:39_

## Summary

This way, static type-checkers won't complain anymore when doing `from ruff.__main__ import find_ruff_bin`.

More info:

- https://typing.python.org/en/latest/spec/distributing.html#packaging-typed-libraries
- https://peps.python.org/pep-0561/

## Test Plan

Not applicable I guess?


---

_Comment by @MichaReiser on 2025-05-20 15:52_

This depends on https://github.com/astral-sh/ruff/issues/18153 Let's wait until we have agreement if this is even considered public api

---

_Closed by @MichaReiser on 2025-05-20 15:52_

---

_Comment by @jorenham on 2025-05-20 15:56_

for what it's worth, `ty` has also has a `py.typed` in its python package: https://github.com/astral-sh/ty/pull/276

---

_Comment by @jorenham on 2025-05-20 15:59_

but even if you decide to not distribute it as a package anymore, having a `py.typed` in there won't hurt anything 🤷🏻 

---

_Comment by @jorenham on 2025-05-20 16:36_

Here's an example that shows the problem that this could solve: https://gist.github.com/jorenham/63942278b01515ffdeb0c7d4d1895684


---

_Comment by @MichaReiser on 2025-05-20 16:51_

I understand the problem this solves. The question is (and this is discussed in #18153) if `find_ruff_bin` is considered public API

---

_Comment by @jorenham on 2025-05-20 16:57_

> The question is (and this is discussed in #18153) if `find_ruff_bin` is considered public API

My point is that even if it isn't public API, a `py.typed` would still help (e.g. when used internally), and there are no downsides to having it. 

---
