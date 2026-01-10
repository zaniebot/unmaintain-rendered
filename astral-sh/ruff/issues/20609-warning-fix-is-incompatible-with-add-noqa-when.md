```yaml
number: 20609
title: "`warning: --fix is incompatible with --add-noqa` when using `--diff`"
type: issue
state: closed
author: smurfix
labels:
  - cli
assignees: []
created_at: 2025-09-28T15:38:00Z
updated_at: 2025-09-30T06:34:19Z
url: https://github.com/astral-sh/ruff/issues/20609
synced_at: 2026-01-10T11:09:59Z
```

# `warning: --fix is incompatible with --add-noqa` when using `--diff`

---

_Issue opened by @smurfix on 2025-09-28 15:38_

### Summary

It is not possible to see what an `add-noqa` operation would do before actually doing it.

```shell
$ ruff check --select D101,D102 --add-noqa --diff src/
warning: --fix is incompatible with --add-noqa.
$ 
```
IMHO: ruff either complains, or it generates fix-ups.
In the latter case, whether ruff (a) rewrites the code or (b) adds `#noqa` should be orthogonal to whether (x) changes are applied or (y) a diff is emitted; ax ay bx by should all be possible.

### Version

0.13.2

---

_Label `cli` added by @MichaReiser on 2025-09-29 07:22_

---

_Comment by @MichaReiser on 2025-09-29 07:24_

Adding a warning (at least) makes sense. Similar to what we already do https://github.com/astral-sh/ruff/blob/959f55ae057843f8b3fe59a23577f5ae3948a903/crates/ruff/src/lib.rs#L317-L319

Although I'd suggest an error in that case because the user clearly intended to **not change** any files. This should be fairly straightforward by listing `diff` as a conflicting here:

https://github.com/astral-sh/ruff/blob/959f55ae057843f8b3fe59a23577f5ae3948a903/crates/ruff/src/args.rs#L408-L419

---

_Closed by @MichaReiser on 2025-09-30 06:34_

---
