```yaml
number: 2935
title: "Implement `asyncio-dangling-task` to track `asyncio.create_task` calls"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/async
created_at: 2023-02-15T18:54:09Z
updated_at: 2023-02-16T14:05:57Z
url: https://github.com/astral-sh/ruff/pull/2935
synced_at: 2026-01-12T04:39:44Z
```

# Implement `asyncio-dangling-task` to track `asyncio.create_task` calls

---

_Pull request opened by @charliermarsh on 2023-02-15 18:54_

This rule guards against `asyncio.create_task` usages of the form:

```py
asyncio.create_task(coordinator.ws_connect())  # Error
```

...which can lead to unexpected bugs due to the lack of a strong reference to the created task. See Will McGugan's blog post for reference: https://textual.textualize.io/blog/2023/02/11/the-heisenbug-lurking-in-your-async-code/.

Note that we can't detect issues like:

```py
def f():
    # Stored as `task`, but never used...
    task = asyncio.create_task(coordinator.ws_connect())
```

So that would be a false negative. But this catches the common case of failing to assign the task in any way.

Closes #2809.


---

_Renamed from "Implement asyncio-dangling-task to track asyncio.create_task calls" to "Implement `asyncio-dangling-task` to track `asyncio.create_task` calls" by @charliermarsh on 2023-02-15 18:54_

---

_Comment by @charliermarsh on 2023-02-15 19:02_

@akx - I notice that `home-assistant/core` has a couple of these. If you have a sec -- do those look like false positives? Or do they look correct? Just trying to do some real-world evaluation prior to merging.

```
❯ cargo run ../core --select RUF006 -n
   Compiling ruff v0.0.247 (ruff/crates/ruff)
   Compiling ruff_cli v0.0.247 (ruff/crates/ruff_cli)
    Finished dev [unoptimized + debuginfo] target(s) in 9.11s
     Running `target/debug/ruff ../core --select RUF006 -n`
core/homeassistant/components/apple_tv/__init__.py:225:13: RUF006 `asyncio.create_task` call without a reference to the returned task
core/homeassistant/components/august/__init__.py:191:13: RUF006 `asyncio.create_task` call without a reference to the returned task
core/homeassistant/components/bluetooth_le_tracker/device_tracker.py:188:17: RUF006 `asyncio.create_task` call without a reference to the returned task
core/homeassistant/components/cast/media_player.py:191:9: RUF006 `asyncio.create_task` call without a reference to the returned task
core/homeassistant/components/crownstone/entry_manager.py:91:9: RUF006 `asyncio.create_task` call without a reference to the returned task
core/homeassistant/components/elkm1/__init__.py:190:5: RUF006 `asyncio.create_task` call without a reference to the returned task
core/homeassistant/components/google/calendar.py:475:9: RUF006 `asyncio.create_task` call without a reference to the returned task
core/homeassistant/components/homeassistant/__init__.py:157:13: RUF006 `asyncio.create_task` call without a reference to the returned task
core/homeassistant/components/homeassistant/__init__.py:180:13: RUF006 `asyncio.create_task` call without a reference to the returned task
core/homeassistant/components/homekit/type_cameras.py:446:13: RUF006 `asyncio.create_task` call without a reference to the returned task
core/homeassistant/components/insteon/__init__.py:175:5: RUF006 `asyncio.create_task` call without a reference to the returned task
core/homeassistant/components/livisi/__init__.py:44:5: RUF006 `asyncio.create_task` call without a reference to the returned task
core/homeassistant/components/mysensors/gateway.py:119:13: RUF006 `asyncio.create_task` call without a reference to the returned task
core/homeassistant/components/plum_lightpad/light.py:54:5: RUF006 `asyncio.create_task` call without a reference to the returned task
core/homeassistant/components/roon/server.py:69:9: RUF006 `asyncio.create_task` call without a reference to the returned task
core/homeassistant/components/sense/__init__.py:127:5: RUF006 `asyncio.create_task` call without a reference to the returned task
core/homeassistant/components/smart_meter_texas/__init__.py:81:5: RUF006 `asyncio.create_task` call without a reference to the returned task
core/homeassistant/components/sonos/__init__.py:481:9: RUF006 `asyncio.create_task` call without a reference to the returned task
core/homeassistant/components/squeezebox/media_player.py:173:5: RUF006 `asyncio.create_task` call without a reference to the returned task
core/homeassistant/components/squeezebox/media_player.py:206:9: RUF006 `asyncio.create_task` call without a reference to the returned task
core/homeassistant/components/unifiprotect/discovery.py:37:5: RUF006 `asyncio.create_task` call without a reference to the returned task
core/homeassistant/components/websocket_api/decorators.py:45:9: RUF006 `asyncio.create_task` call without a reference to the returned task
core/homeassistant/components/wiz/__init__.py:54:5: RUF006 `asyncio.create_task` call without a reference to the returned task
core/homeassistant/components/wled/coordinator.py:98:9: RUF006 `asyncio.create_task` call without a reference to the returned task
core/homeassistant/components/zeroconf/__init__.py:396:9: RUF006 `asyncio.create_task` call without a reference to the returned task
core/homeassistant/components/zha/core/channels/security.py:353:13: RUF006 `asyncio.create_task` call without a reference to the returned task
core/homeassistant/components/zha/core/gateway.py:256:9: RUF006 `asyncio.create_task` call without a reference to the returned task
core/homeassistant/components/zha/light.py:593:17: RUF006 `asyncio.create_task` call without a reference to the returned task
core/homeassistant/util/timeout.py:224:13: RUF006 `asyncio.create_task` call without a reference to the returned task
core/tests/test_runner.py:130:9: RUF006 `asyncio.create_task` call without a reference to the returned task
Found 30 errors.
```

---

_Comment by @akx on 2023-02-15 20:05_

Ooh, hey, @frenck, want to chip in on those home-assistant issues? By random sampling they look like they might be genuine bugs..?

---

_Comment by @frenck on 2023-02-15 20:09_

@charliermarsh We are aware, we started working on it actually. 
See: <https://github.com/home-assistant/core/pull/87970>

This rule would help us a lot to guard it btw, so it is more than welcome 👍 

@akx They are real issues 👍 

../Frenck

Edit: checked them, and it seems to be correct in its reporting 👍 

---

_Comment by @charliermarsh on 2023-02-15 20:18_

Awesome! Thank you for chiming in! Mostly just wanted to sanity-check that it wasn't totally off-base on real-world code :)

---

_Merged by @charliermarsh on 2023-02-15 20:19_

---

_Closed by @charliermarsh on 2023-02-15 20:19_

---

_Branch deleted on 2023-02-15 20:19_

---

_Comment by @frenck on 2023-02-15 20:24_

No problem, feel free to ping me in such cases. Thanks for the awesome project btw ❤️ 

PS: Congrats on merging this one, as it gets 2.5K commits on your main branch 😉 🎉 

![image](https://user-images.githubusercontent.com/195327/219146126-30c024dd-8ffe-42c5-bb76-1f2150b3577b.png)


---

_Comment by @charliermarsh on 2023-02-16 14:05_

@frenck - Thank you so much ❤️ I totally missed 2.5k, it's so cool to see the project come this far!

---
