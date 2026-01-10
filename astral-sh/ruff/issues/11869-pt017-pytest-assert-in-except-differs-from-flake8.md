```yaml
number: 11869
title: PT017 (pytest-assert-in-except) differs from flake8
type: issue
state: open
author: pcavalar
labels:
  - rule
assignees: []
created_at: 2024-06-14T10:28:39Z
updated_at: 2024-06-17T06:24:02Z
url: https://github.com/astral-sh/ruff/issues/11869
synced_at: 2026-01-10T11:09:53Z
```

# PT017 (pytest-assert-in-except) differs from flake8

---

_Issue opened by @pcavalar on 2024-06-14 10:28_

```python
# test-pt017.py
def test_foo():
    x = 0
    try:
        1 / x
    except ZeroDivisionError as e:
        assert x == 0, e
        assert e.args
```

Ruff flags the `assert x == 0, e` line as [`PT017(pytest-assert-in-except)`](https://docs.astral.sh/ruff/rules/pytest-assert-in-except/) even though the exception is not part of the predicate.
Flake8 only flags the `assert e.args` line.

This looks like a false-positive in ruff rather than a bug in flake8?

--- 

Ruff:
```console
$ ruff --version
ruff 0.4.8
$ ruff check --isolated --select=PT017 --output-format=full test-pt017.py
test-pt017.py:6:9: PT017 Found assertion on exception `e` in `except` block, use `pytest.raises()` instead
  |
4 |         1 / x
5 |     except ZeroDivisionError as e:
6 |         assert x == 0, e
  |         ^^^^^^^^^^^^^^^^ PT017
7 |         assert e.args
  |

test-pt017.py:7:9: PT017 Found assertion on exception `e` in `except` block, use `pytest.raises()` instead
  |
5 |     except ZeroDivisionError as e:
6 |         assert x == 0, e
7 |         assert e.args
  |         ^^^^^^^^^^^^^ PT017
  |

Found 2 errors.
```

Flake8:
```console
$ flake8 --version
7.0.0 (flake8-pytest-style: 2.0.0, flake8-pytest: 1.4, mccabe: 0.7.0, pycodestyle: 2.11.1, pyflakes: 3.2.0) CPython 3.11.8 on Darwin
$ flake8 --isolated --select=PT017 --show-source test-pt017.py
test-pt017.py:7:9: PT017 found assertion on exception e in except block, use pytest.raises() instead
        assert e.args
        ^
```

---

_Label `bug` added by @MichaReiser on 2024-06-14 13:20_

---

_Label `rule` added by @MichaReiser on 2024-06-14 13:20_

---

_Comment by @MichaReiser on 2024-06-14 13:31_

Yeah I think this is a bug. 

---

_Comment by @AlexWaygood on 2024-06-14 14:00_

Hmm, I think the snippet here could be written more idiomatically and more correctly as:

```py
def test_foo():
    x = 0
    with pytest.raises(ZeroDivisionError) as exc_info:
        1 / x
    assert x == 0, exc_info.value
    assert exc_info.args
```

I'm not sure I fully understand why we should only emit one error for this snippet rather than two. Both of the assertions here are assertions inside `except` blocks, which is what the rule is trying to catch.

---

_Comment by @MichaReiser on 2024-06-14 14:09_

To me the difference is that only the `test` expression is the assertion. That's what we assert by. The message is, just a message.

---

_Comment by @AlexWaygood on 2024-06-14 14:40_

But I think you still have the footgun where if no exception is actually raised in the `try` block, the test will pass silently (and probably unintentionally). `pytest.raises()` is designed to solve that, as in the vast majority of cases that's not what you want.

---

_Comment by @zanieb on 2024-06-14 14:42_

I'm confused by the rule documentation vs the message. In the documentation, we say we're catching all assertions in except blocks but in the message we say we're catching an assertion _on_ the exception. The latter also matches the rule name. I think I need more context on why we only flag assertions that include the exception before I can say who I agree with :)

---

_Comment by @MichaReiser on 2024-06-14 15:20_

> I'm confused by the rule documentation vs the message. In the documentation, we say we're catching all assertions in except blocks but in the message we say we're catching an assertion on the exception. The latter also matches the rule name. I think I need more context on why we only flag assertions that include the exception before I can say who I agree with :)

Yeah, I considered just updating the message but the problem is that the lint rule only catches `assert`s on the specific exception. If there's no no name referencing the exception by name (It's a bit wild because it doesn't correctly handle scopes. It just searches for any `Name` that has the same identifier 🙄 ), then the violation isn't triggered


That's where I think the rule title and documentation says one thing, but the implementation actually does something entirely else. In the context of the implementation, I would not expect the rule to raise a violation. 

---

_Comment by @AlexWaygood on 2024-06-14 15:30_

The only [rationale given](https://github.com/m-burst/flake8-pytest-style/blob/v2.0.0/docs/rules/PT017.md#rationale) for this rule in the docs for the original `flake8-pytest` plugin is:

> to avoid the situations when the test incorrectly passes because exception was not raised

That feels like it applies equally well to the two assertions in the example snippet.

> That's where I think the rule title and documentation says one thing, but the implementation actually does something entirely else. In the context of the implementation, I would not expect the rule to raise a violation.

I see, but I'm not really sure why a user would want the current behaviour. I don't feel like assertions on exceptions themselves in `except` blocks are any different to other kinds of exceptions in `except` blocks. They're both subject to the same footgun.

In my opinion, the better fix would be to revise the rule so that it highlights more `try`/`except` statements where there are assertions in the `except` block but no assertions in the `try` block, as that's what I'd expect from this rule's documentation, and it would make much more sense to me.

---

_Comment by @MichaReiser on 2024-06-14 15:41_

> I see, but I'm not really sure why you'd want the current behaviour. I don't feel like assertions on exceptions themselves in except blocks are any different to other kinds of exceptions in except blocks. They're both subject to the same footgun.

I'm not speaking in favor of the current implementation. I just tried to explain that the documentation and implementation aren't matching. Or at least, I would understand a different behavior from the rule documentation than is implemented today.  

---

_Label `bug` removed by @MichaReiser on 2024-06-14 15:41_

---

_Comment by @zanieb on 2024-06-15 01:08_

cc @harupy who implemented the rule.

---

_Comment by @harupy on 2024-06-15 02:41_

I agree. The documentation doesn't match the implementation. I would expect the following code to violate PT017, but it doesn't.

```python
def test_abc():
    try:
        foo()
    except Exception:
        bar()
```

---
