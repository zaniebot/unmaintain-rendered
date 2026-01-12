```yaml
number: 15741
title: Add support for configuring indent-width from user settings in VSCode
type: issue
state: closed
author: xabru
labels:
  - wish
  - server
assignees: []
created_at: 2025-01-25T16:37:55Z
updated_at: 2025-03-23T08:06:28Z
url: https://github.com/astral-sh/ruff/issues/15741
synced_at: 2026-01-12T15:54:54Z
```

# Add support for configuring indent-width from user settings in VSCode

---

_@xabru_

### Description

### Description:
Currently, the Ruff extension for VSCode allows users to configure the line length through the user settings, but there is no option to configure the indentation width (the number of spaces used for indentation). It would be highly beneficial to add an option for configuring the indent-width directly from the user settings in VSCode, providing more flexibility and consistency with the user's overall development environment settings.

### Suggested Solution:
Add a configuration option in settings.json for ruff.indent-width, allowing users to set the desired number of spaces for indentation. This could work similarly to how the line-length option is already handled.



---

_Label `wish` added by @MichaReiser on 2025-01-25 18:09_

---

_Label `server` added by @dhruvmanila on 2025-01-27 15:10_

---

_Comment by @MichaReiser on 2025-03-23 08:06_

This is now supported with the new inline [`configuration` client setting](https://docs.astral.sh/ruff/editors/settings/#configuration)

---

_Closed by @MichaReiser on 2025-03-23 08:06_

---
