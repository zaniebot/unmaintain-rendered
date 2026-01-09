---
number: 2743
title: "uv pip compile: add optional --skip-if-newer (or similar) optimisation"
type: issue
state: open
author: bloodearnest
labels:
  - needs-decision
assignees: []
created_at: 2024-03-31T19:22:06Z
updated_at: 2024-04-01T14:44:56Z
url: https://github.com/astral-sh/uv/issues/2743
synced_at: 2026-01-07T13:12:17-06:00
---

# uv pip compile: add optional --skip-if-newer (or similar) optimisation

---

_Issue opened by @bloodearnest on 2024-03-31 19:22_

This a suggestion for a possible `uv pip compile` feature.

In our dev tooling, we check if the .txt file is newer than the corresponding .in file. If it is, we do not run the pip compile step.

This allows us to automatically run pip compile as part of our normal dev commands, as mostly its a no-op. However, if the .in file has changed, we automatically re-compile and re-install. This is very useful if merging or switching branches, as running tests will install any changed dependencies automatically. It also means you can just add a dependency in the .in, and the compile is automatic.

uv's pip compile is *much* faster, but in our testing, its not fast enough to avoid needing this check/skip step if we want to auto-compile.

This seems like a useful pattern, so I wonder if a `--skip-if-newer` or similar option would be worth considering, that implements this logic?

There's one wrinkle we've found with this system - it doesn't interact well with pre-commit's auto stashing/unstashing of changes, which modifies the mtimes, so it sometimes runs compile step when it doesn't need to.  We've used content hashes instead of mtime to work around this, but now you need another file *somewhere* to track the hash.

Just an idea, whilst I've been playing around with moving to uv. Thanks for all your work!



---

_Label `needs-decision` added by @charliermarsh on 2024-04-01 14:44_

---
