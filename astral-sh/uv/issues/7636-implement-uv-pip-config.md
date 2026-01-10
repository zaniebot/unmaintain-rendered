---
number: 7636
title: implement uv pip config
type: issue
state: closed
author: woutervh
labels:
  - question
assignees: []
created_at: 2024-09-23T10:59:01Z
updated_at: 2024-10-28T19:25:30Z
url: https://github.com/astral-sh/uv/issues/7636
synced_at: 2026-01-10T01:24:17Z
---

# implement uv pip config

---

_Issue opened by @woutervh on 2024-09-23 10:59_

I've been searching in the uv docs and issue-tracker, but I did not find my answer.
Apologies if I overlooked something.


As a pip-user, I  regularly use  "pip config list" to display the config pip is actually seeing.
see https://pip.pypa.io/en/stable/cli/pip_config/

## Questions:
 -  is there currently a way to see what config uv is actually using (after merging several sources)?
 -  is there a plan to implement something like "pip config" 


# Usecase
I'm currently  troubleshooting why an (private) index-url in pyproject.toml does not have any effect:

```
[tool.uv]
# see https://docs.astral.sh/uv/reference/settings/
index-url = "https://__token__:<TOKEN>@gitlab.com/api/v4/groups/<GROUPID>/-/packages/pypi/simple"

```

> uv --version
0.4.15



---

_Comment by @agustinvalencia on 2024-09-23 12:01_

Similar case, I couldn't manage to set an self-hosted pypi repository. This pushed me back to poetry
Unfortunately I cannot share logs as it is company related, but back your issue up

---

_Comment by @zanieb on 2024-09-23 13:39_

We support showing your configuration with a `--show-settings` flag, e.g., `uv pip install foo --show-settings`.

We'll probably implement a nicer command for it in the future.

We're improving the index experience in #7481 which should help with this in general!


---

_Label `question` added by @zanieb on 2024-09-23 13:39_

---

_Closed by @zanieb on 2024-10-21 22:05_

---

_Comment by @wimglenn on 2024-10-28 19:23_

Hi @zanieb, could this use a serialization format for ease of use with a deserializer? Something like TOML or [RON](https://github.com/ron-rs/ron), perhaps? If I understand correctly, it's currently just [debug](https://doc.rust-lang.org/std/fmt/trait.Debug.html#stability) output with no stability guarantees.

---
