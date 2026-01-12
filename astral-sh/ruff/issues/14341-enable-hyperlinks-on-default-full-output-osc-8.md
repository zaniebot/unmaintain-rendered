```yaml
number: 14341
title: Enable hyperlinks on default full output (OSC 8)
type: issue
state: closed
author: ssbarnea
labels:
  - diagnostics
assignees: []
created_at: 2024-11-14T17:25:15Z
updated_at: 2025-11-19T08:29:05Z
url: https://github.com/astral-sh/ruff/issues/14341
synced_at: 2026-01-12T15:54:53Z
```

# Enable hyperlinks on default full output (OSC 8)

---

_@ssbarnea_

Current terminal output (full) does provide nice ANSI colored output but without making the rule labels clickable.

Ideally these should point to official documentation pages, as these contain very good examples of adding hyperlinks.

The hyperlink terminal support extension is called OSC 8 and by now is already supported by most terminals.

```
    def link(uri: str, label: str | None = None) -> str:
        """Return a link."""
        if label is None:
            label = uri
        parameters = ""

        # OSC 8 ; params ; URI ST <name> OSC 8 ;; ST
        escape_mask = "\033]8;{};{}\033\\{}\033]8;;\033\\"

        return escape_mask.format(parameters, uri, label)
```

References:

- https://github.com/Alhadis/OSC8-Adoption/

---

_Label `diagnostics` added by @MichaReiser on 2024-11-14 17:36_

---

_Comment by @MichaReiser on 2025-11-19 08:29_

This was implemented in https://github.com/astral-sh/ruff/pull/21514 and will ship in the next release

---

_Closed by @MichaReiser on 2025-11-19 08:29_

---
