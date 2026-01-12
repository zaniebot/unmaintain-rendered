```yaml
number: 5580
title: "Suppress resolver output by default in `uv run` and `uv tool run`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: charlie/run-output
created_at: 2024-07-30T00:01:25Z
updated_at: 2024-07-30T18:11:53Z
url: https://github.com/astral-sh/uv/pull/5580
synced_at: 2026-01-12T16:06:54Z
```

# Suppress resolver output by default in `uv run` and `uv tool run`

---

_@charliermarsh_

## Summary

The idea here is that we hide all resolver output (the grayed out resolver messages, plus the list of environment modifications) by default in `uv run` and `uv tool run`. We show the output under `-v`;  you can also pass `--show-resolution` to re-enable them, though that's intended to facilitate testing.

Closes https://github.com/astral-sh/uv/issues/5458.


---

_Marked ready for review by @charliermarsh on 2024-07-30 00:01_

---

_Label `cli` added by @charliermarsh on 2024-07-30 00:01_

---

_Label `preview` added by @charliermarsh on 2024-07-30 00:01_

---

_@charliermarsh reviewed on 2024-07-30 00:09_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:1909 on 2024-07-30 00:09_

Would love better ideas for this.

---

_@charliermarsh reviewed on 2024-07-30 02:26_

---

_Review comment by @charliermarsh on `crates/uv/tests/run.rs`:94 on 2024-07-30 02:26_

I considered just setting this as a default in `command.run()` for the test context, what do you think?

---

_@konstin reviewed on 2024-07-30 11:04_

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:1909 on 2024-07-30 11:04_

This feels like a case for `-v`

---

_@konstin approved on 2024-07-30 11:04_

---

_@zanieb reviewed on 2024-07-30 14:24_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:1909 on 2024-07-30 14:24_

Yeah I think just `-v` is okay? I guess `-v` includes more output though? Maybe it'll be better once we do https://github.com/astral-sh/uv/issues/1569?

---

_@zanieb reviewed on 2024-07-30 14:25_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:1909 on 2024-07-30 14:25_

I guess this is bad for the test cases though?

---

_@zanieb reviewed on 2024-07-30 14:25_

---

_Review comment by @zanieb on `crates/uv/tests/run.rs`:94 on 2024-07-30 14:25_

That'd be okay with me, I'd just document it.

---

_@zanieb reviewed on 2024-07-30 14:35_

---

_Review comment by @zanieb on `crates/uv/tests/run.rs`:94 on 2024-07-30 14:35_

One problem is that you can't drop the flag if you want to test without it.

---

_@charliermarsh reviewed on 2024-07-30 17:18_

---

_Review comment by @charliermarsh on `crates/uv/tests/run.rs`:94 on 2024-07-30 17:18_

Yeah that's why I "un-did" that approach. I could instead set is an env var and then you can `env_remove` like with `--exclude-newer`.

---

_@charliermarsh reviewed on 2024-07-30 17:18_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:1909 on 2024-07-30 17:18_

This is implied by `-v`, but I thought users might want it separate from the verbose output?

---

_Comment by @charliermarsh on 2024-07-30 17:42_

Ok, we're gonna make this a hidden CLI flag for now (to enable testing), but rely on `-v`.

---

_Merged by @charliermarsh on 2024-07-30 18:11_

---

_Closed by @charliermarsh on 2024-07-30 18:11_

---

_Branch deleted on 2024-07-30 18:11_

---
