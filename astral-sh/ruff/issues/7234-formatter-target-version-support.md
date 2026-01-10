```yaml
number: 7234
title: "Formatter: Target version support"
type: issue
state: closed
author: MichaReiser
labels:
  - formatter
assignees: []
created_at: 2023-09-08T07:54:35Z
updated_at: 2023-10-11T13:32:55Z
url: https://github.com/astral-sh/ruff/issues/7234
synced_at: 2026-01-10T11:09:49Z
```

# Formatter: Target version support

---

_Issue opened by @MichaReiser on 2023-09-08 07:54_

Adjust the formatting based on the syntax supported by the targeted python versions. 

> Black uses this option to decide what grammar to use to parse your code. In addition, it may use it to decide what style to use. For example, support for a trailing comma after *args in a function call was added in Python 3.5, so Black will add this comma only if the target versions are all Python 3.5 or higher.

[Source](https://black.readthedocs.io/en/stable/usage_and_configuration/the_basics.html#t-target-version)


* [ ] Decide on minimal supported python version
* [ ] Trailing args and kwargs comments (depending on the above decision)
* [ ] Parenthesized with items 
---

Relevant features:
 * 3.10: [Parenthesized with items](https://docs.python.org/3/whatsnew/3.10.html#parenthesized-context-managers)
 * 3.12: [f-string grammar](https://peps.python.org/pep-0701/)

---

_Label `formatter` added by @MichaReiser on 2023-09-08 07:54_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-09-08 07:54_

---

_Comment by @konstin on 2023-09-12 07:46_

Do we have a list of syntax that would change formatter behaviour that is not in 3.7/3.8? I can only think of nested f-string quotes in 3.12

---

_Comment by @MichaReiser on 2023-09-12 07:52_

> Do we have a list of syntax that would change formatter behaviour that is not in 3.7/3.8? I can only think of nested f-string quotes in 3.12

f-strings will be fun because they now can contain comments too, yeah. 

I don't have more than what's outlined in the issue above and is derived from Black's documentation. We could do a quick search on Black's repository to identify other version-specific formatting (or look at their version-specific tests)

---

_Comment by @charliermarsh on 2023-09-12 11:52_

Parenthesized with items (mentioned above) -- need to make sure we don't introduce parentheses there if unsupported. I don't know if this needs changes or not, but it was new in Python 3.10. (They have `crates/ruff_python_formatter/resources/test/fixtures/black/py_39/remove_with_brackets.py` for this, I think?)

See also `crates/ruff_python_formatter/resources/test/fixtures/black/py_39/pep_572_py39.py` -- walrus operators can be unparenthesized in some cases starting in Python 3.9, so we need to make sure we don't _remove_ parentheses in such cases on older versions.


---

_Comment by @MichaReiser on 2023-09-13 09:22_

> For my projects, the quote normalization would be OK, as this should be already fixed by the Q rules in the linter. However, the prefix normalization would be very problematic, as it breaks backwards compatibility with some older versions of Pythons that we still need to support for a couple of our projects. Specifically, the issue is the change from br"..." to rb"..." for binary regular expressions, which causes a SyntaxError. Without some type of replacement for this --skip-string-normalization option, we would not be able to switch to Ruff for formatting.

Originally [answer](https://github.com/astral-sh/ruff/discussions/7305#discussioncomment-6987835) by @tdulcet 

> Not versions that are supported by Ruff. Specifically, Python 3.1 and older, including 2.7 which we still need to support unfortunately... [[source](Not versions that are supported by Ruff. Specifically, Python 3.1 and older, including 2.7 which we still need to support unfortunately...)]

---

_Comment by @charliermarsh on 2023-09-14 22:56_

I've confirmed that the unparenthesized walruses is handled correctly (we never _remove_ those parentheses, so we're fine).

---

_Comment by @charliermarsh on 2023-09-14 23:00_

We also get the `remove_with_brackets.py` test case right. I've confirmed that we only add parentheses in a `with` if there were already parentheses to start. (Can see `should_parenthesize` in `stmt_with.rs`; the other case is that in which there's a parenthesized dangling comment, which again implies parentheses.)

---

_Comment by @charliermarsh on 2023-09-14 23:16_

As far as I can tell, there's nothing that we need to do here, assuming...

1. We only want to support Python 3.7 and later.
2. We don't care about _downgrading_ code (e.g., if code was written for Python 3.10, and uses a parenthesized `with`, then when run under `--target-version py37`, we consider it validate to _retain_ the parenthesized `with`).

Some notes on Black's behavior:

- I believe that Black adheres to (2) -- so if you run over a file with a `match` statement using `--target-version py37`, Black will fail. Black uses the target version to determine which _parser_ to use.
- Black has a `VERSION_TO_FEATURES` concept whereby it detects features that are used in a given file, and uses _that_ to _infer_ the target version, if it's not set (see: `detect_target_versions`).
- After inferring the target versions, the only version-specific features that affect formatting are `Feature.TRAILING_COMMA_IN_CALL` and `Feature.TRAILING_COMMA_IN_DEF` (which apply for Python 3.5 and later, i.e., the versions we support), and `Feature.PARENTHESIZED_CONTEXT_MANAGERS`.

This last piece (`Feature.PARENTHESIZED_CONTEXT_MANAGERS`) is interesting, but is only relevant for preview style: https://black.readthedocs.io/en/stable/the_black_code_style/future_style.html#using-backslashes-for-with-statements. In short, Black is going to wrap multiple `with` statements using backslashes on older versions or parentheses on newer versions. Right now, it seems like the parenthesized version is implemented in preview mode. So, once we do preview style, we'll need to support that _and_ gate it on target version.

For example, given:

```python
with a as b, c as d, e(1,) as f:
    pass
```

Black stable formats as:

```python
with a as b, c as d, e(
    1,
) as f:
    pass
```

But preview gives you:

```python
with (
    a as b,
    c as d,
    e(
        1,
    ) as f,
):
    pass
```

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-14 23:20_

---

_Comment by @MichaReiser on 2023-09-15 13:02_

> (Can see should_parenthesize in stmt_with.rs; the other case is that in which there's a parenthesized dangling comment, which again implies parentheses.)

This may no longer be true if we implement https://github.com/astral-sh/ruff/issues/7243

> We don't care about downgrading code (e.g., if code was written for Python 3.10, and uses a parenthesized with, then when run under --target-version py37, we consider it validate to retain the parenthesized with).

I'm unsure whether I would expect my formatter to downgrade the syntax. I guess mainly because I'm just not used to having this setting in a formatter. Removing parentheses raises interesting question when it comes to comments, especially leading and trailing commnts

---

_Comment by @MichaReiser on 2023-09-27 13:29_

What's the status on this? Do we plan to support this for the Beta? 

---

_Comment by @charliermarsh on 2023-09-27 13:34_

My assessment is that there's nothing to do here until we support preview style.

---

_Removed from milestone `Formatter: Beta` by @MichaReiser on 2023-09-27 13:37_

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-09-27 13:37_

---

_Closed by @charliermarsh on 2023-10-11 13:32_

---
