```yaml
number: 7319
title: "FR: Do not update PATH on `uv self update`."
type: issue
state: closed
author: ilyagr
labels:
  - releases
assignees: []
created_at: 2024-09-12T01:36:55Z
updated_at: 2024-09-12T03:29:45Z
url: https://github.com/astral-sh/uv/issues/7319
synced_at: 2026-01-10T04:45:10Z
```

# FR: Do not update PATH on `uv self update`.

---

_Issue opened by @ilyagr on 2024-09-12 01:36_

https://docs.astral.sh/uv/getting-started/installation/#upgrading-uv currently explains that `uv self update` will attempt to update the user's PATH unless the environment variable `INSTALLER_NO_MODIFY_PATH=1` is set.

I think it shouldn't; if the user installed `uv` requesting that it doesn't modify the path, they probably don't expect it modified during an update. I suspect you're well aware of this, but I feel like having a thread for this that I and others could subscribe to might be nice.

It seems that https://docs.rs/axoupdater/latest/axoupdater does not support another behavior, but perhaps a feature could be added to it that sets `INSTALLER_NO_MODIFY_PATH=1` before it calls the installer script.

Thank you for resolving all the other related issues I previously reported, by the way! The speed with which this tool is evolving is impressive.

---

_Label `releases` added by @zanieb on 2024-09-12 02:17_

---

_Comment by @zanieb on 2024-09-12 02:23_

Related https://github.com/astral-sh/uv/issues/5576 — though I think that requests that we don't update it at all during updates and this requests that we respect the option that was provide at install time?

---

_Comment by @zanieb on 2024-09-12 02:24_

I think, generally, the shell profile files are changed on update because the new installer might have new target files.

---

_Comment by @ilyagr on 2024-09-12 03:27_

I think this is just a dupe of #5576. It makes the most sense to me if `self update` never updates the shell profile. If the user wanted them updated, the installer would have done it. If the user wants to update them again for some reason, this has nothing to do with updating the version of the tool, and they would probably just rerun the installer.

So, I'll close this as a duplicate. I also found a piece of info I'll post in #5576.

> I think, generally, the shell profile files are changed on update because the new installer might have new target files.

The new files should probably be placed in the same binary dirs, though.

---

_Closed by @ilyagr on 2024-09-12 03:29_

---
