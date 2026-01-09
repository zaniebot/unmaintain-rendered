---
number: 3378
title: F401 has started being incorrectly missed
type: issue
state: closed
author: henryiii
labels:
  - question
assignees: []
created_at: 2023-03-07T03:47:00Z
updated_at: 2023-03-22T17:03:32Z
url: https://github.com/astral-sh/ruff/issues/3378
synced_at: 2026-01-07T13:12:14-06:00
---

# F401 has started being incorrectly missed

---

_Issue opened by @henryiii on 2023-03-07 03:47_

This pattern suddenly has stopped triggering F401:


```python
try:
   import something  # noqa: F401
except ModuleNotFound:
   ...
```

If `something` is never used, the F401 is correct. It's helpful to tell the reader that this is not going to be used later, and it's a hint that this is actually often an incorrect pattern (the correct one in Python 3.4+ is to use `importlib.utils.find_spec("something")`, which won't trigger the error and will be faster, since it doesn't have to run the `Loader`, and won't trigger side effects).

I couldn't find anything in the changelogs or PRs about it, but happened between v0.0.253 and v0.0.254. https://github.com/scikit-hep/hist/pull/482 and https://github.com/pypa/build/pull/585 are two recent cases where this is happening.

---

_Comment by @charliermarsh on 2023-03-07 03:50_

For context the relevant PR and Issue is https://github.com/charliermarsh/ruff/pull/3288 and https://github.com/charliermarsh/ruff/issues/3279, but I'm guessing you don't agree with the change :)

---

_Comment by @henryiii on 2023-03-07 04:15_

It should have been in the changelogs, and no, I don't like the change. It wasn't broken. If you import something, you should use it (that's the point of the check). If you only want to ask if something is there, that's what `importlib.util.find_spec` is useful for. It's faster and doesn't trigger side effects. And if you really want the side effects, then it deserves a noqa.

What about just making the error message smarter in the case it's inside the block, giving the `importlib.util.find_spec` suggestion? As it stands, this is a regression from the matching Flake8 check too, Ruff is less useful now than Flake8 for this check. If I have a "find unused imports" checks, I want to find unused imports, including inside a try block. They are still unused. :)

---

_Comment by @henryiii on 2023-03-07 04:16_

`importlib.util.find_spec` used to be `importlib.util.find_module`, which I think was more discoverable, but it triggered the import side effects, so it was replaced in Python 3.4 with `find_spec`. But that's exactly what it's for.

---

_Comment by @charliermarsh on 2023-03-07 04:24_

Yeah I hear you, it _is_ in the changelog, but it's marked as a bugfix rather than a breaking change (https://github.com/charliermarsh/ruff/releases/tag/v0.0.254 -- search for `ModuleNotFound`).

I think you're right that we can do better here. Just to explain the reasoning a bit more: prior to this change, the autofix was unsafe in those cases, because the import itself affected the `try`-`catch`. My primary motivation in changing the rule was to avoid autofix breakages due to code that _depended_ on the import (i.e., cases in which the import was effectively being _used_ by the `try`-`catch`).

If we revert, and add a more refined error message, would you agree that we should still avoid autofixing, since the import in that case _does_ have a side-effect?

(Aside: we could also look at the `except` block and continue flagging in the event that it's empty. Would that have mattered here?)


---

_Label `question` added by @charliermarsh on 2023-03-07 04:25_

---

_Comment by @henryiii on 2023-03-07 14:56_

Ahh, I was searching for the error code.

I agree about the autofix; I've had that happen once, and it's a bit annoying. But all auto fixes that remove lines of code can be - T201 can delete all `print(...)` lines, F401 can delete one or more imports just because you misspell a usage, etc. Though this is a particularly bad because it could be what you intended to do. But knowing how to check and revert changes & noqa them is a required prerequisite to use any autofixer, IMO.

I'd agree that keeping it from being autofixed would be very helpful. In fact, I think the problem is that it's the wrong autofix, since this structure is not a no-op. It would be better to instead autofix it into the `importlib.util.find_spec` form! (still a small change of behavior, as it removes side effects, but much closer to the original intent and likely to be the desired behavior, vs. removing it).

The `except blocks` might be empty. If you look at the `pypa/build` PR above, that had an empty `except` block. The point of that one was to verify that `flit_core` was not installed. I sometimes have to do the same thing with `setuptools_scm`, since it changes the behavior of (all usages) of setuptools. Etc.

---

_Comment by @charliermarsh on 2023-03-07 15:22_

Yeah it's tricky.

If the user has:

```py
try:
   import something
except ModuleNotFound:
   pass
```

And `something` _was_ used, but the usages were removed, and then we autofix it to:

```py
try:
   importlib.util.find_spec("something")  # I can't remember the exact API
except ModuleNotFound:
   ...
```

That could also be unexpected, but of course it's hard to know, we don't have enough information there to confidently infer the intent.

As a compromise, then, what we could do is:

- Flag unused imports in `ModuleNotFoundError`-handled bodies.
- Avoid automatically fixing them.
- Include a modified "Did you mean...?"-style error message for those cases.


---

_Comment by @henryiii on 2023-03-07 15:53_

It's:

```python
if importlib.util.find_spec("something") is None:
    ...
```

;) Also, autofixing always has this issue. Deleting unused variables, etc all can really make a mess if you make a mistake. I even have a few checks I usually suggest marking unfixable - see https://scikit-hep.org/developer/style#ruff - just because of how easy it is to have them be a bit surprising. But, IMO, that's completely okay - autofixers have permission to change code - if you didn't like it doing that, you wouldn't enable `--fix` in the first place. :) They are fantastic though if you really need the fix on a large codebase, and tweaking settings a bit to get the fix (like adding `--fix` or commenting out an unfixable line) is much faster than manually editing tens or hundreds of lines.

I'm not very seriously suggesting an autofix to this, though a suggestion including `find_spec` would be great. I think the "compromise" suggestion sounds close to ideal.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-03-08 05:02_

---

_Comment by @charliermarsh on 2023-03-21 00:26_

I still intend to change this.

---

_Referenced in [astral-sh/ruff#3658](../../astral-sh/ruff/pulls/3658.md) on 2023-03-21 23:14_

---

_Closed by @charliermarsh on 2023-03-22 17:03_

---
