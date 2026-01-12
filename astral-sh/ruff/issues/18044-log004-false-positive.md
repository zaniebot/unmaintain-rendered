```yaml
number: 18044
title: LOG004 false positive
type: issue
state: closed
author: sk-
labels:
  - question
assignees: []
created_at: 2025-05-12T14:11:36Z
updated_at: 2025-09-16T16:25:41Z
url: https://github.com/astral-sh/ruff/issues/18044
synced_at: 2026-01-12T15:54:56Z
```

# LOG004 false positive

---

_@sk-_

### Summary

The preview rule LOG004 ([log-exception-outside-except-handler](https://docs.astral.sh/ruff/rules/log-exception-outside-except-handler/)) is producing a false positive when explicitly passing an `exc_info` argument.

The code below ([playground link](https://play.ruff.rs/a03680fc-069a-4dbf-80f0-a79e30970605)) reproduces the issue:

```python
import logging

logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)
logger.exception("Test LOG004", exc_info=IndexError("test message"))
```

producing the output 

```
test_logging.py:5:1: LOG004 `.exception()` call outside exception handlers
  |
3 | logging.basicConfig(level=logging.INFO)
4 | logger = logging.getLogger(__name__)
5 | logger.exception("Test LOG004", exc_info=IndexError("test message"))
  | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ LOG004
  |
  = help: Replace with `.error()`

Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

However,  this code is perfectly valid and it outputs

```
ERROR:__main__:Test LOG004
IndexError: test message
```


Note that the [justification](https://docs.astral.sh/ruff/rules/log-exception-outside-except-handler/) for the rule states:

> Calling .exception() outside of an exception handler attaches None as exception information, leading to confusing messages:

However in this case, the exception information is indeed attached because it was passed explicitly.

I could see why someone may prefer to just use `error` and avoid using `exception` in this case, but that IMO would be a stylistical rule similar to [LOG007](https://docs.astral.sh/ruff/rules/exception-without-exc-info/). Such rule could get some good reception as others [have manifested](https://github.com/astral-sh/ruff/issues/4136#issuecomment-2586709067) that they never use `logger.exception`.

### Version

0.11.9

---

_Comment by @ntBre on 2025-05-12 21:23_

I don't have a strong opinion either way here, but I think there's a good argument that this is not a false positive. The [`logging.exception` docs](https://docs.python.org/3/library/logging.html#logging.exception) say:

> This function should only be called from an exception handler.

without any qualification, which seems to be what this lint rule enforces. I think this is also in line with the [upstream linter](https://github.com/adamchainz/flake8-logging/blob/3ae3b15643fcdc822cff11d34ba9fe5d6d2ea3cd/src/flake8_logging/__init__.py#L283-L287), although I haven't tested to be sure.

If we decide that this is a true positive, we could at least update the rule documentation to align more closely with the Python docs.

---

_Label `question` added by @ntBre on 2025-05-12 21:24_

---

_Comment by @sk- on 2025-05-12 23:31_

I agree with you assessment and sorry for missing the `logging.exception` docs, I was looking at the docstrings, which are different to what is published on the docs.

It'd be great if the rule's [documentation](https://docs.astral.sh/ruff/rules/log-exception-outside-except-handler/) could be changed so that it is clear that the rules enforces what the [logging docs](https://docs.python.org/3/library/logging.html#logging.exception) suggest. 


In any case, jut like others we opted to get rid of all calls to `.exception` in favor of calls to `.error`.

---

_Closed by @sk- on 2025-05-12 23:31_

---

_Comment by @spaceone on 2025-09-16 15:34_

I think the rule docs should suggest to use `error` method, if `exception(exc_info=…)` is used.

---

_Comment by @ntBre on 2025-09-16 15:57_

@spaceone I don't think I'm following your suggestion. Both the error message shown in this issue and the rule docs already recommend using `logging.error`.

---

_Comment by @spaceone on 2025-09-16 16:25_

Well yes, https://docs.astral.sh/ruff/rules/log-exception-outside-except-handler/ says to replace `exception()` to `error()`.
I was only irritated first because:
`log.exception("foo", exc_info=exc_info)` is valid in a non-exception context. but should of course also be migrated to just use error().

---
