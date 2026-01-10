```yaml
number: 15229
title: "Errors for attempted autofixes visible to users even without passing `--fix` and for noqa lines"
type: issue
state: closed
author: AA-Turner
labels:
  - cli
assignees: []
created_at: 2025-01-02T22:04:11Z
updated_at: 2025-01-03T14:50:14Z
url: https://github.com/astral-sh/ruff/issues/15229
synced_at: 2026-01-10T11:09:56Z
```

# Errors for attempted autofixes visible to users even without passing `--fix` and for noqa lines

---

_Issue opened by @AA-Turner on 2025-01-02 22:04_

I think related to #5454. cc @MichaReiser @wookie184

### Reproducer:

Save to `bug.py`: 

```python
# bug.py
from typing import Callable


def spam() -> None:
    C = Callable[[int], None]  # NoQA: UP006
    _ = C
    _ = Callable  # NoQA: UP006
```

Run the following:

```ps1con
PS> ruff --version
ruff 0.8.5
PS> ruff clean; ruff check bug.py --isolated --select UP006 --preview
Removing cache at: .ruff_cache
error: Failed to create fix for NonPEP585Annotation: Unable to insert `Callable` into scope due to name conflict
error: Failed to create fix for NonPEP585Annotation: Unable to insert `Callable` into scope due to name conflict
All checks passed!
```

Note the error only appears when there is no existing cache, hence `ruff clean`. Adding or removing `from __future__ import annotations` has no effect.

A

---

_Comment by @wookie184 on 2025-01-03 01:23_

Good find, thanks for the issue.

From what I can tell this isn't a new issue, and #5454 just made it easier to trigger it. An example that errors on 0.8.4 is 
```python
from typing import Deque
deque = ...
Deque
```

That said, I also remember noticing this issue while writing the PR and implementing a fix. Unfortunately it seems like the fix was lost at some point, probably while a merge conflict was being resolved, and although I wrote a snapshot test for this case which *is* still there, the error doesn't seem to be shown in the snapshot file so the regression wasn't caught!

---

_Comment by @dhruvmanila on 2025-01-03 02:32_

Thanks for the report. @wookie184 Do you plan on addressing this? It's ok if you don't have the time, I can look at it and do a release later today.

---

_Label `bug` added by @dhruvmanila on 2025-01-03 02:33_

---

_Comment by @wookie184 on 2025-01-03 02:35_

> Thanks for the report. @wookie184 Do you plan on addressing this? It's ok if you don't have the time, I can look at it and do a release later today.

If you could that would be great

---

_Comment by @charliermarsh on 2025-01-03 02:39_

@dhruvmanila -- We might also want to move these fix failures to `--verbose`. I don't know if they make sense as user-visible warnings.

---

_Comment by @dhruvmanila on 2025-01-03 02:49_

> We might also want to move these fix failures to `--verbose`. I don't know if they make sense as user-visible warnings.

Yeah, that makes sense.

---

_Assigned to @dhruvmanila by @dhruvmanila on 2025-01-03 02:49_

---

_Comment by @dylwil3 on 2025-01-03 04:03_

> That said, I also remember noticing this issue while writing the PR and implementing a fix. Unfortunately it seems like the fix was lost at some point, probably while a merge conflict was being resolved, and although I wrote a snapshot test for this case which is still there, the error doesn't seem to be shown in the snapshot file so the regression wasn't caught!

Apologies @wookie184 that was probably my fault - interactive rebasing feels like pinball with 20 balls at once, I must've lost a few! Please let me know if you notice any other errors in transcription and I'll fix them.

If you don't get around to this @dhruvmanila, feel free to reassign it to me since I made the mess in the first place!

---

_Comment by @dhruvmanila on 2025-01-03 04:14_

> If you don't get around to this [<img alt="" width="16" height="16" src="https://avatars.githubusercontent.com/u/67177269?s=64&amp;u=3f6384e9bb2b4338388f8729ce4820e080607ebb&amp;v=4">@dhruvmanila](https://github.com/dhruvmanila), feel free to reassign it to me since I made the mess in the first place!

@dylwil3 Feel free to take this on if you want as you have the most context and will be able to resolve this faster than me :)

---

_Assigned to @dylwil3 by @dylwil3 on 2025-01-03 04:27_

---

_Unassigned @dhruvmanila by @dylwil3 on 2025-01-03 04:27_

---

_Comment by @dylwil3 on 2025-01-03 05:10_

Just to clarify: Unless I'm misunderstanding, I don't think this has anything to do with #5454 . The actual fix/diagnostic behavior is as expected here, and there's no issues in the snapshot tests from that PR that I can find.

This is really _just_ about the visibility of that error message coming from `try_set_fix`. For example, you can see the same behavior with a FastAPI rule that also tries to get an import that would cause a name collision:

```console
❯ cat tmp.py
from fastapi import Depends, FastAPI

Annotated = "hey"

app = FastAPI()


async def common_parameters(q: str | None = None, skip: int = 0, limit: int = 100):
    return {"q": q, "skip": skip, "limit": limit}


@app.get("/items/")
async def read_items(commons: dict = Depends(common_parameters)):
    return commons

❯ uvx ruff check tmp.py --isolated --select FAST002 --preview --no-cache
error: Failed to create fix for FastApiNonAnnotatedDependency: Unable to insert `Annotated` into scope due to name conflict
tmp.py:13:22: FAST002 FastAPI dependency without `Annotated`
   |
12 | @app.get("/items/")
13 | async def read_items(commons: dict = Depends(common_parameters)):
   |                      ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ FAST002
14 |     return commons
   |
   = help: Replace with `typing.Annotated`

Found 1 error.
```

The cache is sort of a red herring here that I believe has to do with the noqa comments in the original example: on the first run, I think ruff is running the diagnostic on those lines (which eagerly tries to make a fix anyway), and on subsequent runs the cache says not to worry about it.

All that to say: I'm gonna fix the visibility of the errors here and call it good unless someone can tell me I'm missing something!

---

_Label `bug` removed by @dylwil3 on 2025-01-03 05:13_

---

_Label `cli` added by @dylwil3 on 2025-01-03 05:13_

---

_Renamed from "0.8.5 regression: "error: Failed to create fix for NonPEP585Annotation"" to "Errors for attempted autofixes visible to users even without passing `--fix` and for noqa lines" by @dylwil3 on 2025-01-03 06:02_

---

_Closed by @dylwil3 on 2025-01-03 14:50_

---
