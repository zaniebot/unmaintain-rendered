```yaml
number: 4560
title: "Move from a shared `tools.toml` to separated tool receipts"
type: pull_request
state: merged
author: zanieb
labels:
  - preview
assignees: []
merged: true
base: main
head: zb/tool-install-toml
created_at: 2024-06-26T18:05:03Z
updated_at: 2024-06-26T20:48:19Z
url: https://github.com/astral-sh/uv/pull/4560
synced_at: 2026-01-12T16:06:18Z
```

# Move from a shared `tools.toml` to separated tool receipts

---

_@zanieb_

Refactors the installed tool metadata per commentary in #4492 

We now store a `uv-receipt.toml` per tool install instead of a single `tools.toml` 

---

_Label `preview` added by @zanieb on 2024-06-26 18:05_

---

_Comment by @zanieb on 2024-06-26 18:07_

I'll build more work on top of this demonstrating the purpose of the tool metadata entries, since there are some open questions about that.

---

_Comment by @charliermarsh on 2024-06-26 18:57_

Is it a goal or an explicit non-goal to support installing multiple _versions_ of a given package?

---

_Comment by @zanieb on 2024-06-26 19:16_

Good question. I think if you want to do that then you need to provide a "suffix" to distinguish the entry points and we'd use that suffix in the tool name path. I'm kind of on the fence about that functionality to start.

---

_Comment by @charliermarsh on 2024-06-26 19:17_

In the initial design documents, I seem to recall something like `uv tool run black+22.0.0`. Or maybe that was for Python?

---

_Comment by @charliermarsh on 2024-06-26 19:20_

I think in cargo-dist they call this a "receipt", it's like a manifest that records what we installed. I might find terminology like that a little more intuitive than `tool.toml` which "feels" user-edited.

---

_Comment by @zanieb on 2024-06-26 19:27_

> In the initial design documents...

We talked about that but people weren't really into it. If you're using `uv tool run` there's no reason to install something though. It was the idea of an automatic suffix with `+<version>`.

> I think in cargo-dist they call this a "receipt"...

Down to call it that. As I mentioned before, I want to have a user-edited tool declaration eventually but am moving to this for now as it's clearer that it's _not_ supposed to be user edited.



---

_Comment by @zanieb on 2024-06-26 19:30_

For reference, `pipx` uses `pipx_metadata.json`:

```json
{
    "injected_packages": {},
    "main_package": {
        "app_paths": [
            {
                "__Path__": "/Users/zb/.local/pipx/venvs/black/bin/black",
                "__type__": "Path"
            },
            {
                "__Path__": "/Users/zb/.local/pipx/venvs/black/bin/blackd",
                "__type__": "Path"
            }
        ],
        "app_paths_of_dependencies": {},
        "apps": [
            "black",
            "blackd"
        ],
        "apps_of_dependencies": [],
        "include_apps": true,
        "include_dependencies": false,
        "man_pages": [],
        "man_pages_of_dependencies": [],
        "man_paths": [],
        "man_paths_of_dependencies": {},
        "package": "black",
        "package_or_url": "black",
        "package_version": "24.4.2",
        "pip_args": [],
        "suffix": ""
    },
    "pipx_metadata_version": "0.4",
    "python_version": "Python 3.12.3",
    "source_interpreter": {
        "__Path__": "/opt/homebrew/opt/python@3.12/libexec/bin/python",
        "__type__": "Path"
    },
    "venv_args": []
}% 
```

---

_Renamed from "Move from a shared `tools.toml` to separate `tool.toml` entries" to "Move from a shared `tools.toml` to separated tool receipts" by @zanieb on 2024-06-26 20:00_

---

_Marked ready for review by @zanieb on 2024-06-26 20:03_

---

_Review requested from @charliermarsh by @zanieb on 2024-06-26 20:13_

---

_Comment by @zanieb on 2024-06-26 20:27_

I guess it makes sense to write JSON directly? I don't have strong feelings about the format but am leaning towards just leaving it.

---

_Review requested from @konstin by @zanieb on 2024-06-26 20:29_

---

_@charliermarsh approved on 2024-06-26 20:48_

---

_Merged by @charliermarsh on 2024-06-26 20:48_

---

_Closed by @charliermarsh on 2024-06-26 20:48_

---

_Branch deleted on 2024-06-26 20:48_

---
