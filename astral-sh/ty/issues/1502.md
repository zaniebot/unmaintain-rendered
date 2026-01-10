```yaml
number: 1502
title: "Failed to infer the type of `.parent` for `pathlib.PurePath`/`pathlib.Path`/..."
type: issue
state: closed
author: ynty
labels:
  - attribute access
assignees: []
created_at: 2025-11-08T04:25:19Z
updated_at: 2025-11-11T12:03:12Z
url: https://github.com/astral-sh/ty/issues/1502
synced_at: 2026-01-10T02:06:25Z
```

# Failed to infer the type of `.parent` for `pathlib.PurePath`/`pathlib.Path`/...

---

_Issue opened by @ynty on 2025-11-08 04:25_

### Summary

Failed to resolve the type of [`PurePath.parent`/`Path.parent`][path-parent] in [`pathlib`][pathlib], the type is inferred as `Unknown`.

[path-parent]: https://docs.python.org/3.13/library/pathlib.html#pathlib.PurePath.parent
[pathlib]: https://docs.python.org/3.13/library/pathlib.html

## Minimal Reproducible Example

1. A GitHub Repository is [here][gh], you can clone and open it with [Zed].
2. ty Playground is [here][play].

<img width="672" height="247" alt="Image" src="https://github.com/user-attachments/assets/1e2356b8-bfc5-4215-bc45-b18cb690d65c" />
<img width="671" height="277" alt="Image" src="https://github.com/user-attachments/assets/4ef1b479-8dc2-494b-9598-53627e097d14" />
<img width="1751" height="298" alt="Image" src="https://github.com/user-attachments/assets/44f77a6a-cf34-43b0-a181-2acd9bdf10a7" />

[gh]: https://github.com/ynty/ty-mwe/tree/report/pathlib-path-parent
[play]: https://play.ty.dev/fe521402-de34-454d-9d3e-5cdec121a06f

[Zed]: https://zed.dev

### Version

ty 0.0.1-alpha.25 (3abd4c968 2025-10-29)

---

_Assigned to @sharkdp by @sharkdp on 2025-11-08 08:16_

---

_Label `attribute access` added by @sharkdp on 2025-11-08 08:16_

---

_Comment by @sharkdp on 2025-11-08 10:20_

Thank you for reporting this. This will be fixed by https://github.com/astral-sh/ruff/pull/21335

---

_Closed by @sharkdp on 2025-11-10 10:13_

---

_Comment by @ynty on 2025-11-11 12:03_

> Thank you for reporting this. This will be fixed by [astral-sh/ruff#21335](https://github.com/astral-sh/ruff/pull/21335)

The problem has been solved after updating ty to 0.0.1-alpha.26.

<img width="481" height="149" alt="Image" src="https://github.com/user-attachments/assets/f3a6cd58-d68c-4712-be4c-ad32098ef493" />
<img width="696" height="275" alt="Image" src="https://github.com/user-attachments/assets/290990d6-468d-4036-a54a-1593b5370e33" />


---
