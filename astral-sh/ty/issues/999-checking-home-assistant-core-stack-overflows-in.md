```yaml
number: 999
title: "Checking home-assistant-core stack overflows in `find_legacy_typevars`"
type: issue
state: closed
author: MichaReiser
labels:
  - fatal
assignees: []
created_at: 2025-08-15T11:40:13Z
updated_at: 2025-08-22T16:22:11Z
url: https://github.com/astral-sh/ty/issues/999
synced_at: 2026-01-12T15:54:24Z
```

# Checking home-assistant-core stack overflows in `find_legacy_typevars`

---

_@MichaReiser_

### Summary

It still requires an import but the repro is:

```py
from .connection import ActiveConnection


def handle_subscribe_bootstrap_integrations(
    hass: HomeAssistant, connection: ActiveConnection, msg: dict[str, Any]
) -> None:
    """Handle subscribe bootstrap integrations command."""

    def forward_bootstrap_integrations(message: dict[str, Any]) -> None:
        """Forward bootstrap integrations to websocket."""
        connection.send_message(messages.event_message(msg["id"], message))

```

from `﻿﻿﻿homeassistant/components/websocket_api/commands.py`


Stacktrace https://gist.github.com/MichaReiser/7727742b46821073b8a1a660238bec8e

Related to https://github.com/astral-sh/ty/issues/256

### Version

_No response_

---

_Label `fatal` added by @MichaReiser on 2025-08-15 11:40_

---

_Comment by @MichaReiser on 2025-08-15 11:40_

CC: @dcreager 

---

_Added to milestone `Beta` by @MichaReiser on 2025-08-15 11:41_

---

_Renamed from "Checking home-assistant-core stack overflows" to "Checking home-assistant-core stack overflows in `find_legacy_typevars`" by @MichaReiser on 2025-08-15 11:41_

---

_Comment by @MichaReiser on 2025-08-15 11:55_

Note: I wasn't sure if I should open this one or if it's still a known issue that we're stack overflowing in some cases but I couldn't find any issue specifically to stack overflows (the others mainly mention too many iterations). If this is tracked elsewhere, feel free to close without a commennt

---

_Assigned to @dcreager by @carljm on 2025-08-15 15:39_

---

_Comment by @MichaReiser on 2025-08-22 12:11_

@dcreager it seems home assistant passes now. Did you land a fix for this?

---

_Comment by @carljm on 2025-08-22 16:21_

There have been a few recent PRs that improved our coverage of type visitors that might have fixed this

---

_Closed by @carljm on 2025-08-22 16:22_

---
