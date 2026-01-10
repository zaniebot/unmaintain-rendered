---
number: 1887
title: "`discord.py`: Checking `audit_logs.py` takes very long"
type: issue
state: open
author: MichaReiser
labels:
  - performance
assignees: []
created_at: 2025-12-15T11:18:04Z
updated_at: 2026-01-09T00:01:56Z
url: https://github.com/astral-sh/ty/issues/1887
synced_at: 2026-01-10T01:48:23Z
---

# `discord.py`: Checking `audit_logs.py` takes very long

---

_Issue opened by @MichaReiser on 2025-12-15 11:18_

Checking `audit_logs.py` dominates the entire type-checking time:

* wall-time: 380ms
* [`audit_logs.py`](https://github.com/Rapptz/discord.py/blob/master/discord/audit_logs.py): `/home/micha/astral/discord.py/discord/audit_logs.py` took more than 100ms (340.341158ms)

I haven't looked into what's special about `audit_logs.py` (or something it imports), that explains its long type-checking time.


There are some other "slow" files but none takes as long as `audit_logs.py`:

```
INFO Checking file `/home/micha/astral/discord.py/discord/ext/commands/bot.py` took more than 100ms (104.770849ms)
INFO Checking file `/home/micha/astral/discord.py/discord/client.py` took more than 100ms (121.575732ms)
INFO Checking file `/home/micha/astral/discord.py/discord/http.py` took more than 100ms (117.107697ms)
INFO Checking file `/home/micha/astral/discord.py/discord/guild.py` took more than 100ms (124.385833ms)
INFO Checking file `/home/micha/astral/discord.py/discord/message.py` took more than 100ms (147.963634ms)
INFO Checking file `/home/micha/astral/discord.py/discord/state.py` took more than 100ms (103.881121ms)
```

---

_Label `performance` added by @MichaReiser on 2025-12-15 11:18_

---

_Comment by @MichaReiser on 2025-12-15 11:22_

We can also see this in profiles where most threads finish after 180ms, but then there's one thread that just keeps going 

https://share.firefox.dev/3KAw91E

---

_Comment by @MichaReiser on 2025-12-15 11:44_

We see the same pattern on macos and linux. It's more notable on my Linux machine because it has more cores, but each core has worse single-core performance compared to my mac

---

_Comment by @MichaReiser on 2025-12-15 17:15_

One thing that stands out to me is that we call `is_redundant_with` a lot... Like a ton:

- Total queries executed: 56,481
- is_redundant_with queries: 42,628
- Percentage: ~75.5% (42,628 / 56,481)

https://gist.github.com/MichaReiser/0ba51a38663ef7a9de13641a50d1719f

---

_Added to milestone `Pre-stable 1` by @carljm on 2026-01-09 00:01_

---
