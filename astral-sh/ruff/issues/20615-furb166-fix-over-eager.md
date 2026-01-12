```yaml
number: 20615
title: "FURB166: fix: over-eager"
type: issue
state: closed
author: smurfix
labels:
  - question
assignees: []
created_at: 2025-09-28T21:14:55Z
updated_at: 2025-11-06T13:27:56Z
url: https://github.com/astral-sh/ruff/issues/20615
synced_at: 2026-01-12T15:54:57Z
```

# FURB166: fix: over-eager

---

_@smurfix_

### Summary

FURB166 mangles my code:

```diff
                if p == "":
                    res.append("")
                elif p[0] != ":":
                    res.append(something(p))
                elif p[1] == "b":
-                   res.append(int(p[2:], 2))
+                   res.append(int(p, 0))
```

There is no guarantee whatsoever that p[0] contains an ASCII zero; worse, in this case it definitely doesn't.

### Version
0.13.2

---

_Comment by @dylwil3 on 2025-09-29 13:03_

The fix is marked as unsafe essentially for this reason: https://docs.astral.sh/ruff/rules/int-on-sliced-str/#fix-safety

Are you running `--unsafe-fixes`?

I think if we tried to detect the leading prefix before making the fix available it would be quite a large restriction in the scope of the fix - but I'm not opposed to considering it.

---

_Label `question` added by @dylwil3 on 2025-09-29 13:03_

---

_Closed by @MichaReiser on 2025-11-06 13:27_

---
