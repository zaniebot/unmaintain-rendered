---
number: 1738
title: "uv cache clean doesn't work"
type: issue
state: closed
author: sandipb
labels: []
assignees: []
created_at: 2024-02-20T08:05:16Z
updated_at: 2025-03-27T05:57:33Z
url: https://github.com/astral-sh/uv/issues/1738
synced_at: 2026-01-07T13:12:16-06:00
---

# uv cache clean doesn't work

---

_Issue opened by @sandipb on 2024-02-20 08:05_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
## Environment

`uv` installed via `pipx`.

```
$ uv --version
uv 0.1.5

$ uname -a
Darwin machine.local 23.3.0 Darwin Kernel Version 23.3.0: Wed Dec 20 21:33:31 PST 2023; root:xnu-10002.81.5~7/RELEASE_ARM64_T8112 arm64
```

## Problem

According to the README, to clear the cache, I should be running `uv cache clean`.

> To clear the global cache entirely, run uv cache clean.

But:

```shell-session
$ uv cache clean
error: unrecognized subcommand 'cache'

Usage: uv [OPTIONS] <COMMAND>

For more information, try '--help'.
```

---

_Comment by @hauntsaninja on 2024-02-20 08:29_

Wait for uv 0.1.6 to be released, in the meantime you can run `uv clean`

---

_Comment by @charliermarsh on 2024-02-20 14:53_

Ah sorry, that's a disconnect between the README (docs) and the latest published version.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-20 14:57_

---

_Closed by @charliermarsh on 2024-02-20 18:58_

---

_Comment by @rjn32s on 2025-03-26 04:24_

Thanks @hauntsaninja 


---

_Comment by @hauntsaninja on 2025-03-26 04:30_

Wow, why are you using uv 0.1, it's 2025!

---

_Comment by @sandipb on 2025-03-26 15:59_

> Wow, why are you using uv 0.1, it's 2025!

🤨 I created this ticket in Feb 2024 😀. 

---

_Comment by @rjn32s on 2025-03-27 05:57_

🥹 living in past

---
