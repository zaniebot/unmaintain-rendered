```yaml
number: 5109
title: "uv-resolver: add TRACE dump of resolver output"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/trace-resolution
created_at: 2024-07-16T14:43:00Z
updated_at: 2024-07-16T16:51:57Z
url: https://github.com/astral-sh/uv/pull/5109
synced_at: 2026-01-12T16:06:38Z
```

# uv-resolver: add TRACE dump of resolver output

---

_@BurntSushi_

Specifically, this shows the resolution produced by the
resolver *before* constructing a resolution graph.

Unlike most trace messages, this is a multi-line message
that needs to do some small amount of work to build
itself. So we do an explicit gating on the log level here
instead of just relying on the `trace!` macro itself.

I added this as part of debugging #5086, and it seemed
like a useful thing to keep around. It could be a little
noisy, which is why I put it under the TRACE level, but
its output should be about the same as, say, the output
of `uv pip compile`.


---

_Review requested from @konstin by @BurntSushi on 2024-07-16 14:44_

---

_Review requested from @charliermarsh by @BurntSushi on 2024-07-16 14:44_

---

_Comment by @BurntSushi on 2024-07-16 14:44_

Here's an example output (from #5086):

```
[andrew@duff uv]$ RUST_LOG=trace cargo run -p uv -- pip compile requirements.in -p3.12 --universal -v 2>&1 | rg 'Resolution:'
TRACE Resolution: torchvision -> numpy
TRACE Resolution:     0.15.1+rocm5.4.2 -> 2.0.0
TRACE Resolution:     0.17.0+rocm5.7 -> 2.0.0
TRACE Resolution: requests -> idna
TRACE Resolution:     2.32.3 -> 3.7
TRACE Resolution: torch -> typing-extensions
TRACE Resolution:     2.0.0 -> 4.12.2
TRACE Resolution:     2.2.0 -> 4.12.2
TRACE Resolution: torchvision -> torch
TRACE Resolution:     0.17.0+rocm5.7 -> 2.2.0
TRACE Resolution:     0.15.1+rocm5.4.2 -> 2.0.0
TRACE Resolution: requests -> charset-normalizer
TRACE Resolution:     2.32.3 -> 3.3.2
TRACE Resolution: torchvision -> pillow
TRACE Resolution:     0.17.0+rocm5.7 -> 10.4.0
TRACE Resolution:     0.15.1+rocm5.4.2 -> 10.4.0
TRACE Resolution: torchvision -> requests
TRACE Resolution:     0.17.0+rocm5.7 -> 2.32.3
TRACE Resolution:     0.15.1+rocm5.4.2 -> 2.32.3
TRACE Resolution: sympy -> mpmath
TRACE Resolution:     1.13.0 -> 1.3.0
TRACE Resolution: ROOT -> torch
TRACE Resolution:     0a0.dev0 -> 2.0.0 ; platform_machine == 'x86_64'
TRACE Resolution:     0a0.dev0 -> 2.2.0 ; platform_machine != 'x86_64'
TRACE Resolution: requests -> certifi
TRACE Resolution:     2.32.3 -> 2024.7.4
TRACE Resolution: torch -> filelock
TRACE Resolution:     2.0.0 -> 3.15.4
TRACE Resolution:     2.2.0 -> 3.15.4
TRACE Resolution: torch -> sympy
TRACE Resolution:     2.2.0 -> 1.13.0
TRACE Resolution:     2.0.0 -> 1.13.0
TRACE Resolution: torch -> jinja2
TRACE Resolution:     2.2.0 -> 3.1.4
TRACE Resolution:     2.0.0 -> 3.1.4
TRACE Resolution: requests -> urllib3
TRACE Resolution:     2.32.3 -> 2.2.2
TRACE Resolution: ROOT -> torchvision
TRACE Resolution:     0a0.dev0 -> 0.15.1+rocm5.4.2
TRACE Resolution:     0a0.dev0 -> 0.17.0+rocm5.7
TRACE Resolution: torch -> networkx
TRACE Resolution:     2.0.0 -> 3.3
TRACE Resolution:     2.2.0 -> 3.3
TRACE Resolution: torch -> fsspec
TRACE Resolution:     2.2.0 -> 2024.6.1
TRACE Resolution: jinja2 -> markupsafe
TRACE Resolution:     3.1.4 -> 2.1.5
```

---

_@zanieb reviewed on 2024-07-16 14:48_

---

_Review comment by @zanieb on `crates/uv-resolver/src/resolver/mod.rs`:621 on 2024-07-16 14:48_

I feel like `tracing::enabled!` would be appropriate here

---

_@BurntSushi reviewed on 2024-07-16 14:51_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/mod.rs`:621 on 2024-07-16 14:51_

Yesssss thank you! I missed that macro and apparently did it the hard way. Lol. And it was already being used in this same module. -_-

---

_Comment by @zanieb on 2024-07-16 14:55_

I'm down for this, but want to let someone more focused on the resolver approve :)

---

_@ibraheemdev approved on 2024-07-16 16:51_

---

_Merged by @BurntSushi on 2024-07-16 16:51_

---

_Closed by @BurntSushi on 2024-07-16 16:51_

---

_Branch deleted on 2024-07-16 16:51_

---
