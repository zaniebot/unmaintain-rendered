```yaml
number: 1518
title: "D400: false positive and bad autofix"
type: issue
state: closed
author: buster-blue
labels:
  - docstring
assignees: []
created_at: 2022-12-31T21:18:26Z
updated_at: 2022-12-31T23:38:23Z
url: https://github.com/astral-sh/ruff/issues/1518
synced_at: 2026-01-10T12:05:30Z
```

# D400: false positive and bad autofix

---

_Issue opened by @buster-blue on 2022-12-31 21:18_

I'm using ruff version 0.0.205

The following code triggers D400 with the message "First line should end with a period":
```python
def list_contains_round(rounds: list[int], number: int) -> bool:
    """
    :param rounds: list - rounds played.
    :param number: int - round number.
    :return:  bool - was the round played?
    """
    return number in rounds
```
The first line already ends with a period, so this is a false positive. Additionally, the autofix that it applies is bad:
```python
def list_contains_round(rounds: list[int], number: int) -> bool:
    """
    :param rounds: list - rounds played.
    :param number: int - round number.
    :return:  bool - was the round played?.
    """
    return number in rounds
```
it adds a period after the question mark on the third line. Ruff must think that the third line is actually the first line, but even if that was the case, the autofix shouldn't add the period after a question mark. I'm not sure what the right autofix to have instead would be though.

---

_Comment by @buster-blue on 2022-12-31 21:26_

This smaller piece of code triggers the same bug for me. It might help with finding the problem.
```python
def lis():
    """
    played.
    played?
    """
    pass
```

---

_Label `bug` added by @charliermarsh on 2022-12-31 21:38_

---

_Label `docstring` added by @charliermarsh on 2022-12-31 21:38_

---

_Comment by @charliermarsh on 2022-12-31 21:39_

Thanks for reporting this. Looks like pydocstyle doesn't error here, so we shouldn't either. Will fix, should be easy.

---

_Comment by @charliermarsh on 2022-12-31 21:43_

Looking back at the code... Now I'm not so sure. First-line punctuation is really intended for summary lines. Here's an example from [PEP 257]():

```py
def complex(real=0.0, imag=0.0):
    """Form a complex number.

    Keyword arguments:
    real -- the real part (default 0.0)
    imag -- the imaginary part (default 0.0)
    """
    if imag == 0.0 and real == 0.0:
        return complex_zero
    ...
```

Notice how, if we respected the rule in your case (at the end of individual parameter definitions), we'd then get _this_ case wrong, which would be equally "valid":

```py
def list_contains_round(rounds: bool, number: int) -> None:
    """
    :param rounds: bool - was the round played?
    :param number: int - round number.
    """
    return number in rounds
```


---

_Comment by @charliermarsh on 2022-12-31 21:45_

The logic as it exists today looks for the first logical line, i.e., it's sort of looking for the first line-break, and taking the lines that precede it as the "first line".

It helps get some other cases right, like:

```py
def list_contains_round(rounds: list[int], number: int) -> bool:
    """Here's a summary line,
    but it continues onto the next line.

    :param rounds: list - rounds played.
    :param number: int - round number.
    :return:  bool - was the round played?
    """
    return number in rounds
```

---

_Comment by @charliermarsh on 2022-12-31 21:46_

I could probably special-case to avoid enforcing this rule at all for lines that look like a section (`:param`), but I doubt I can enforce that the params end in a period.

---

_Comment by @buster-blue on 2022-12-31 21:58_

I know that the docstrings are nonstandard (they weren't written by me) but I figured that having an unusual docstring like one without a summary line still shouldn't cause ruff to autofix wrong and report an error that at least seems incorrect by the message that it gives.

If it's intended behavior (or undefined behavior or something like that) that ruff might not play nice with usual docstrings like this, then I can just close the issue and turn off D autofixes for myself in cases like this. It's not too big of a problem.

I don't know how cases like this are handled by other linters because I'm actually pretty new to linters.

---

_Comment by @buster-blue on 2022-12-31 22:04_

> Looking back at the code... Now I'm not so sure. First-line punctuation is really intended for summary lines. Here's an example from PEP 257:
> 
> ```python
> def complex(real=0.0, imag=0.0):
>     """Form a complex number.
> 
>     Keyword arguments:
>     real -- the real part (default 0.0)
>     imag -- the imaginary part (default 0.0)
>     """
>     if imag == 0.0 and real == 0.0:
>         return complex_zero
>     ...
> ```
> 
> Notice how, if we respected the rule in your case (at the end of individual parameter definitions), we'd then get _this_ case wrong, which would be equally "valid":
> 
> ```python
> def list_contains_round(rounds: bool, number: int) -> None:
>     """
>     :param rounds: bool - was the round played?
>     :param number: int - round number.
>     """
>     return number in rounds
> ```
Actually if I'm understanding you right, shouldn't that case not be valid? The lint says "First line should end with a period". A question mark isn't a period, so doesn't that mean that it's still wrong? If that's not what the lint is intended to be for, then maybe the message should be changed somehow.

---

_Comment by @buster-blue on 2022-12-31 22:14_

I think I understand now. I took the message of D400 literally. It says "First line should end with a period" so I thought that, regardless of what it is, the first line of a docstring (where there's first text), should end in a period. It sounds like what's it's actually meant to mean is "Summary line should end with a period", but it just checks for the first line because it assumes that they're the same. So if there's no summary line, then the assumption that first and summary line are the same doesn't work. Is that what's going on here?

---

_Comment by @charliermarsh on 2022-12-31 22:34_

> Actually if I'm understanding you right, shouldn't that case not be valid? The lint says "First line should end with a period". A question mark isn't a period, so doesn't that mean that it's still wrong? If that's not what the lint is intended to be for, then maybe the message should be changed somehow.

Yeah sorry -- I was making a confusing point here, which is that the period at the end of the _parameter_ description feels like a false-negative given the intention of the rule. I wasn't being very clear.

> I think I understand now. I took the message of D400 literally. It says "First line should end with a period" so I thought that, regardless of what it is, the first line of a docstring (where there's first text), should end in a period. It sounds like what's it's actually meant to mean is "Summary line should end with a period", but it just checks for the first line because it assumes that they're the same. So if there's no summary line, then the assumption that first and summary line are the same doesn't work. Is that what's going on here?

Yeah that's right. We could probably do a better job of detecting that the first line in your case _isn't_ a summary line.


---

_Comment by @charliermarsh on 2022-12-31 22:37_

I'll try to make a few improvements on that front. For example, we could ignore lines with `:param`, ignore sections like `Args:`, etc.

---

_Label `bug` removed by @charliermarsh on 2022-12-31 22:37_

---

_Assigned to @charliermarsh by @charliermarsh on 2022-12-31 23:09_

---

_Closed by @charliermarsh on 2022-12-31 23:38_

---
