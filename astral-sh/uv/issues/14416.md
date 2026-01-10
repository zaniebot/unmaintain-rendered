```yaml
number: 14416
title: "Where is `uv tool install az` installed, and why it cannot be uninstalled?"
type: issue
state: closed
author: stdedos
labels:
  - question
assignees: []
created_at: 2025-07-02T10:21:34Z
updated_at: 2025-07-02T18:11:18Z
url: https://github.com/astral-sh/uv/issues/14416
synced_at: 2026-01-10T03:32:45Z
```

# Where is `uv tool install az` installed, and why it cannot be uninstalled?

---

_Issue opened by @stdedos on 2025-07-02 10:21_

### Question

```console
$ uv tool install az
Resolved 1 package in 341ms
Prepared 1 package in 56ms
Installed 1 package in 4ms
 + az==0.1.0.dev1
No executables are provided by `az`
$ uv tool uninstall az
error: `az` is not installed
```

### Platform

Ubuntu 24.04.2 Linux 6.8.0-62-generic x86_64 GNU/Linux

### Version

uv 0.7.15

---

_Label `question` added by @stdedos on 2025-07-02 10:21_

---

_Comment by @zanieb on 2025-07-02 14:22_

We remove it in that case

https://github.com/astral-sh/uv/blob/e9d5780369da14ecf9f834efc8a17b0118823cfb/crates/uv/src/commands/tool/common.rs#L218-L231

---

_Comment by @stdedos on 2025-07-02 15:05_

_I would close this_, since "all I wanted to know is" "wte is that `az` package?, ""is it infecting my computer"???"

BUT I'm very happy this is becoming a PR 😀 ❤ 

---

_Closed by @zanieb on 2025-07-02 18:11_

---
