```yaml
number: 1698
title: "Don't process files with CR line endings"
type: pull_request
state: closed
author: squiddy
labels: []
assignees: []
base: main
head: no-carriage-return-support
created_at: 2023-01-06T20:43:16Z
updated_at: 2023-01-29T18:30:41Z
url: https://github.com/astral-sh/ruff/pull/1698
synced_at: 2026-01-12T15:55:06Z
```

# Don't process files with CR line endings

---

_@squiddy_

Returning an error in `check_path` is currently mapped to an IOError, which doesn't seem fitting. Open to suggestions.

---

_Review comment by @charliermarsh on `src/linter.rs`:77 on 2023-01-06 23:55_

Is there any way to do this lazily? Like, detect when it would become a problem, and fail in that case? (Not asking you to make that change.)

---

_@charliermarsh reviewed on 2023-01-06 23:55_

---

_@charliermarsh reviewed on 2023-01-06 23:55_

---

_Review comment by @charliermarsh on `src/linter.rs`:77 on 2023-01-06 23:55_

Actually, I guess it's fine, this should be quite cheap...

---

_@charliermarsh reviewed on 2023-01-07 00:19_

---

_Review comment by @charliermarsh on `src/source_code_style.rs`:184 on 2023-01-07 00:19_

Would it be faster to check for the first `\n`, then check if it's preceded by a `\r`, and only check for a `\r` if we don't find _any_ `\n`?

As-is, we have to traverse the entire string for the `\n` case, to learn that there's no `\r`.

---

_@charliermarsh reviewed on 2023-01-07 00:37_

---

_Review comment by @charliermarsh on `src/source_code_style.rs`:178 on 2023-01-07 00:37_

Oh, in Cargo.toml, we should also disable the `cr_lines` feature on Ropey, I think? Or, actually, we could keep it on and consider using `Ropey` to find the first line ending quickly?

---

_@squiddy reviewed on 2023-01-07 16:08_

---

_Review comment by @squiddy on `src/source_code_style.rs`:178 on 2023-01-07 16:08_

I completely missed that `ropey` seems to have proper support for `CR` line endings. Perhaps we can support them after all.. I'll check that.

---

_@charliermarsh reviewed on 2023-01-07 16:24_

---

_Review comment by @charliermarsh on `src/source_code_style.rs`:178 on 2023-01-07 16:24_

But I think things like `.lines()` still won't work, right? Per your original note in the issue.

---

_Comment by @charliermarsh on 2023-01-29 18:30_

Closing for now, but we should re-visit at some point.

---

_Closed by @charliermarsh on 2023-01-29 18:30_

---
