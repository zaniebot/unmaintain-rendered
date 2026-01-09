---
number: 14917
title: Pull in upstream patches for nushell activation script
type: issue
state: closed
author: zanieb
labels:
  - help wanted
  - compatibility
assignees: []
created_at: 2025-07-26T19:32:41Z
updated_at: 2025-09-05T21:12:50Z
url: https://github.com/astral-sh/uv/issues/14917
synced_at: 2026-01-07T13:12:19-06:00
---

# Pull in upstream patches for nushell activation script

---

_Issue opened by @zanieb on 2025-07-26 19:32_

> Thanks IIRC uv used to upstream from the virtualenv one that also got fixed: https://github.com/pypa/virtualenv/commit/5e875b3b89ff5b7c11c6e7b6cdcd688009e1a620

_Originally posted by @melMass in https://github.com/astral-sh/uv/issues/14914#issuecomment-3122156446_


See

- https://github.com/pypa/virtualenv/pull/2910
- #14888 

---

_Label `help wanted` added by @zanieb on 2025-07-26 19:33_

---

_Label `compatibility` added by @zanieb on 2025-07-26 19:33_

---

_Comment by @2bndy5 on 2025-08-14 02:54_

Upstream `virtualenv` has already released their fix for this (as of [v20.33.0](https://github.com/pypa/virtualenv/releases/tag/20.33.0))

@zanieb I see the "help wanted" label here and in #14888 . How can I help?

---

_Comment by @zanieb on 2025-08-14 04:16_

Thanks for offerring!

I think someone just needs to validate and pull in those upstream changes into our virtual environment activation scripts. Ideally, we'd have some test coverage that makes it apparent we're resolving the problem.

---

_Comment by @2bndy5 on 2025-08-14 05:51_

Ok, I'm looking into it. FWIW, I'll be pulling in changes from upstream latest stable release (v20.34.0 at time of writing).

As for the test idea, ~it may be difficult/complex to capture stderr from rust unit test(s). Maybe I can utilize [`std::process::Command`](https://doc.rust-lang.org/std/process/struct.Command.html) to ensure the script(s) execute without problems. But this would involve adjusting the CI to ensure certain tools (eg various third-party shells) are installed for certain platforms, and the CI already looks very complex for a newcomer; I'm no stranger to GitHub Actions, though I do find the .github/workflows/setup-dev-drive.ps1 script rather foreign.~

EDIT: I see now that integration tests are written directly in the CI.yml workflow (an overwhelmingly long file). While unconventional per rust standards, this makes writing integration tests easier. I'll look at adding tests for each _supported_ shell.

---

_Referenced in [astral-sh/uv#15272](../../astral-sh/uv/pulls/15272.md) on 2025-08-14 08:54_

---

_Referenced in [astral-sh/uv#15294](../../astral-sh/uv/issues/15294.md) on 2025-08-15 01:45_

---

_Closed by @zanieb on 2025-09-05 21:12_

---
