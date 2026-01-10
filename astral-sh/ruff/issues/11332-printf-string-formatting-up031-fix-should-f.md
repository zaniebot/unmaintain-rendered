```yaml
number: 11332
title: "`printf-string-formatting` (`UP031`) - fix should f-string instead of `str.format`"
type: issue
state: open
author: DetachHead
labels:
  - fixes
assignees: []
created_at: 2024-05-08T08:11:49Z
updated_at: 2025-04-28T21:43:15Z
url: https://github.com/astral-sh/ruff/issues/11332
synced_at: 2026-01-10T11:09:53Z
```

# `printf-string-formatting` (`UP031`) - fix should f-string instead of `str.format`

---

_Issue opened by @DetachHead on 2024-05-08 08:11_

when both `UP031` and `UP032` are enabled, the fix for `UP031` causes `UP032`:

```py
foo = "foo"
"%s" % (foo,) # error: UP031
```
after applying the quick fix:
```py
foo = "foo"
"{}".format(foo) # error: UP032
```
https://play.ruff.rs/c4a825b4-12e5-4479-9167-c0f4388a17c2

this means i have to apply 2 fixes to get to the correct code. it would be nice if `UP031` suggested f strings instead.

---

_Comment by @dhruvmanila on 2024-05-08 08:16_

We iteratively apply fixes until the source code is stable so if you've select both `UP031` and `UP032`, the final diff should be the f-string code:

```console
$ ruff check --unsafe-fixes --fix --no-cache --diff --select=UP031,UP032 src/autofix.py
--- src/autofix.py
+++ src/autofix.py
@@ -1,2 +1,2 @@
 foo = "foo"
-"%s" % (foo,)
+f"{foo}"

Would fix 2 errors.
```

Can you tell me how are you running `ruff`?

---

_Comment by @DetachHead on 2024-05-08 08:50_

i meant when applying the quick fix from vscode (or in this case the playground):

![image](https://github.com/astral-sh/ruff/assets/57028336/4cfc2f16-d627-4568-8439-831442bc0f84)

that only applies one quick fix at a time. IMO, since f strings are the preferred method of interpolating strings, i think `UP031`'s message and fix should be to change it to an f string

---

_Label `fixes` added by @AlexWaygood on 2024-05-08 08:51_

---

_Comment by @dhruvmanila on 2024-05-08 09:06_

I think https://github.com/astral-sh/ruff-vscode/issues/451 is related.

For this specific case, maybe we could prioritize `UP032` if both `UP031` and `UP032` are enabled similar to what @charliermarsh did in https://github.com/astral-sh/ruff/pull/11173.



---

_Comment by @dhruvmanila on 2024-05-08 09:12_

Oh, but that might not be possible because `UP032` isn't triggered until `UP031` is fixed.

---

_Comment by @charliermarsh on 2024-05-08 14:23_

Yeah this is roughly by design though I understand why it's not ideal in an editor. IIRC we only support translating `%` to `.format`, and `.format` to f-string.

---

_Comment by @autinerd on 2024-05-09 19:58_

Maybe it would be a good idea to merge UP031 and UP032. I don't know if ruff has a minimum version, but f-strings exist since Python 3.6 🤔

---

_Comment by @Avasam on 2024-05-31 21:05_

@autinerd `UP031` and `UP032` have different concerns. `.format` can still be used to avoid very long lines or multiline string concatenation (styling preferences).

---

_Comment by @Avasam on 2025-04-28 21:43_

I'll quote my message from Discord:

---
I've been having a hard time with [printf-string-formatting (UP031)](https://docs.astral.sh/ruff/rules/printf-string-formatting/) + [f-string (UP032)](https://docs.astral.sh/ruff/rules/f-string/)

I don't necessarily want to enforce changing `.format` to f-strings (using format can be very readable, sometimes moreso than f-strings). But when autofixing `UP031` I would prefer them to be autofixed as f-strings, not `.format`.

Moreso, I can't really enable either on older projects because they catch *so many cases* that are not autofixable.

Especially projects that may be wary of manual changes in older code.

And I don't think that I can configure a specific rule to "run autofixes, but don't fail otherwise".

---

Being able to configure `UP031` to fix directly to f-string without passing by / having to enable `UP032` would be beneficial.

---
