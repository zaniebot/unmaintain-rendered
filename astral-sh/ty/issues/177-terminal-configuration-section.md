```yaml
number: 177
title: "`terminal` configuration section"
type: issue
state: closed
author: MichaReiser
labels:
  - cli
  - configuration
assignees: []
created_at: 2025-03-14T10:02:31Z
updated_at: 2025-06-18T14:16:01Z
url: https://github.com/astral-sh/ty/issues/177
synced_at: 2026-01-12T15:54:22Z
```

# `terminal` configuration section

---

_@MichaReiser_

Support configuring the `output-format` (`concise` and `full`, where `full` is the default) and `color` (`auto` | `never` and `always`) in the configuration and on the cli

---

_Renamed from "[red-knot] `terminal` configuration section" to "`terminal` configuration section" by @MichaReiser on 2025-05-07 15:26_

---

_Label `cli` added by @AlexWaygood on 2025-05-11 07:27_

---

_Label `configuration` added by @AlexWaygood on 2025-05-11 07:27_

---

_Comment by @MichaReiser on 2025-06-18 14:16_

The settings `output-format` and `error_on_warning` are implemented. The only option that is missing is `color`, but that's somewhat tricky because we load the settings **after** initializing tracing. There's a `color` CLI argument which I think is sufficient for now and it also avoids the issue where we might end up printing some log statements **before** the settings are resolved, using the wrong "color" mode. 

---

_Closed by @MichaReiser on 2025-06-18 14:16_

---
