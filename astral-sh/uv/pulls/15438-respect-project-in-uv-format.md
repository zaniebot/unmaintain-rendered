```yaml
number: 15438
title: "Respect `--project` in `uv format`"
type: pull_request
state: merged
author: jorgehermo9
labels:
  - preview
assignees: []
merged: true
base: main
head: feature/uv-format-project-dir
created_at: 2025-08-21T22:50:06Z
updated_at: 2025-08-22T16:27:52Z
url: https://github.com/astral-sh/uv/pull/15438
synced_at: 2026-01-10T06:44:33Z
```

# Respect `--project` in `uv format`

---

_Pull request opened by @jorgehermo9 on 2025-08-21 22:50_

Closes https://github.com/astral-sh/uv/issues/15431

This is my first contribution, so I'm sorry if I miss something! Didn't update any documentation nor added tests. Tell me if this is needed for this feature :smile: 

I will try to address https://github.com/astral-sh/uv/issues/15430 in another PR

---

_Comment by @zanieb on 2025-08-21 22:54_

Nice! The best way to test this is probably to copy the existing `format_project` and place the project in a subdirectory instead then add an unformatted file to the working directory and assert that it doesn't change but the one in the subdirectory does when you use `--project`.

---

_Label `preview` added by @zanieb on 2025-08-21 22:54_

---

_Comment by @zanieb on 2025-08-21 22:54_

(I haven't written the `uv format` documentation yet, so don't worry about that part)

---

_Review comment by @jorgehermo9 on `crates/uv/src/commands/project/format.rs`:61 on 2025-08-21 22:55_

I'm not sure if this is the expected behaviour...

Now `uv format --project . -- main.py`

would format both the main.py file two times - 1 time per this `project_dir` arg and another time for the extra arg for the `main.py` file...

Is it okay to **always** include all the files of the `project_dir`?

---

_@jorgehermo9 reviewed on 2025-08-21 22:55_

---

_@zanieb reviewed on 2025-08-21 22:55_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/format.rs`:61 on 2025-08-21 22:55_

I think we might just want to change the working directory for the subprocess to `project_dir` instead of passing it as an argument.

---

_Review comment by @zanieb on `crates/uv/src/commands/project/format.rs`:61 on 2025-08-21 22:58_

Reporting that the file is formatted twice is really weird. I'll look into fixing that upstream in Ruff.

---

_@zanieb reviewed on 2025-08-21 22:58_

---

_@zanieb reviewed on 2025-08-21 23:05_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/format.rs`:61 on 2025-08-21 23:05_

https://github.com/astral-sh/ruff/issues/20035

---

_@jorgehermo9 reviewed on 2025-08-21 23:07_

---

_Review comment by @jorgehermo9 on `crates/uv/src/commands/project/format.rs`:61 on 2025-08-21 23:07_

>I think we might just want to change the working directory for the subprocess to project_dir instead of passing it as an argument.

Oh, that will be better, yes. I thought about that, but didn't see any `ruff` option exposing it. It will be better to use `Command::current_dir`. Thanks!

---

_@jorgehermo9 reviewed on 2025-08-21 23:25_

---

_Review comment by @jorgehermo9 on `crates/uv/tests/it/format.rs`:59 on 2025-08-21 23:25_

Inspired in https://github.com/astral-sh/uv/blob/35a8dd514ef22d4c71cab430c74af2c1b95fbfe3/crates/uv/tests/it/lock.rs#L25729

I'm not sure if the `relative_project` nomenclature applies here.

Also, there are a lot of duplicated code among these tests (mainly, the same `pyproject.toml`, and the formatted/unformatted code snippets). I preferred to maintain the previous style and copypaste the mock data. I'm not sure what approach do you usually follow here (to extract into constants/fixtures vs always copypaste)

---

_Comment by @jorgehermo9 on 2025-08-21 23:28_

Everything addressed @zanieb! Thank you very much for your review 😃 

---

_@zanieb approved on 2025-08-22 16:25_

---

_@zanieb reviewed on 2025-08-22 16:25_

---

_Review comment by @zanieb on `crates/uv/tests/it/format.rs`:59 on 2025-08-22 16:25_

We could probably move them into a reusable snippet here. 

---

_@zanieb reviewed on 2025-08-22 16:27_

---

_Review comment by @zanieb on `crates/uv/tests/it/format.rs`:59 on 2025-08-22 16:27_

#15458

---

_Merged by @zanieb on 2025-08-22 16:27_

---

_Closed by @zanieb on 2025-08-22 16:27_

---

_Renamed from "feat: uv format should read project dir" to "Respect `--project` in `uv format`" by @zanieb on 2025-08-22 16:27_

---
