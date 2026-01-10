```yaml
number: 9636
title: "Add `uv tool list --outdated` to show outdated tools"
type: pull_request
state: closed
author: j178
labels: []
assignees: []
base: main
head: tool-outdated
created_at: 2024-12-04T11:28:57Z
updated_at: 2025-02-13T13:41:56Z
url: https://github.com/astral-sh/uv/pull/9636
synced_at: 2026-01-10T11:10:34Z
```

# Add `uv tool list --outdated` to show outdated tools

---

_Pull request opened by @j178 on 2024-12-04 11:28_

## Summary

Add `uv tool list --outdated`, similar to `uv tree --outdated` and `uv pip list --outdated`, to display only outdated tools.

```console
cargo run -- tool list --outdated
ipython v8.29.0 (latest: v8.30.0)
- ipython
- ipython3
maturin v1.7.4 (latest: v1.7.7)
- maturin
ruff v0.8.0 (latest: v0.8.1)
- ruff
yt-dlp v2024.11.18 (latest: v2024.12.3)
- yt-dlp
```

Closes #9309 

---

_Review requested from @zanieb by @konstin on 2024-12-04 11:33_

---

_@j178 reviewed on 2024-12-04 11:37_

---

_Review comment by @j178 on `crates/uv/src/commands/tool/list.rs`:321 on 2024-12-04 11:37_

Not quite sure how to address this error, this approach might be incorrect.

---

_Review requested from @charliermarsh by @zanieb on 2024-12-10 19:22_

---

_Comment by @zanieb on 2024-12-10 19:22_

@charliermarsh I think you have more context on the `--outdated` implementations — but let me know if you'd prefer that I review.

---

_Comment by @charliermarsh on 2024-12-10 19:25_

👍 No worries, I can take this.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-11 01:13_

---

_Comment by @j178 on 2025-01-23 08:11_

Hi @charliermarsh, could you take a look at this PR? Let me know if there’s anything I can do to help move it forward!

---

_Closed by @j178 on 2025-02-13 13:41_

---
