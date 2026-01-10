---
number: 12687
title: "Allow using `--dry-run` with `--frozen` / `--locked` in `uv sync`"
type: issue
state: closed
author: aldanor
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2025-04-06T03:24:56Z
updated_at: 2025-04-14T13:08:27Z
url: https://github.com/astral-sh/uv/issues/12687
synced_at: 2026-01-10T01:25:23Z
---

# Allow using `--dry-run` with `--frozen` / `--locked` in `uv sync`

---

_Issue opened by @aldanor on 2025-04-06 03:24_

### Question

When attempting to do something like
```sh
> uv sync --inexact --frozen --dry-run
```
to see what uv sync will do before it actually does it, it complains that dry run mode is mutually exclusive with frozen/locked mode:
```sh
error: the argument '--dry-run' cannot be used with '--frozen'
```
... so the only way to preview the changes to the environment is to actually run it?

Thanks!

### Platform

macOS

### Version

0.6.4

---

_Label `question` added by @aldanor on 2025-04-06 03:24_

---

_Comment by @FishAlchemist on 2025-04-06 04:26_

**--dry-run**:
> In dry-run mode, uv will resolve the project’s dependencies and report on the resulting changes to both the lockfile and the project environment, but will not modify either.

**--frozen**:
> Instead of checking if the lockfile is up-to-date, uses the versions in the lockfile as the source of truth. 

(The above is a snippet from its documentation.)

According to the documentation, ``--dry-run`` doesn't mean it skips checking if the lockfile is up-to-date. It just doesn't write the final changes to the file. And the act of checking the lockfile itself already conflicts with the ``--frozen`` option.

Regarding whether to allow ``--dry-run`` to support ``--frozen``, I think that's something we can discuss further. 

---

_Comment by @aldanor on 2025-04-06 05:19_

@FishAlchemist Thanks for chiming in - the current docs seem to align with the current behaviour indeed.

I guess my question was more about this
> Regarding whether to allow `--dry-run` to support `--frozen`, I think that's something we can discuss further.

If you intend to use uv sync with `--frozen` enabled, currently there's no way of previewing the changes "to the project environment" because of the enforced mutual exclusivity of the flags.



---

_Comment by @zanieb on 2025-04-06 15:43_

It seems fine to allow it to be used with `--frozen` and only perform the environment check.

---

_Label `question` removed by @zanieb on 2025-04-06 15:43_

---

_Label `enhancement` added by @zanieb on 2025-04-06 15:43_

---

_Renamed from "Why `--dry-run` in `uv sync` is mutually exclusive with `--frozen` (and `--locked`)?" to "Allow using `--dry-run` with `--frozen` / `--locked` in `uv sync`" by @zanieb on 2025-04-06 15:44_

---

_Comment by @zanieb on 2025-04-06 15:45_

Similarly, `--locked` seems fine — we'd just exit with a non-zero code if the lockfile is out-of-date but (maybe) perform the environment check as described for `--frozen`?

---

_Comment by @aldanor on 2025-04-07 00:06_

@zanieb Yep, exactly (re: both).

---

_Label `help wanted` added by @zanieb on 2025-04-07 23:12_

---

_Comment by @christeefy on 2025-04-08 13:00_

Mind if I take a stab at this? 

---

_Comment by @zanieb on 2025-04-08 17:24_

Go for it!

---

_Referenced in [astral-sh/uv#12778](../../astral-sh/uv/pulls/12778.md) on 2025-04-09 13:02_

---

_Closed by @charliermarsh on 2025-04-14 13:08_

---

_Closed by @charliermarsh on 2025-04-14 13:08_

---
