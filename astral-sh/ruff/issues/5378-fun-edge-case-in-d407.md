```yaml
number: 5378
title: Fun edge case in D407
type: issue
state: closed
author: john-science
labels:
  - question
  - docstring
assignees: []
created_at: 2023-06-26T20:42:30Z
updated_at: 2023-06-27T16:50:23Z
url: https://github.com/astral-sh/ruff/issues/5378
synced_at: 2026-01-10T11:09:47Z
```

# Fun edge case in D407

---

_Issue opened by @john-science on 2023-06-26 20:42_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Let's say I have a function in my code with a standard NumPy docstring:

```python
def something_amazing(inputs):
    """
    Wow, this does stuff!
    
    Parameters
    ==========
    inputs: dict
        Oh boy, oh boy
    """
    return sum(inputs.keys())
```

This will return the ruff error:

```bash
λ ruff .
awesome_sauce.py:6:9: D407 [*] Missing dashed underline after section ("Parameters")
Found 1 error.
```

But when I do a `ruff --fix .`, the code now looks like this:

```python
def something_amazing(inputs):
    """
    Wow, this does stuff!
    
    Parameters
    ----------
    ==========
    inputs: dict
        Oh boy, oh boy
    """
    return sum(inputs.keys())
```

This happened hundreds of times in our codebases, because someone thought dashes equal signs were the same.  

Now, I'm not sure if you want to support finding and fixing this problem. But in an ideal world I wouldn't have to worry about `ruff --fix` breaking things.

So, just FYI.

---

_Comment by @charliermarsh on 2023-06-26 20:49_

Haha. When an issue has "Fun" in the title...

Do you think D407 should detect and replace the equals signs?


---

_Label `question` added by @charliermarsh on 2023-06-26 20:49_

---

_Label `docstring` added by @charliermarsh on 2023-06-26 20:49_

---

_Comment by @zanieb on 2023-06-26 20:55_

If they're of a matching length it seems like a good user experience to remove the equal signs automatically.

---

_Assigned to @dhruvmanila by @dhruvmanila on 2023-06-27 05:17_

---

_Comment by @john-science on 2023-06-27 14:42_

> Do you think D407 should detect and replace the equals signs?

It would have helped my team.  Luckily, someone didn't trust the auto `--fix` flag and reviewed my PR.

BUT, I'm willing to concede I don't have a wide view of the huge feature set that `ruff` wants to support.  So it's up to ya'll.

I could take a whack at it, of course. But I haven't tried to contribute to `ruff` before, so there would be a long spin-up process.

---

_Comment by @charliermarsh on 2023-06-27 15:15_

All good, @dhruvmanila already finished it :)

---

_Closed by @charliermarsh on 2023-06-27 16:50_

---
