```yaml
number: 3908
title: "`isort` thrash with `ruff` on imports"
type: issue
state: closed
author: leos
labels:
  - documentation
assignees: []
created_at: 2023-04-07T22:21:26Z
updated_at: 2023-04-13T04:05:30Z
url: https://github.com/astral-sh/ruff/issues/3908
synced_at: 2026-01-10T11:09:46Z
```

# `isort` thrash with `ruff` on imports

---

_Issue opened by @leos on 2023-04-07 22:21_

`isort` version 5.12.0, `ruff` version 0.0.261

Relevant fragments from `pyproject.toml`:
```
[tool.isort]
profile = "black"
known_first_party = ["redacted", "redacted",]
lines_after_imports = 2

[tool.ruff]
select = ["I", "ICN", "YTT"]
target-version = "py310"

[tool.ruff.isort]
known-first-party = ["redacted", "redacted",]
lines-after-imports = 2
```

`isort` and `ruff` seem to aggregate these imports differently. This may be an `isort` issue (as I think `ruff`'s way is correct), but raising it here as it's a behavior difference:

After running `isort`:
```
from tenacity import before_sleep_log
from tenacity import retry as retry_decorator
from tenacity import stop_after_attempt, wait_fixed
```
After running `ruff --fix`:
```
from tenacity import before_sleep_log, stop_after_attempt, wait_fixed
from tenacity import retry as retry_decorator
```

`tenacity` is https://github.com/jd/tenacity

---

_Comment by @charliermarsh on 2023-04-07 22:28_

Ah yeah, this is one of the two "known deviations" that we have with `isort` -- there's some discussion on it here: https://github.com/charliermarsh/ruff/issues/1381. (And I'll just link to the "known deviations" in the [FAQ](https://beta.ruff.rs/docs/faq/#how-does-ruffs-import-sorting-compare-to-isort) for completeness.) I also prefer our behavior, and there aren't currently plans to change it, though I know it's annoying that it causes larger-than-expected diffs during migration.


---

_Comment by @leos on 2023-04-07 22:38_

Ah, got it, thanks. I was hoping to have both `isort` and `ruff` running for a bit just to ensure that everything was good between them, but I guess I'll just switch over.

I actually saw the FAQ entry before filing this, but it wasn't obvious to me (w/o clicking through to the actual issues) that "a few known, minor differences in how Ruff and isort break ties between similar imports, and in how Ruff and isort treat inline comments in some cases" was talking about this issue - I didn't think these were similar imports that would be tied and it's definitely not inline comments.

---

_Comment by @charliermarsh on 2023-04-07 23:01_

Yeah that's a good point. I don't think I really internalized the scope of this deviation when I wrote that. Will fix.


---

_Label `documentation` added by @charliermarsh on 2023-04-07 23:01_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-04-07 23:01_

---

_Closed by @charliermarsh on 2023-04-13 04:05_

---
