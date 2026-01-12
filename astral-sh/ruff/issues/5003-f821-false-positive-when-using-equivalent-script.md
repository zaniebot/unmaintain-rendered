```yaml
number: 5003
title: F821 False Positive when using equivalent script characters
type: issue
state: closed
author: TizzySaurus
labels:
  - bug
assignees: []
created_at: 2023-06-10T12:02:11Z
updated_at: 2024-03-29T23:48:43Z
url: https://github.com/astral-sh/ruff/issues/5003
synced_at: 2026-01-12T15:54:45Z
```

# F821 False Positive when using equivalent script characters

---

_@TizzySaurus_

Bug: Ruff incorrectly treats characters from other scripts as different, resulting in a F821 undefined name.
This is possibly related to #942.

```python
𝒞 = 500
print(𝒞)
print(C + 𝒞)  # ruff says `C` isn't defined
print(C / 𝒞)
print(C == 𝑪 == 𝒞 == 𝓒 == 𝕮)  # ruff says `C`, `𝑪`, ... isn't defined
```
The above code, albeit somewhat funky, is actually perfectly valid python code and runs successfully, due to how Python handles different script characters (it treats equivalent characters from different scripts as the same at runtime).

However, it appears that ruff (somewhat understandably) doesn't like this, and errors stating that `C`, etc. isn't defined.

Ruff should realise that these characters are equivalent at runtime, and not error stating that they're undefined.

---

_Comment by @charliermarsh on 2023-06-10 14:44_

Woah, whaaaaaat? I've never seen this.

I understand that these are the language semantics, but just to help illustrate the issue, do you have a realistic example of where this is used "in the wild"?

---

_Label `bug` added by @charliermarsh on 2023-06-10 14:44_

---

_Label `question` added by @charliermarsh on 2023-06-10 14:44_

---

_Comment by @dhruvmanila on 2023-06-10 15:02_

FWIW, `pyright` and `mypy` understands them correctly and another interesting note is that the error message specifies that "C" is undefined (instead of the actual character) once you remove the assignment statement:
```
mypy: Name "C" is not defined
Pyright: "C" is not defined [reportUndefinedVariable]
Ruff: Undefined name `𝒞` [F821]
``` 

---

_Label `question` removed by @charliermarsh on 2023-06-10 15:21_

---

_Comment by @TizzySaurus on 2023-06-11 13:11_

> do you have a realistic example of where this is used "in the wild"?
Not really 😅 

It's very much a "quirk" of the language that the vast majority of people aren't even aware of, and I'm not aware of a practical use case for it -- although it is quite a nice thing that Python supports multiple scripts, being (afaik) the only mainstream language to do so.

I came across this feature of Python years ago, and realised Ruff specifically doesn't like it when messing with some esoteric (deliberately unreadable and obfuscated) code yesterday.

Figured that if other linters both support this behaviour and correctly report which variable is undefined etc., then "why can't ruff?", hence the creation of this issue.


---

_Comment by @charliermarsh on 2023-06-12 00:25_

Makes sense. We _should_ support it, and I don't think it's _too_ hard to do so, but I also consider it low priority for now :)

---

_Closed by @AlexWaygood on 2024-03-18 11:56_

---

_Comment by @covracer on 2024-03-29 23:48_

In case anyone wants to read more about Python's NFKC normalization for characters above 0x7f: https://peps.python.org/pep-3131/

---
