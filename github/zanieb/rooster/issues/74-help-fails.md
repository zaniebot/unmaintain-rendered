---
number: 74
title: "`--help` fails"
type: issue
state: closed
author: zsol
labels: []
assignees: []
created_at: 2025-09-08T10:25:11Z
updated_at: 2025-09-12T13:07:32Z
url: https://github.com/zanieb/rooster/issues/74
synced_at: 2026-01-07T13:12:14-06:00
---

# `--help` fails

---

_Issue opened by @zsol on 2025-09-08 10:25_

```
❯ uvx --from rooster-blue rooster --help
Traceback (most recent call last):

  File "/Users/zsol/.cache/uv/archive-v0/7OScaHJPMw5W8_YfceXsm/bin/rooster", line 12, in <module>
    sys.exit(app())
             ~~~^^

TypeError: Parameter.make_metavar() missing 1 required positional argument: 'ctx'
```

---

_Comment by @zanieb on 2025-09-08 12:53_

This is fixed on `main` — annoyingly it's a Click / typer incompatibility.

Dup of https://github.com/zanieb/rooster/issues/65

---

_Closed by @zanieb on 2025-09-12 13:07_

---
