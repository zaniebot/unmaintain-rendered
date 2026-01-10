```yaml
number: 18164
title: "ASYNC115 and ASYNC116 check the wrong argument name for `anyio.sleep`"
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - good first issue
assignees: []
created_at: 2025-05-18T18:17:00Z
updated_at: 2025-05-26T09:53:05Z
url: https://github.com/astral-sh/ruff/issues/18164
synced_at: 2026-01-10T11:09:58Z
```

# ASYNC115 and ASYNC116 check the wrong argument name for `anyio.sleep`

---

_Issue opened by @dscorbett on 2025-05-18 18:17_

### Summary

[`async-zero-sleep` (ASYNC115)](https://docs.astral.sh/ruff/rules/async-zero-sleep/) and [`long-sleep-not-forever` (ASYNC116)](https://docs.astral.sh/ruff/rules/long-sleep-not-forever/) check both `trio.sleep` and `anyio.sleep`. The argument can be specified by position or name. Trio names it `seconds` whereas AnyIO names it `delay`. ASYNC115 and ASYNC116 always check for the name `seconds`, causing false positives and false negatives for AnyIO.

```console
$ cat <<'# EOF' | ruff --isolated check - --preview --select ASYNC115,ASYNC116 --unsafe-fixes --diff -q
import anyio
async def async115_false_positive(): await anyio.sleep(seconds=0)
async def async115_false_negative(): await anyio.sleep(delay=0)
async def async116_false_positive(): await anyio.sleep(seconds=86401)
async def async116_false_negative(): await anyio.sleep(delay=86401)
# EOF
@@ -1,5 +1,5 @@
 import anyio
-async def async115_false_positive(): await anyio.sleep(seconds=0)
+async def async115_false_positive(): await anyio.lowlevel.checkpoint()
 async def async115_false_negative(): await anyio.sleep(delay=0)
-async def async116_false_positive(): await anyio.sleep(seconds=86401)
+async def async116_false_positive(): await anyio.sleep_forever()
 async def async116_false_negative(): await anyio.sleep(delay=86401)


```

### Version

ruff 0.11.10 (b35bf8ae0 2025-05-15)

---

_Label `bug` added by @MichaReiser on 2025-05-18 18:24_

---

_Label `good first issue` added by @MichaReiser on 2025-05-18 18:24_

---

_Comment by @Vasanth-96 on 2025-05-18 19:41_

Hi I would like to contribute to this issue, thanks 

---

_Assigned to @Vasanth-96 by @ntBre on 2025-05-19 13:20_

---

_Closed by @MichaReiser on 2025-05-26 09:53_

---
