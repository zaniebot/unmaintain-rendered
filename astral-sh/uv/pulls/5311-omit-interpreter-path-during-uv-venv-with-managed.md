```yaml
number: 5311
title: "Omit interpreter path during `uv venv` with managed Python"
type: pull_request
state: merged
author: zanieb
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: zb/venv-managed
created_at: 2024-07-22T21:08:11Z
updated_at: 2024-07-23T19:20:25Z
url: https://github.com/astral-sh/uv/pull/5311
synced_at: 2026-01-12T16:06:45Z
```

# Omit interpreter path during `uv venv` with managed Python

---

_@zanieb_

e.g. 
```
❯ cargo run -q -- venv --preview
Using Python 3.12.1
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
```

instead of 

```
❯ cargo run -q -- venv --preview
Using Python 3.12.1 interpreter at: /Users/zb/Library/Application Support/uv/python/cpython-3.12.1-macos-aarch64-none/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
```

---

_Label `cli` added by @zanieb on 2024-07-22 21:08_

---

_Label `preview` added by @zanieb on 2024-07-22 21:08_

---

_Comment by @zanieb on 2024-07-22 21:09_

I went back and forth on saying "Using managed Python XXX" but am leaning no.

There will be follow-ups to do this elsewhere.

---

_@zanieb reviewed on 2024-07-22 21:13_

---

_Review comment by @zanieb on `crates/uv/src/commands/venv.rs`:155 on 2024-07-22 21:13_

Alternatively we could check if this is a child of the managed Python directory but that seems more brittle.

---

_Comment by @zanieb on 2024-07-22 21:16_

I think there should be no test changes here since we find all the interpreters on the PATH during tests — if a managed interpreter is found on the PATH instead of via a managed Python directory scan we'll still show the path as before.

---

_Marked ready for review by @zanieb on 2024-07-23 16:51_

---

_Review requested from @charliermarsh by @zanieb on 2024-07-23 16:51_

---

_Review requested from @BurntSushi by @zanieb on 2024-07-23 16:51_

---

_@BurntSushi approved on 2024-07-23 18:47_

At first I was sad that the path to the interpreter was going away, but then I realized that it was only going away in the case of managed Python (which, of course, you said, but it didn't register for me). And then I was happy again.

I like this!

---

_Comment by @zanieb on 2024-07-23 18:56_

:) I could have included that in the pull request body too — a little lazy of me.

Here's the verbose output fwiw:

```
❯ cargo run -q -- venv -v --preview
DEBUG uv 0.2.27
DEBUG Reading requests from `.python-versions`
DEBUG Using the first version from `.python-versions`
DEBUG Searching for Python 3.12.1 in managed installations or system path
DEBUG Searching for managed installations at `/Users/zb/Library/Application Support/uv/python`
DEBUG Found managed Python `cpython-3.12.1-macos-aarch64-none`
DEBUG Found cpython 3.12.1 at `/Users/zb/Library/Application Support/uv/python/cpython-3.12.1-macos-aarch64-none/bin/python3` (managed installations)
Using Python 3.12.1
Creating virtualenv at: .venv
INFO Removing existing directory
Activate with: source .venv/bin/activate
```

---

_Comment by @BurntSushi on 2024-07-23 18:58_

That's awesome. (I have so many ancient scars from using a different `python` than I thought I was using.)

---

_Merged by @zanieb on 2024-07-23 19:20_

---

_Closed by @zanieb on 2024-07-23 19:20_

---

_Branch deleted on 2024-07-23 19:20_

---
