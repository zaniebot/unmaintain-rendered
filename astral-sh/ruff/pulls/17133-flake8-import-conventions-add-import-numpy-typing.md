```yaml
number: 17133
title: "[flake8-import-conventions] Add import `numpy.typing as npt` to default `flake8-import-conventions.aliases`"
type: pull_request
state: merged
author: amin-not-found
labels:
  - configuration
assignees: []
merged: true
base: main
head: npt-import-convention
created_at: 2025-04-01T20:13:26Z
updated_at: 2025-04-02T07:25:46Z
url: https://github.com/astral-sh/ruff/pull/17133
synced_at: 2026-01-10T19:40:37Z
```

# [flake8-import-conventions] Add import `numpy.typing as npt` to default `flake8-import-conventions.aliases`

---

_Pull request opened by @amin-not-found on 2025-04-01 20:13_

## Summary
Adds import `numpy.typing as npt` to `default in flake8-import-conventions.aliases`
Resolves #17028

## Test Plan
Manually ran local ruff on the altered fixture and also ran `cargo test`

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_import_conventions/custom.py`:40 on 2025-04-01 20:45_

Is this one supposed to be `import numpy.typing as npt`?

---

_@ntBre reviewed on 2025-04-01 20:52_

Thanks! This looks good except for one question about the `conventional` test case.

If you end up changing that, it might be nice to move the new tests to the bottom of the test file too. That will make it much easier to review the changes since it won't move any of the other snapshots!

@MichaReiser does this need to be in preview? I think it's a new default restriction on the allowed name for the `numpy.typing` import, but I guess we'll see if there are any ecosystem hits.

---

_Label `configuration` added by @ntBre on 2025-04-01 20:52_

---

_Comment by @github-actions[bot] on 2025-04-01 20:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @amin-not-found on 2025-04-01 21:33_

> Is this one supposed to be `import numpy.typing as npt`?

Yes, that's right. I forgot to type `typing` there. It should be fixed now.

>  it might be nice to move the new tests to the bottom of the test file too

I have changed it, but I was trying to keep the structure. Maybe I can reorder the imports by library instead, so that it has some kind of structure that is easy to keep whit new additions? Something like this:
```python
import altair  # unconventional
import altair as altr  # unconventional
import altair as alt  # conventional

import dask.array  # unconventional
import dask.array as darray  # unconventional
...
```
Or maybe not if that's too much change.

---

_Comment by @ntBre on 2025-04-01 21:39_

Nice! Yeah it's always a tradeoff on keeping the structure or not. I think it's okay like you have it now though, without worrying about rearranging the other cases.

Let's wait for confirmation on whether this needs to be preview-gated or not, but otherwise this looks good to me.

---

_@ntBre approved on 2025-04-01 21:40_

---

_Comment by @amin-not-found on 2025-04-01 22:10_

Yeah I get it and also appreciate your feedback

---

_Comment by @MichaReiser on 2025-04-02 07:25_

I think it's fine to ship this as is. I suspect that most numpy users added `npt` to their convention configuration already and, if not, it only removes the need for some suppression comments. 

---

_Merged by @MichaReiser on 2025-04-02 07:25_

---

_Closed by @MichaReiser on 2025-04-02 07:25_

---
