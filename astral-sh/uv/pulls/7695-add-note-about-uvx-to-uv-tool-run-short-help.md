```yaml
number: 7695
title: "Add note about `uvx` to `uv tool run` short help"
type: pull_request
state: merged
author: JacobCoffee
labels:
  - cli
assignees: []
merged: true
base: main
head: main
created_at: 2024-09-26T04:18:36Z
updated_at: 2024-10-08T21:43:51Z
url: https://github.com/astral-sh/uv/pull/7695
synced_at: 2026-01-12T16:07:57Z
```

# Add note about `uvx` to `uv tool run` short help

---

_@JacobCoffee_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
- Adds detail to the `uv tool run --help` CLI  that lets users know about the shorthand, `uvx`

## Test Plan

<!-- How was it tested? -->
Building and running CLI
```
…📝✓] via 🐋 orbstack via 🎁 v0.4.16 via  pyenv via ⚙️ v1.81.0on ☁️  (us-east-2) took 3s 
➜ ./target/debug/uv tool run --help
Run a command provided by a Python package. Also available via the alias `uvx`.

Usage: uv tool run [OPTIONS] [COMMAND]

...

You can also use `uvx` as an alias for `uv tool run`. 
Use `uv help tool run` for more details.
```

---

_@zanieb reviewed on 2024-09-26 12:18_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:3179 on 2024-09-26 12:18_

Oh there's already a comment for this above in the long help i.e. `uv help tool run`. I'm a bit hesitant to add it to the short help? 

---

_@JacobCoffee reviewed on 2024-09-27 03:17_

---

_Review comment by @JacobCoffee on `crates/uv-cli/src/lib.rs`:3179 on 2024-09-27 03:17_

Sorry! Do you mean `Run a command provided by a Python package` or just that we don't need a second short note about `uvx` being available in the short help? If so feel free to close :D

---

_@zanieb reviewed on 2024-09-30 14:23_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:3179 on 2024-09-30 14:23_

`uv help tool run` already includes 

> `uvx` is provided as a convenient alias for `uv tool run`, their behavior is identical.

So we'd have a duplicate message here which feels weird.

---

_@zanieb reviewed on 2024-09-30 14:24_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:3179 on 2024-09-30 14:24_

I think if anything we'd use just `after_help` and not modify the `about`? You'd need to set `after_long_help` to an empty string.

---

_Converted to draft by @zanieb on 2024-10-01 19:38_

---

_Review comment by @JacobCoffee on `crates/uv-cli/src/lib.rs`:3179 on 2024-10-08 21:06_

Maybe [882b4d3](https://github.com/astral-sh/uv/pull/7695/commits/882b4d395d1d905fa19c5851f7b46260605217d8)?

---

_@JacobCoffee reviewed on 2024-10-08 21:06_

---

_Review requested from @zanieb by @JacobCoffee on 2024-10-08 21:06_

---

_Marked ready for review by @zanieb on 2024-10-08 21:37_

---

_@zanieb approved on 2024-10-08 21:37_

Thanks! I futzed with this a bit, not in love but can't find anything better.

---

_Label `cli` added by @zanieb on 2024-10-08 21:37_

---

_Renamed from "docs: add alias context to `uv tool run --help` command" to "Add note about `uvx` to `uv tool run` short help" by @zanieb on 2024-10-08 21:38_

---

_Merged by @zanieb on 2024-10-08 21:43_

---

_Closed by @zanieb on 2024-10-08 21:43_

---
