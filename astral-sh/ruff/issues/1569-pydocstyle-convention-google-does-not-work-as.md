```yaml
number: 1569
title: "Pydocstyle `convention = \"google\"` does not work as expected"
type: issue
state: closed
author: stinodego
labels:
  - docstring
assignees: []
created_at: 2023-01-02T22:48:54Z
updated_at: 2023-07-15T09:04:15Z
url: https://github.com/astral-sh/ruff/issues/1569
synced_at: 2026-01-10T11:09:43Z
```

# Pydocstyle `convention = "google"` does not work as expected

---

_Issue opened by @stinodego on 2023-01-02 22:48_

Minimal reproduction:

#### pyproject.toml
```toml
[tool.ruff]
select = ["D"]

[tool.ruff.pydocstyle]
convention = "google"
```

#### testruff.py
```python
def some_function(n):
    """Return some result.

    Args:
        n: Number.
    """
```

Running `ruff testruff.py` gives:

```bash
testruff.py:1:1: D100 Missing docstring in public module
testruff.py:2:5: D213 Multi-line docstring summary should start at the second line
testruff.py:2:5: D407 Missing dashed underline after section ("Args")
Found 3 error(s).
1 potentially fixable with the --fix option.
```

However, as specified in [pydocstyle docs](http://www.pydocstyle.org/en/stable/error_codes.html#default-conventions), D213 and D407 should not be checked for the Google docstring convention.

---

_Comment by @charliermarsh on 2023-01-02 22:52_

Yeah, this is known but not good. It's documented [here](https://github.com/charliermarsh/ruff#does-ruff-support-numpy--or-google-style-docstrings). In short, we only use `convention` to help with some of the checks themselves, and not to configure which checks are enabled. For that, you _have_ to go through `select` and `ignore`. (There are example, proper configurations present in the linked FAQ.)

I'll consider making `convention` automatically add the relevant checks to `select`. I wanted to keep `select` "pure" in this way but it might be too confusing as-is.

---

_Comment by @charliermarsh on 2023-01-02 22:53_

I'll play around with this now, maybe it'll turn out clean.

---

_Label `configuration` added by @charliermarsh on 2023-01-02 22:53_

---

_Label `docstring` added by @charliermarsh on 2023-01-02 22:53_

---

_Label `configuration` removed by @charliermarsh on 2023-01-02 22:53_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-02 22:55_

---

_Comment by @stinodego on 2023-01-02 23:02_

Apologies, I missed that section of the README. Thanks for pointing me to it. Maybe just adding a disclaimer [here](https://github.com/charliermarsh/ruff#pydocstyle) would already help.

Feel free to close this if you prefer to keep the current behaviour.

---

_Comment by @charliermarsh on 2023-01-02 23:04_

The current setup is confusing enough that I'm slightly embarrassed by it, which is a sign that I should probably fix it 😅 

I'll use this issue to track the work, if that's okay.

---

_Closed by @charliermarsh on 2023-01-03 00:08_

---

_Comment by @stinodego on 2023-01-03 07:32_

@charliermarsh Thanks for the quick solution here! I have one comment on the approach you implemented:

Could it be more intuitive if the `convention = "google"` actually populates the `ignore` list rather than the `select` list? Before your change, my settings looked like this (leaving out unrelated lints):

```toml
[tool.ruff]
select = [
    "D",  # pydocstyle
]
ignore = [
    # Google docstring convention
    "D203", "D204", "D213", "D215",
    "D400", "D404", "D406", "D407", "D408", "D409", "D413",
    # Docstrings not required
    "D1"
]

[tool.ruff.pydocstyle]
convention = "google"
```

My thinking was that by adding `select = ["D"]`, I 'enable' pydocstyle. And then by specifying the convention, I configure it to behave the way I want.

Now my settings look like this:

```toml
[tool.ruff]
select = []
ignore = [
    # Docstrings not required
    "D1"
]

[tool.ruff.pydocstyle]
convention = "google"
```

This feels weird, because it is not clear that I ever 'enabled' pydocstyle, but I am ignoring error codes for it and configuring it. By making the configuration populate the ignore list, I could have a settings file that makes sense to my way of thinking:

```toml
[tool.ruff]
select = [
    "D",  # pydocstyle
]
ignore = [
    # Docstrings not required
    "D1"
]

[tool.ruff.pydocstyle]
convention = "google"
```

This approach would also make sense with the way the conventions are [formulated](http://www.pydocstyle.org/en/stable/error_codes.html#default-conventions): "All lints except ...". And it is in line with how people are used to working with flake8 extensions: you add an extension (`pip install flake8-docstrings`), then you configure it (`convention = "google"`).

Curious to hear your thoughts about this! Either way, very happy with the existing solution and `ruff` as a new tool in my toolbelt!

---

_Comment by @stinodego on 2023-01-03 07:50_

And another note: the VSCode extension now no longer underlines docstrings, even if they would raise an error code.

---

_Comment by @charliermarsh on 2023-01-03 12:24_

Yeah I think what you're describing makes sense. I can make that change.

---

_Comment by @charliermarsh on 2023-01-03 12:25_

> And another note: the VSCode extension now no longer underlines docstrings, even if they would raise an error code.

Is it possible that you're using the bundled version of the extension, which hasn't yet been updated?

---

_Comment by @stinodego on 2023-01-03 12:25_

> > And another note: the VSCode extension now no longer underlines docstrings, even if they would raise an error code.
> 
> Is it possible that you're using the bundled version of the extension, which hasn't yet been updated?

Oh yeah that's probably it! 

---

_Comment by @charliermarsh on 2023-01-03 12:31_

I'll cut a new pre-release in a bit :)

You can also point the extension to an interpreter or hard-code the path to the Ruff binary in the extension settings.

---

_Comment by @charliermarsh on 2023-01-03 22:09_

I think what you're describing above makes sense, RE adding to the `ignore` list instead. It's kind of like saying that `convention = "google"` makes Ruff operate as if none of the non-Google-related checks exist? And it's a little more explicit which I prefer.

---

_Comment by @zundertj on 2023-07-15 09:04_

Hello, I understand that this change may be seen as an improvement over the previous behavior, but the current behavior is still lacking imo. Starting with this

```toml
[tool.ruff]
select = [
    "D",  # pydocstyle
]
ignore = [
    # Docstrings not required
    "D1"
]

[tool.ruff.pydocstyle]
convention = "google"
```

It will turn off all rules not part of the Google convention, *including* rules that are still compatible. How could one get all rules except the ones in conflict with the convention, i.e. to maximize the number of checks whilst being in line with the convention? It forces us to abandon the convention config altogether, as it leaves no way for us to turn on compatible-but-not-included rules. I like the explicitness of setting "convention = X" rather than a bunch of `D` codes, but it seems that can only be had if you are not interested in rules in addition to the convention at the moment.

---
