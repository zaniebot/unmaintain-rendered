```yaml
number: 4769
title: "Respect `XDG_DATA_HOME` on macOS if set"
type: pull_request
state: closed
author: zanieb
labels:
  - configuration
  - preview
assignees: []
draft: true
base: main
head: zb/state-xdg
created_at: 2024-07-03T13:50:35Z
updated_at: 2024-08-06T01:50:30Z
url: https://github.com/astral-sh/uv/pull/4769
synced_at: 2026-01-12T16:06:26Z
```

# Respect `XDG_DATA_HOME` on macOS if set

---

_@zanieb_

Rather than using `Application Support` for state storage, we use `XDG_DATA_HOME` _if_ it is set. This feels like a reasonable compromise respecting the OS defaults while allowing users to opt-in to XDG.

Closes #4411 

e.g. 

```
❯ export XDG_DATA_HOME="$HOME/.local/share"
❯ cargo run -- python install
Looking for installation Python 3.12.1 (any-3.12.1-any-any-any)
Looking for installation Python 3.11.7 (any-3.11.7-any-any-any)
Looking for installation Python 3.10.13 (any-3.10.13-any-any-any)
Looking for installation Python 3.9.18 (any-3.9.18-any-any-any)
Looking for installation Python 3.8.18 (any-3.8.18-any-any-any)
Looking for installation Python 3.8.12 (any-3.8.12-any-any-any)
Found 6/6 installations requiring installation
Downloading cpython-3.12.1-macos-aarch64-none
Downloading cpython-3.11.7-macos-aarch64-none
Downloading cpython-3.10.13-macos-aarch64-none
Downloading cpython-3.9.18-macos-aarch64-none
Installed Python 3.12.1 to /Users/zb/.local/share/uv/python/cpython-3.12.1-macos-aarch64-none
Installed Python 3.11.7 to /Users/zb/.local/share/uv/python/cpython-3.11.7-macos-aarch64-none
Installed Python 3.10.13 to /Users/zb/.local/share/uv/python/cpython-3.10.13-macos-aarch64-none
Installed Python 3.9.18 to /Users/zb/.local/share/uv/python/cpython-3.9.18-macos-aarch64-none
Downloading cpython-3.8.18-macos-aarch64-none
Downloading cpython-3.8.12-macos-aarch64-none
Installed Python 3.8.18 to /Users/zb/.local/share/uv/python/cpython-3.8.18-macos-aarch64-none
Installed Python 3.8.12 to /Users/zb/.local/share/uv/python/cpython-3.8.12-macos-aarch64-none
Installed 6 installations in 8s
```

---

_Label `configuration` added by @zanieb on 2024-07-03 13:50_

---

_Label `preview` added by @zanieb on 2024-07-03 13:50_

---

_Comment by @konstin on 2024-07-03 13:54_

In ruff, we're using `etcetera` for xdg directories, is there a reason not to do this in uv, too?

---

_Comment by @zanieb on 2024-07-03 13:57_

I'm just following the patterns we're using in our other crates here. I don't have context on why not `etcetera`, cc @charliermarsh. We could change separately from this if we want.

---

_Comment by @zanieb on 2024-07-03 14:00_

Since we're not a GUI application I'm basically down to just move over to XDG entirely too btw.

---

_Comment by @charliermarsh on 2024-08-05 03:22_

I'm planning to move us to XDG entirely for macOS (maybe not for the cache for now, though). How much do we care about backwards-compatibility here given that it's preview-only?

If we make it backwards-compatible, we may need to "never upgrade" existing users (since we'd just detect if `/Users/crmarsh/Library/Application Support/uv` exists and use that). Or we could support both (read-only from `/Users/crmarsh/Library/Application Support/uv`, read/write to XDG), but it's more work of course.


---

_Comment by @zanieb on 2024-08-05 13:42_

You could just move it if we find it in the old location? That's what I did when I renamed the Python directory. We could make a link?

---

_Comment by @charliermarsh on 2024-08-05 13:50_

Hmm, will there be any absolute references to things in the existing folder? Like paths to interpreters? Not sure...

---

_Comment by @zanieb on 2024-08-05 13:55_

Fair, that could be problematic. I guess we "never upgrade" and provide instructions for manual migration?

---

_Comment by @charliermarsh on 2024-08-05 13:57_

Tragic!

---

_Comment by @charliermarsh on 2024-08-06 01:50_

Continuing on in https://github.com/astral-sh/uv/pull/5806.

---

_Closed by @charliermarsh on 2024-08-06 01:50_

---
