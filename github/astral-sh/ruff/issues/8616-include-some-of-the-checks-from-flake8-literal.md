---
number: 8616
title: Include some of the checks from flake8-literal
type: issue
state: open
author: ThiefMaster
labels:
  - rule
  - wish
assignees: []
created_at: 2023-11-11T13:45:52Z
updated_at: 2023-11-14T08:57:29Z
url: https://github.com/astral-sh/ruff/issues/8616
synced_at: 2026-01-07T13:12:15-06:00
---

# Include some of the checks from flake8-literal

---

_Issue opened by @ThiefMaster on 2023-11-11 13:45_

This plugin is quite similar than `flake8-quotes`, but it contains some nice checks such as warning about unnecessary quote escapes, e.g. `'foo\"bar'` (which, in fact, are created by ruff's fixer - I've opened a separate issue about this: #8617).

`.flake8` config:

```ini
[flake8]
inline-quotes = single
multiline-quotes = single
docstring-quotes = double
avoid-escape = true

literal-inline-quotes = single
literal-multiline-quotes = single
literal-docstring-quotes = double
literal-avoid-escape = true
```

`ruff.toml` config

```toml
select = ['F', 'E', 'W', 'Q', 'RUF', 'SIM']
preview = true

[flake8-quotes]
inline-quotes = 'single'
multiline-quotes = 'single'
docstring-quotes = 'double'
avoid-escape = true
```

Python code:

```python
a = 'hello'
b = "world"

c = '''foo'''
d = """bar"""

e = 'check \'this\''
f = "check 'that'"
g = 'check \'this\' "and that" \'and this\''
h = "check \"this\" 'and that' \"and this\""

i = "foo\'bar"
j = 'foo\"bar'


def foo():
    '''doc'''


def bar():
    """doc"""
```

flake8 output:

```
[adrian@claptrap:/tmp/rufftest]> flake8 test.py
test.py:2:5: Q000 Double quotes found but single quotes preferred
test.py:2:5: LIT001 Use single quotes for string

test.py:5:5: Q001 Double quote multiline found but single quotes preferred
test.py:5:5: LIT003 Use single quotes for multiline string

test.py:7:5: Q003 Change outer quotes to avoid escaping inner quotes
test.py:7:5: LIT011 Use double quotes for string to avoid escaped single quote

test.py:10:5: LIT001 Use single quotes for string

test.py:12:5: LIT013 Escaped single quote is not necessary

test.py:13:5: LIT014 Escaped double quote is not necessary

test.py:17:5: Q002 Single quote docstring found but double quotes preferred
test.py:17:5: LIT006 Use double quotes for docstring
```

ruff output (built from latest master):

```
[adrian@claptrap:/tmp/rufftest]> ~/dev/ruff/target/release/ruff check --no-cache /tmp/rufftest/test.py
test.py:2:5: Q000 [*] Double quotes found but single quotes preferred
test.py:5:5: Q001 [*] Double quote multiline found but single quotes preferred
test.py:7:5: Q003 [*] Change outer quotes to avoid escaping inner quotes
test.py:17:5: Q002 [*] Single quote docstring found but double quotes preferred
Found 4 errors.
[*] 4 fixable with the `--fix` option.
```

In particular it would be nice if anything related to avoid escaping would actually pick the quote style to ensure the least amount of escapes (in case the string contains both types of quotes). Maybe even consider switching to a triple-quoted string, but maybe that's a bit more controversial and should be configurable...

---

_Comment by @charliermarsh on 2023-11-11 14:56_

My first impression is that some of these feel redundant with existing rules, e.g., "Use double quotes for docstring" exists in the `D` rule set. The one I could imagine including is the unnecessary escape rule. We have `W605` which checks for _invalid_ escapes, but not _unnecessary_ escapes.

I do consider this low priority since the formatter already performs this normalization and removes unnecessary escapes, but I'd accept a contribution for such a rule in the `Q set.

---

_Label `rule` added by @charliermarsh on 2023-11-11 14:56_

---

_Label `wish` added by @charliermarsh on 2023-11-11 14:56_

---

_Comment by @charliermarsh on 2023-11-14 04:36_

@ThiefMaster - Anything else you want to include here, or is the string-escaping sufficient to close?

---

_Comment by @ThiefMaster on 2023-11-14 08:56_

That was the main one I was interested in. These ones at least sound interesting:

- LIT015 - Use double quotes for continuation strings to match
- LIT016 - Use single quotes for continuation strings to match

This code currently triggers a warning in flake8-quotes but not in ruff (with single-quote preference):

```python
foo = (
    "i'm"
    "really"  # <-- does not need double quotes
    'cool'
)
```

But probably this could be a setting that affects the existing Q000 rule (whether consistency for continuation strings should be preferred, avoided or ignored).

---

- LIT101 - Remove raw prefix when not using escapes
- LIT102 - Use raw prefix to avoid escaped slash

There are no rules for this yet. Could be a nice one to add.

---

- LIT103 - Use raw prefix for re pattern

There is no rule for this one yet. It's pretty common to always use raw strings for regexps regardless whether there are any escapes in it or not (but it's particularly useful when there are escapes!), so at least for those passed directly as literals to the various functions from the `re` module a warning could be shown (probably not worth trying to track variables when the regex string is first assigned to a variable and that variable gets passed to one of these functions, unless there's already functionality for such logic making it simple to do as well).

---

_Referenced in [astral-sh/ruff#11525](../../astral-sh/ruff/issues/11525.md) on 2024-06-01 17:02_

---
