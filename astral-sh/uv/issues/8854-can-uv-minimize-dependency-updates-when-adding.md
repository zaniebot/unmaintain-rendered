---
number: 8854
title: Can UV minimize dependency updates when adding new dependencies, updating only to the minimum required version?
type: issue
state: closed
author: TonyYanOnFire
labels: []
assignees: []
created_at: 2024-11-06T04:44:41Z
updated_at: 2024-11-07T02:36:38Z
url: https://github.com/astral-sh/uv/issues/8854
synced_at: 2026-01-10T01:24:33Z
---

# Can UV minimize dependency updates when adding new dependencies, updating only to the minimum required version?

---

_Issue opened by @TonyYanOnFire on 2024-11-06 04:44_

Scenario: Adding dependency foo, which requires bar >= 1.5.0. The latest version of bar is 1.9.0, but the project currently locks bar at version 1.0.0. In this case, pip will upgrade bar directly to 1.9.0. However, what i want is to upgrade bar only to 1.5.0. My goal is to minimize changes to the existing project dependencies. 

Can UV help achieve this?

---

_Comment by @FishAlchemist on 2024-11-06 09:45_

Would setting a minimum compatible resolution resolve your issue?
https://docs.astral.sh/uv/reference/settings/#resolution

By the way, this setting can also be configured via the command-line interface. If you only need a temporary change, you can simply specify the parameter.


---

_Comment by @TonyYanOnFire on 2024-11-07 02:36_

Thanks @FishAlchemist .  This feature is just what I'm looking for.

---

_Closed by @TonyYanOnFire on 2024-11-07 02:36_

---
