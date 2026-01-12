```yaml
number: 3989
title: "`RET` selection is behaving incorrectly"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2023-04-16T23:15:05Z
updated_at: 2023-04-18T02:52:28Z
url: https://github.com/astral-sh/ruff/issues/3989
synced_at: 2026-01-12T15:54:44Z
```

# `RET` selection is behaving incorrectly

---

_@charliermarsh_

Given:

```py
from typing import Optional


def system_content(self) -> Optional[str]:
    if self.type is MessageType.role_subscription_purchase:
        if not (data := self.role_subscription_data):
            return

        guild_name = f"**{self.guild.name}**" if self.guild else None
        if data.total_months_subscribed > 0:
            action = "renewed" if data.is_renewal else "joined"
            return (
                f"{self.author.name} {action} **{data.tier_name}** and has been a subscriber "
                f"of {guild_name} for {data.total_months_subscribed} "
                f"{'month' if data.total_months_subscribed == 1 else 'months'}!"
            )
        elif data.is_renewal:
            return f"{self.author.name} renewed **{data.tier_name}** in their {guild_name} membership!"
        else:
            return f"{self.author.name} joined **{data.tier_name}** as a subscriber of {guild_name}!"

    return None
```

Running `ruff . --select RET505 --select RET502` raises `RET505`, but not `RET502`.

Reported on Discord.

---

_Label `bug` added by @charliermarsh on 2023-04-16 23:15_

---

_Comment by @charliermarsh on 2023-04-16 23:40_

This is consistent with `flake8-return`, but I think we should tweak the behavior. It's a little confusing.

---

_Comment by @charliermarsh on 2023-04-16 23:40_

Effectively, if we detect a `RET505` error, we abort, and skip some of the later checks (like `RET502`).

---

_Closed by @charliermarsh on 2023-04-18 02:52_

---
