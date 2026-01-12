```yaml
number: 14539
title: Ruff removes import for fixture used by pytest
type: issue
state: closed
author: jkugler
labels: []
assignees: []
created_at: 2024-11-22T20:02:19Z
updated_at: 2024-11-22T21:23:58Z
url: https://github.com/astral-sh/ruff/issues/14539
synced_at: 2026-01-12T15:54:53Z
```

# Ruff removes import for fixture used by pytest

---

_@jkugler_

I searched for "remove pytest fixture"

I have code like this:
```py
from my_code import my_fixture
...
def test_my_thing(my_fixture):
    do_the_test(my_fixture)
```
Ruff sees `my_fixture` as unused and removes the `import` line. This, of course, then breaks the test.

This was done with `ruff check --fix` on ruff 0.8.0, with no special settings.

---

_Comment by @dylwil3 on 2024-11-22 20:30_

Sorry to hear this broke your tests, that's frustrating!

This is a bit challenging to address because pytest is (if I understand correctly) performing a custom parse of your test in order to actually "insert" the fixture as an argument to that function. From the point of view of the actual Python specification the imported `my_fixture` really _is_ unused.

Would it be possible in your use-case to:

1. Ignore the rule [unused-import (F401)](https://docs.astral.sh/ruff/rules/unused-import/#unused-import-f401) for test files by using the [`exclude`](https://docs.astral.sh/ruff/settings/#exclude) option, or by making a separate config in your tests directory? or
2. Use [conftest.py](https://docs.pytest.org/en/stable/reference/fixtures.html#conftest-py-sharing-fixtures-across-multiple-files) so you don't have to import fixtures?

---

_Comment by @dylwil3 on 2024-11-22 20:38_

Ah, and in fact it looks like this is a duplicate of #3295 - so maybe further discussion should be moved there!

---

_Closed by @dylwil3 on 2024-11-22 20:38_

---

_Comment by @jkugler on 2024-11-22 21:23_

Thanks!

---
