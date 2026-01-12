```yaml
number: 6840
title: "Idea for new Rule: Check pytest.raises Exception Info for broad Exceptions"
type: issue
state: open
author: Cielquan
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-08-24T06:32:14Z
updated_at: 2024-08-20T19:10:01Z
url: https://github.com/astral-sh/ruff/issues/6840
synced_at: 2026-01-12T15:54:46Z
```

# Idea for new Rule: Check pytest.raises Exception Info for broad Exceptions

---

_@Cielquan_

### Where we are

There is the rule (PT011)[https://beta.ruff.rs/docs/rules/pytest-raises-too-broad/] which checks for `pytest.raises()` calls with a set of Exceptiones that are deemed as too broad and complains, when the `pytest.raises()` call is missing the `match` parameter.

Example from PT011:
```py
import pytest


def test_foo():
    # Bad because too broad Exception with no exception info checking
    with pytest.raises(ValueError):
        ...

    # Good according to PT011
    with pytest.raises(ValueError, match="expected message"):
        ...
```

### Where is the problem

The problem with the `match` parameter is, that it is a regex under the hood, which is not that obvious. Or if you know it is, it is error prone. For example the Example above would also match Exception texts like `Not the expected message` which can be bad if the tested function raises different similar exceptions. One solution could be for this specific to put the `^` character at the beginning and the `$` character at the end.

Anthony has a video going more in depth on this https://www.youtube.com/watch?v=-1LXV_laNMs

### Alternative solution(s)

Because of the error prone nature of the `match` parameter I tend to save the exception in a variable and check the Exception info with a string comparison myself.
```py
import pytest


def test_foo():
    with pytest.raises(ValueError) as exc:
        # Checks if the message is exactly like the given string.
        assert "expected message" == str(exc.value)
```

Alternatively I also check with the `in` keyword, which in the PT011 case above has the same result, but is more obvious in what it does if you don't know that `match` is a regex.
```py
import pytest


def test_foo():
    with pytest.raises(ValueError) as exc:
        # Checks if the message contains the given string.
        assert "expected message" in str(exc.value)
```

### New Rule request

I would like to propose a new rule, which checks if there is a broad exception, just like PT011 does, but instead complains, when the exception info is not checked.
This would need to consider
- that the `exc` variable can have any name
- that the comparison can be `==` or `in`
- direct comparison like `assert "expected message" == str(exc.value)`
- assignments before comparison:
```py
msg, = exc.value.args
# or
str(exc.value)

assert "expected message" == msg
# or
assert "expected message" in msg
```

Perhaps also a rule to bann the `match` parameter would be nice, but I guess this would be too opinionated.

---

_Label `rule` added by @charliermarsh on 2023-08-25 21:44_

---

_Label `needs-decision` added by @charliermarsh on 2023-08-25 21:44_

---

_Comment by @Avasam on 2024-08-12 15:17_

https://github.com/astral-sh/ruff/issues/5157#issuecomment-2284256186

> imo as long as the exception info variable is used in a following assert statement, it's plenty enough to avoid false-positives whilst minimizing false-negatives.
> 
> Using an assert instead of the match param is often desired for exact string matches and to avoid having to escape special regex characters.
> 
> This is much more precise
> 
> ```python
> with pytest.raises(ValueError) as excinfo:
>     function_that_raises()
> assert "The complete error" == str(excinfo.value)
>     
> with pytest.raises(ValueError) as excinfo:
>     function_that_raises()
> assert "..." in str(excinfo.value)
> ```
> 
> Than this:
> 
> ```python
> with pytest.raises(ValueError, match="The complete error") as excinfo: # oops, this is a partial match !
>         function_that_raises()
>     
> with pytest.raises(ValueError, match="...") as excinfo: # oops, this matches any 3+ character strings !
>         function_that_raises()
> ```

A separate rule to avoid using `match=` with a non `re.escape` str could be nice (only disallow string literals, until Ruff gains multifile type information, pre-espaced strings can't be detected obviously)

---
