```yaml
number: 18143
title: "PLC2701 treats `os._exit` as private, unlike SLF001"
type: issue
state: open
author: dscorbett
labels:
  - bug
assignees: []
created_at: 2025-05-16T20:46:53Z
updated_at: 2026-01-13T15:37:12Z
url: https://github.com/astral-sh/ruff/issues/18143
synced_at: 2026-01-13T16:27:25Z
```

# PLC2701 treats `os._exit` as private, unlike SLF001

---

_@dscorbett_

### Summary

Despite its name, [`os._exit`](https://docs.python.org/3.9/library/os.html#os._exit) is not private; see #6483. [`import-private-name` (PLC2701)](https://docs.astral.sh/ruff/rules/import-private-name/) treats it as private, which is inconsistent with [`private-member-access` (SLF001)](https://docs.astral.sh/ruff/rules/private-member-access/). The two rules should share the same set of exceptionally non-private module members (which is currently just `os._exit`).

```console
$ cat >plc2701.py <<'# EOF'
from os import _exit
# EOF

$ ruff --isolated check plc2701.py --preview --select PLC2701 --output-format concise -q
plc2701.py:1:16: PLC2701 Private name import `_exit` from external module `os`
```

### Version

ruff 0.11.10 (b35bf8ae0 2025-05-15)

---

_Label `bug` added by @ntBre on 2025-05-16 21:41_

---

_Comment by @danparizher on 2025-05-17 00:08_

Would the solution here not be as simple as adding this snippet to `crates\ruff_linter\src\rules\pylint\rules\import_private_name.rs`?:
```rust
        if matches!(import_info.qualified_name.segments(), ["os", "_exit"]) {
            continue;
        }
```
Are there any situations where we would need to expand on this?

---

_Comment by @dscorbett on 2025-05-18 18:20_

That solution could work. There are other underscore-initial public module members in the standard library, though, so the list will probably be expanded at some point.

---

_Comment by @manueljacob on 2026-01-13 13:31_

> That solution could work. There are other underscore-initial public module members in the standard library, though, so the list will probably be expanded at some point.

I think it would be useful to keep the list shared between SLF001 and PLC2701. I’m not familiar with the ruff code base. Maybe somewhere in `crates/ruff_python_stdlib`?

---

_Comment by @ntBre on 2026-01-13 15:37_

Yeah, this could probably fit in `ruff_python_stdlib`. I think it would also be okay to share a `pub(crate)` helper function between the two rules for now.

---
