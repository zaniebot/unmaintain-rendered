```yaml
number: 19492
title: "Formatter inserts trailing newline at the end of a file even when inside a `fmt: off` region"
type: issue
state: open
author: MilkClouds
labels:
  - bug
  - suppression
  - formatter
assignees: []
created_at: 2025-07-22T16:30:25Z
updated_at: 2025-07-31T12:17:43Z
url: https://github.com/astral-sh/ruff/issues/19492
synced_at: 2026-01-12T15:54:56Z
```

# Formatter inserts trailing newline at the end of a file even when inside a `fmt: off` region

---

_@MilkClouds_

### Summary

title is all.

you can test this bug with:
```
# fmt: off
print(1,2,3,4,5,9,13518,8315987831959831,3851758979515,31958139573175915,158315897151935,135815798151,)
```

make sure end of file does not contain newline. run `ruff format`. then newline is inserted.

### Version

ruff 0.12.4

---

_Comment by @ntBre on 2025-07-22 16:43_

I can reproduce this:

```shell
$ printf '# fmt: off\n1' | ruff format --diff -
@@ -1,2 +1,2 @@
 # fmt: off
-1
\ No newline at end of file
+1
```

It's not strictly related to W292, which is a lint rule and shouldn't be affected by the formatter, but the formatter does add a newline to the end of the file even with `# fmt: off`.


---

_Label `formatter` added by @ntBre on 2025-07-22 16:43_

---

_Label `suppression` added by @ntBre on 2025-07-22 16:43_

---

_Renamed from "`fmt: off` does not ignore W292" to "Formatter inserts trailing newline at the end of a file even when inside a `fmt: off` region" by @MichaReiser on 2025-07-23 06:47_

---

_Label `bug` added by @MichaReiser on 2025-07-23 06:47_

---
