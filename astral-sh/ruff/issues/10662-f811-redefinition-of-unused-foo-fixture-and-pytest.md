```yaml
number: 10662
title: "F811 Redefinition of unused `foo_fixture` and Pytest"
type: issue
state: closed
author: arrdem
labels:
  - needs-decision
assignees: []
created_at: 2024-03-29T19:43:01Z
updated_at: 2024-04-10T19:51:27Z
url: https://github.com/astral-sh/ruff/issues/10662
synced_at: 2026-01-12T15:54:50Z
```

# F811 Redefinition of unused `foo_fixture` and Pytest

---

_@arrdem_

- **Ruff version:** 0.3.3
- **Rule:** redefined-while-unused / F811

### Ex. foo_test.py
```
from .fixtures import foo_fixture

def test_bar_using_foo(foo_fixture):
   with foo_fixture() as foo:
      assert foo.bar() == "baz"
```

When pytest runs test code it inspects both the namespaces defined by sibling and parent `conftest.py` files for fixtures, as well as the namespace of the package containing a given test. This makes it possible (although admittedly not great style) for users to explicitly import fixtures they intend to use so as to make them available to pytest in the module.

Doing so produces spurious instances of F811.

While I wouldn't really expect ruff to do a dynamic analysis to see if the imported entity is marked as `@pytest.fixture` and while this is an idiom I intend to move the codebase away from, it would be nice if we could at a minimum selectively ignore this error generally within the scope of the tests tree without disabling it generally.

---

_Comment by @charliermarsh on 2024-03-30 00:47_

This has come up a few times:

- https://github.com/astral-sh/ruff/issues/3295
- https://github.com/astral-sh/ruff/issues/4046

I think the conclusion we came to was that we didn't have a reliable way to detect these without re-implementing a bunch of `pytest`-specific logic.


---

_Label `needs-decision` added by @charliermarsh on 2024-03-30 00:47_

---

_Comment by @dhruvmanila on 2024-04-01 08:40_

> it would be nice if we could at a minimum selectively ignore this error generally within the scope of the tests tree without disabling it generally.

I'm sorry but I'm unable to follow this. Do you want to ignore `F811` in test files? If so, you could use [`lint.extend-per-file-ignores`](https://docs.astral.sh/ruff/settings/#lint_extend-per-file-ignores).

---

_Comment by @arrdem on 2024-04-03 02:33_

I think this would do what I want --

```
[tool.ruff.lint.extend-per-file-ignores]
"src/pytests/**/test_*.py" = ["F811"]
"src/pytests/**/*_test.py" = ["F811"]
```

---

_Closed by @arrdem on 2024-04-10 19:51_

---
