```yaml
number: 21973
title: "[G004] Fix resolve incorrect when `\\n` is present in the literal."
type: issue
state: closed
author: richardhapb
labels: []
assignees: []
created_at: 2025-12-14T04:15:33Z
updated_at: 2025-12-15T14:13:47Z
url: https://github.com/astral-sh/ruff/issues/21973
synced_at: 2026-01-12T15:54:58Z
```

# [G004] Fix resolve incorrect when `\n` is present in the literal.

---

_@richardhapb_

### Summary

In rule `G004` when the fix is applied to a `fstring` like this.

```
logger.info(f"Using vectorized approach for \n{timespan}")
```

A new line is introduced and generate a syntax error.

```
logger.info("Using vectorized approach for 
%s", timespan)
```

### Version

ruff 0.14.9

---

_Comment by @ntBre on 2025-12-15 14:13_

Thanks for the report! I believe this is a duplicate of #20151.

---

_Closed by @ntBre on 2025-12-15 14:13_

---
