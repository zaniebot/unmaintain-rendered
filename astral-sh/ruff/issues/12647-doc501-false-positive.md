```yaml
number: 12647
title: DOC501 False Positive
type: issue
state: closed
author: will-schneble
labels:
  - bug
  - docstring
assignees: []
created_at: 2024-08-02T21:44:28Z
updated_at: 2024-08-08T16:58:27Z
url: https://github.com/astral-sh/ruff/issues/12647
synced_at: 2026-01-12T15:54:52Z
```

# DOC501 False Positive

---

_@will-schneble_

**Describe the bug**
DOC501 rule violation when the docstring includes the exception.

ruff==0.5.5
python==3.12.4

Command: `ruff check --preview --select DOC`

No pyproject.toml or other configuration than what's shown here.

**Steps to reproduce**
Sample file to test against.
```python
class Foo:
    def __init__(self, bar, zap):
        self.bar = bar
        self.zap = zap
        
    def get_bar(self) -> str:
        """Print and return bar.
        
        Raises:
            ValueError: bar is not bar.

        Returns:
            str: bar value.
        """    
        print(self.bar)
        if self.bar != "bar":
            raise ValueError(self.bar)
        return self.bar
```

**Expected behavior**
No violations.

Running `pydoclint --style=google test.py` gives the expected no violation result.

**Debug logs**
```
test.py:17:19: DOC501 Raised exception `ValueError` missing from docstring
   |
15 |         print(self.bar)
16 |         if self.bar != "bar":
17 |             raise ValueError(self.bar)
   |                   ^^^^^^^^^^^^^^^^^^^^ DOC501
18 |         return self.bar
   |
```

**Notes**
Adding an argument to Foo.get_bar and adding that to the docstring then makes the DOC501 violation go away. Example below:
```python
    def get_bar(self, x: str) -> str:
        """Print and return bar.
        
        Args:
            x (str): A dummy value.
        
        Raises:
            ValueError: bar is not bar.

        Returns:
            str: bar value
        """    
        print(self.bar)
        if self.bar != "bar":
            raise ValueError(self.bar)
        return self.bar
```


---

_Comment by @will-schneble on 2024-08-02 21:47_

Just saw ruff==0.5.6 released. Double checked and it is still an issue in that version.

---

_Label `bug` added by @charliermarsh on 2024-08-02 21:51_

---

_Label `docstring` added by @charliermarsh on 2024-08-02 21:51_

---

_Comment by @charliermarsh on 2024-08-02 21:51_

Thank you!

---

_Comment by @charliermarsh on 2024-08-02 22:34_

I think the issue is that the above section is actually ambiguous. All the defined sections are valid section names in both NumPy and Google. You should set the convention explicitly in your `pyproject.toml`, e.g.:

```toml
[tool.ruff.lint.pydocstyle]
convention = "google"
```

(Adding `Args` works because `Args` is Google-only, so we can safely differentiate.)


---

_Comment by @will-schneble on 2024-08-02 22:39_

You're correct, adding `--config 'lint.pydocstyle.convention="google"'` produces the expected results. For some reason I thought pydocstyle config didn't affect the pydoclint rules. Thanks for looking into it.

---

_Comment by @JasonGrace2282 on 2024-08-02 22:44_

It might make sense to produce a more reasonable error message instead of a vague DOC501 linting error?

---

_Comment by @charliermarsh on 2024-08-02 22:50_

@JasonGrace2282 - can you expand on that? It’s inferring a NumPy docstring, and in that case, it can’t find any raises.

@will-schneble - gonna leave this open because I think we can improve our detection.

---

_Comment by @JasonGrace2282 on 2024-08-02 23:00_

I'm not familiar with how the parsing works in Ruff, but as a user I would expect a warning if the convention is numpy but a docstring looks like Google convention.
I understand what I just said is a bit vague (and possibly hard to implement), so really any kind of hint if the docstring convention isn't explicitly set would be useful imo.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-02 23:35_

---

_Closed by @charliermarsh on 2024-08-03 01:04_

---

_Closed by @charliermarsh on 2024-08-03 01:04_

---

_Comment by @trim21 on 2024-08-08 16:58_

this is confusing, doc use google style but it requires extra config

https://docs.astral.sh/ruff/rules/docstring-missing-exception/

---
