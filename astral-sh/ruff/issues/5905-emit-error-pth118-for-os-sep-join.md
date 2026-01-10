```yaml
number: 5905
title: "Emit error PTH118 for `os.sep.join()`"
type: issue
state: closed
author: dosisod
labels:
  - rule
assignees: []
created_at: 2023-07-20T06:29:11Z
updated_at: 2023-07-21T01:36:04Z
url: https://github.com/astral-sh/ruff/issues/5905
synced_at: 2026-01-10T11:09:48Z
```

# Emit error PTH118 for `os.sep.join()`

---

_Issue opened by @dosisod on 2023-07-20 06:29_

The following shows 2 similar snippets of code, but only one is picked up by Ruff currently:

```python
import os

os.path.join("hello", "world")   # PTH118
os.sep.join(("hello", "world"))  # no error
```

`os.sep.join()` acts the same as `os.path.join()`, just written differently. Searching on [grep.app](https://grep.app/search?q=os.sep.join&filter[lang][0]=Python) shows that this is a somewhat common idiom.

Running:

```
$ ruff --version
ruff 0.0.278

$ ruff file.py
file.py:3:1: PTH118 `os.path.join()` should be replaced by `Path` with `/` operator
Found 1 error.
```

Expected:

An error for both `os.sep.join` and `os.path.join`.

---

_Comment by @sbrugman on 2023-07-20 08:45_

I can take this one (also working on some other pathlib ports from refurb)

Nice catch, and also interesting to learn about grep.app. I've been using the GitHub search functionality so far

---

_Label `rule` added by @charliermarsh on 2023-07-20 14:04_

---

_Comment by @dosisod on 2023-07-20 18:10_

I've found that GitHub search doesn't find *exactly* for what I'm looking for while grep.app does, in addition to providing case-insensitive and regex searching. It's also great for gauging how impactful a new rule might be, and finding potential edge-cases.

And thank you for taking the time to port parts of Refurb into Ruff! I've added a lot of checks since #1348 was opened, and I've been meaning to leave a comment with an updated list but haven't had the time. I'll find some time this weekend to update the list!

---

_Comment by @charliermarsh on 2023-07-21 00:23_

Any idea why folks are using `os.sep.join` in lieu of `os.path.join`? I just want to make sure there's not a subtle distinction that we're overlooking.

---

_Comment by @sbrugman on 2023-07-21 00:29_

`os.sep.join` is a string join on the `os.sep` character. `os.path.join` also handles some path logic. `os.sep.join` may be faster ([so](https://stackoverflow.com/questions/48701235/why-os-sep-is-faster-than-os-path-join))

For the Pathlib rules this shouldn't matter. The overhead of path objects in is neglectable unless it's in a loop (and this is mentioned in our docs in https://github.com/astral-sh/ruff/pull/5815)

---

_Comment by @sbrugman on 2023-07-21 00:32_

Just realised that we should also detect the opposite: `.split(os.sep)` is also [frequently used](https://grep.app/search?q=.split%28os.sep%29&filter%5Blang%5D%5B0%5D=Python)

This should be a new rule, the requires a combination of `Path.name` and `Path.parent`, or `Path.parts`

---

_Comment by @dosisod on 2023-07-21 00:51_

One thing to note is that `os.path.join()` will drop falsey arguments, but `os.sep.join()` won't:

```python
>>> import os
>>> os.path.join("hello", "world")
'hello/world'
>>> os.sep.join(("hello", "", "world"))
'hello//world'
```

This isn't an issue when using it for the `Path()` constructor, but might be an issue if generalized to all uses of `os.path.join()`:

```python
>>> from pathlib import Path
>>> Path("hello", "", "world") == Path("hello", "world")
True
```

---

_Closed by @charliermarsh on 2023-07-21 01:36_

---
