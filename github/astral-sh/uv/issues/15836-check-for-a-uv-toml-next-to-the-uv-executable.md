---
number: 15836
title: "Check for a `uv.toml` next to the `uv` executable"
type: issue
state: open
author: zanieb
labels:
  - configuration
  - needs-decision
assignees: []
created_at: 2025-09-14T14:04:19Z
updated_at: 2025-09-14T15:01:26Z
url: https://github.com/astral-sh/uv/issues/15836
synced_at: 2026-01-07T13:12:19-06:00
---

# Check for a `uv.toml` next to the `uv` executable

---

_Issue opened by @zanieb on 2025-09-14 14:04_

Originally requested in https://github.com/astral-sh/uv/issues/11360

I think the idea here is that this would simplify configuring uv across multiple platforms?

I'm not sure where this would fit into our system -> user -> project configuration hierarchy.



---

_Label `configuration` added by @zanieb on 2025-09-14 14:04_

---

_Label `needs-decision` added by @zanieb on 2025-09-14 14:04_

---

_Comment by @konstin on 2025-09-14 14:13_

I would consider it a system level `uv.toml`, matching the request for portable installs  in https://github.com/astral-sh/uv/issues/11360 where the the directory of the uv install would be the system level. It also works for other linux-patterns, such as a `/opt/uv` install.

---

_Comment by @zanieb on 2025-09-14 14:20_

It needs to go either before or after the existing system configuration though.

---

_Comment by @zanieb on 2025-09-14 14:21_

@ma-shell can you share a bit more about your use-case here?

---

_Comment by @Ma-Shell on 2025-09-14 14:30_

The main intention here was for a portable installation. The main use case here is sharing python projects with non-IT folks. I would like to give users a folder containing my Python-scripts, the UV-Executable and the uv-configuration, along with a batch-file which simply contains `.\uv.exe run my_script.py`. When you mention "before or after the existing system configuration", I suppose you are referring to its priority. My suggestion would be, if a local `uv.toml` is found, for that to be the only considered configuration file but I would also be open to that one being merged with the system configuration

---

_Comment by @zanieb on 2025-09-14 14:53_

Ah interesting. You probably want something like https://github.com/astral-sh/uv/issues/5802 then, though this could be a good intermediate step.

---

_Comment by @Ma-Shell on 2025-09-14 15:01_

Yeah, definitely! A bundling solution would be absolutely amazing and as you mentioned, this is mainly a "workaround". But even using this workaround uv has been an incredible gamechanger considering how hard it was before to get Python Applications running on end-user PCs (unless you were using pyinstaller & co but even there I ran into trouble). For now I just did all the configuration of uv-environment variables in the accompanying bat-file which works but becomes nasty when I want to execute some other uv-commands, as I either have to change the bat-file or set the environment variables manually. This is also the reason, why in my original issue I combined it with the toml-extensions.

---

_Referenced in [astral-sh/uv#15751](../../astral-sh/uv/issues/15751.md) on 2025-09-16 02:02_

---
