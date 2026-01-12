```yaml
number: 198
title: reimplement additional mypy error codes
type: issue
state: closed
author: ngnpope
labels: []
assignees: []
created_at: 2025-02-14T16:07:10Z
updated_at: 2025-05-12T10:38:40Z
url: https://github.com/astral-sh/ty/issues/198
synced_at: 2026-01-12T15:54:22Z
```

# reimplement additional mypy error codes

---

_@ngnpope_

### Description

There are a number of extra error codes not enabled by default in mypy, many of which are very handy:

```toml
enable_error_code = [
    "deprecated",
    "explicit-override",
    "ignore-without-code",
    "mutable-override",
    "possibly-undefined",
    "redundant-expr",
    "redundant-self",
    "truthy-bool",
    "truthy-iterable",
    "unimported-reveal",
    "unused-awaitable",
]
```

It would be useful for these to be reimplemented under `ruff`/`red-knot`.

Related issues:

- https://github.com/astral-sh/ruff/issues/3893
- https://github.com/astral-sh/ruff/issues/15828



---

_Comment by @MichaReiser on 2025-02-14 16:15_

Thanks for the suggestion. 

Just skimming through. To me it seems that most of them require multifile analysis to be useful and Ruff doesn't support that yet. That's why I'm inclined to close the issue as it isn't actionable until we implement multifile analysis (which we're working on but it isn't just around the corner). 

It's also likely that we implement some of those checks as part of red-knot, our work-in progress type checkers. 

Related issue: https://github.com/astral-sh/ruff/issues/9833

---

_Label `rule` added by @MichaReiser on 2025-02-14 16:15_

---

_Comment by @ngnpope on 2025-02-14 16:19_

> It's also likely that we implement some of those checks as part of red-knot, our work-in progress type checkers.

Sorry - yes - this was intended as raising interest w.r.t. that work rather than as lint rules for ruff (and for tracking).

---

_Comment by @MichaReiser on 2025-02-14 16:24_

No worries. it's good to know what you all are looking for. It helps with prioritisation :)

---

_Label `rule` removed by @AlexWaygood on 2025-02-15 11:15_

---

_Renamed from "Reimplement additional mypy error codes" to "[red-knot] reimplement additional mypy error codes" by @carljm on 2025-03-27 19:01_

---

_Renamed from "[red-knot] reimplement additional mypy error codes" to "reimplement additional mypy error codes" by @MichaReiser on 2025-05-07 15:26_

---

_Comment by @AlexWaygood on 2025-05-11 10:58_

I think this issue is too broad for it to be useful for us right now, and many will be covered by work we plan to do as part of other issues:
- `deprecated`: this is covered by https://github.com/astral-sh/ty/issues/153
- `explicit-override`: this is covered by https://github.com/astral-sh/ty/issues/155
- `mutable-override`: we plan to do this as part of our implementation of the Liskov Substitution Principle (https://github.com/astral-sh/ty/issues/166)
- `possibly-undefined`: this is already implemented (though it is currently disabled-by-default)
- `unimported-reveal`: this is already implemented

It could be useful to add an issue per error code for the remaining ones, but they all seem like relatively low priority for us right now. I won't object if you want to do that, but I won't take it on myself ;)

---

_Closed by @AlexWaygood on 2025-05-11 10:58_

---

_Comment by @ngnpope on 2025-05-12 10:38_

> It could be useful to add an issue per error code for the remaining ones, but they all seem like relatively low priority for us right now.

No worries. I may do in the future, but may as well wait to see how things develop and what the gaps are later 🙂 

---
