```yaml
number: 1948
title: Warn on (accidentally) unreachable code blocks
type: issue
state: open
author: AlexWaygood
labels:
  - unreachable-code
  - lint
assignees: []
created_at: 2025-12-16T17:36:25Z
updated_at: 2025-12-16T18:42:51Z
url: https://github.com/astral-sh/ty/issues/1948
synced_at: 2026-01-10T01:55:00Z
```

# Warn on (accidentally) unreachable code blocks

---

_Issue opened by @AlexWaygood on 2025-12-16 17:36_

Mypy and pyright both offer opt-in lints which, when enabled, cause them to complain about the `else` branch in this snippet:

```py
def f(x: int | str):
    if isinstance(x, int):
        print("It's an int")
    elif isinstance(x, str):
        print("It's a string")
    else:
        print("It's something else")
```

They can both detect that the `else` branch is unreachable, and that this is probably an accident, indicating a logic error somewhere in the function. Mypy's version can be opted into using `--warn-unreachable` on the command line; pyright's version requires `reportUnreachable=true`.

For feature parity with these tools, we should aim to provide a similar lint.

Note that the lint should only complain about _accidentally_ unreachable code. There's a lot of code that ty infers as being unreachable that is nonetheless _deliberately_ unreachable, which we should _not_ complain about:
- Any unreachable branches that consist solely of a `raise` statement should not be reported
- Any unreachable branches that consist solely of an `assert_never()` call should not be reported
- Any branches that are unreachable due to a `sys.version_info`, `sys.platform`, or `if TYPE_CHECKING` check should not be reported

---

_Added to milestone `Stable` by @AlexWaygood on 2025-12-16 17:36_

---

_Label `unreachable-code` added by @AlexWaygood on 2025-12-16 17:36_

---

_Label `lint` added by @AlexWaygood on 2025-12-16 18:42_

---
