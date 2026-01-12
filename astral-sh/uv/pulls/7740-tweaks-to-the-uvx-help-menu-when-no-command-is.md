```yaml
number: 7740
title: "Tweaks to the `uvx` help menu when no command is provided"
type: pull_request
state: merged
author: zanieb
labels:
  - cli
assignees: []
merged: true
base: main
head: zb/tool-help
created_at: 2024-09-27T15:11:30Z
updated_at: 2024-09-27T17:02:24Z
url: https://github.com/astral-sh/uv/pull/7740
synced_at: 2026-01-12T16:07:58Z
```

# Tweaks to the `uvx` help menu when no command is provided

---

_@zanieb_

Follow-up to #7641 with some minor changes to the implementation and a simplification of the output

---

_Label `cli` added by @zanieb on 2024-09-27 15:11_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:85 on 2024-09-27 15:12_

Handling this case with `?` instead.

---

_@zanieb reviewed on 2024-09-27 15:12_

---

_@zanieb reviewed on 2024-09-27 15:13_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:283 on 2024-09-27 15:13_

After playing with this a bit more, I was worried users would feel like they had to use `uv tool install` before they could use `uv tool run` which isn't the case so I dropped this message.

---

_@zanieb reviewed on 2024-09-27 15:13_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:294 on 2024-09-27 15:13_

Instead of using `sort_by_key` I use `sorted_by` to avoid the extra clone.

---

_@zanieb reviewed on 2024-09-27 15:14_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:305 on 2024-09-27 15:14_

I do all the filtering here, so we don't need to use a `buf: String` later.

---

_@zanieb reviewed on 2024-09-27 15:15_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:323 on 2024-09-27 15:15_

I dropped the entrypoints, mostly to simplify the output and avoid confusing around `--from` usage which is a bit more advanced and hard to spell here.

---

_Review comment by @zanieb on `crates/uv/tests/tool_run.rs`:783 on 2024-09-27 15:16_

I went back and forth on a newline here. I _think_ it looks better if we have an extra newline separating the list? Relevant for #7687 

---

_@zanieb reviewed on 2024-09-27 15:16_

---

_Review requested from @charliermarsh by @zanieb on 2024-09-27 15:16_

---

_Marked ready for review by @zanieb on 2024-09-27 15:16_

---

_Review comment by @kakkoyun on `crates/uv/src/commands/tool/run.rs`:85 on 2024-09-27 15:19_

Definitely, way cleaner 🙈 

---

_Review comment by @kakkoyun on `crates/uv/src/commands/tool/run.rs`:273 on 2024-09-27 15:19_

👍 

---

_Review comment by @kakkoyun on `crates/uv/src/commands/tool/run.rs`:283 on 2024-09-27 15:21_

👍 

---

_@kakkoyun approved on 2024-09-27 15:23_

LGTM. Thanks for cleaning things up and making the message much clearer.

These changes made me realize that I still write Rust as if I’m writing Go 😬

---

_@kakkoyun reviewed on 2024-09-27 15:34_

---

_Review comment by @kakkoyun on `crates/uv/tests/tool_run.rs`:783 on 2024-09-27 15:34_

The extra line looks way cleaner to me. If you're okay with me putting it here, I will change the other PR.

---

_@zanieb reviewed on 2024-09-27 15:48_

---

_Review comment by @zanieb on `crates/uv/tests/tool_run.rs`:783 on 2024-09-27 15:48_

👍 yeah sorry I think I (wrongly) suggested you remove it over there

---

_Merged by @charliermarsh on 2024-09-27 17:02_

---

_Closed by @charliermarsh on 2024-09-27 17:02_

---

_Branch deleted on 2024-09-27 17:02_

---
