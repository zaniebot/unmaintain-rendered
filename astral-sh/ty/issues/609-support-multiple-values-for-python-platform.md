```yaml
number: 609
title: "support multiple values for `python-platform`"
type: issue
state: closed
author: DetachHead
labels:
  - documentation
  - help wanted
assignees: []
created_at: 2025-06-08T01:54:07Z
updated_at: 2025-06-09T12:01:26Z
url: https://github.com/astral-sh/ty/issues/609
synced_at: 2026-01-12T15:54:23Z
```

# support multiple values for `python-platform`

---

_@DetachHead_

it's very common for projects to target multiple python versions, but it seems that the [`python-platform`](https://github.com/astral-sh/ty/blob/main/docs/reference/configuration.md#python-platform) setting only supports targeting one platform.

it would be nice if you could specify `"all"` ([like pyright](https://microsoft.github.io/pyright/#/configuration?id=environment-options)), or perhaps an array of supported platforms.

---

_Comment by @MichaReiser on 2025-06-08 05:48_

ty does support `all`. But it seems that we failed to mentione that in both the CLI and configuration reference.

---

_Label `documentation` added by @MichaReiser on 2025-06-08 05:48_

---

_Added to milestone `Beta` by @MichaReiser on 2025-06-08 05:48_

---

_Label `help wanted` added by @MichaReiser on 2025-06-08 05:48_

---

_Closed by @AlexWaygood on 2025-06-09 12:01_

---
