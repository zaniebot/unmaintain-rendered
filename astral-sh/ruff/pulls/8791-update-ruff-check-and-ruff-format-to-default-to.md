```yaml
number: 8791
title: "Update `ruff check` and `ruff format` to default to the current directory"
type: pull_request
state: merged
author: zanieb
labels:
  - cli
assignees: []
merged: true
base: main
head: zanie/default
created_at: 2023-11-20T16:35:58Z
updated_at: 2023-11-21T17:34:23Z
url: https://github.com/astral-sh/ruff/pull/8791
synced_at: 2026-01-10T23:40:55Z
```

# Update `ruff check` and `ruff format` to default to the current directory

---

_Pull request opened by @zanieb on 2023-11-20 16:35_

Closes https://github.com/astral-sh/ruff/issues/7347
Closes #3970 via use of `include`

We could update examples in our documentation, but I worry since we do not have versioned documentation users on older versions would be confused. Instead, I'll open an issue to track updating use of `ruff check .` in the documentation sometime in the future.

---

_Label `cli` added by @zanieb on 2023-11-20 16:35_

---

_Comment by @zanieb on 2023-11-20 16:41_

I'm looking into the behavior this exhibits with `include` per https://github.com/astral-sh/ruff/issues/3970 — it looks like more changes will be needed to properly support the desired behavior there.

---

_@zanieb reviewed on 2023-11-20 17:17_

---

_Review comment by @zanieb on `crates/ruff_cli/tests/integration_test.rs`:601 on 2023-11-20 17:17_

I'm not enthused about this change. I'm unsure how best to fix it. I'm not sure I want to deal with changing the default for `files` later so we can detect this case but it also feels kind of wrong to just avoid raising the warning if the file is `.`.

Source: https://github.com/astral-sh/ruff/blob/7b4b0045067b8eb41d0b03d9c3549ce3595373fa/crates/ruff_cli/src/lib.rs#L87-L95

---

_@zanieb reviewed on 2023-11-20 17:19_

---

_Review comment by @zanieb on `crates/ruff_cli/tests/integration_test.rs`:601 on 2023-11-20 17:19_

:/ bde7712ad

---

_Marked ready for review by @zanieb on 2023-11-20 17:22_

---

_@zanieb reviewed on 2023-11-20 17:59_

---

_Review comment by @zanieb on `docs/configuration.md`:341 on 2023-11-20 17:59_

This really feels like it should not be the case. We should join our default inclusions to directories. I'd prefer to tackle that separately though.

---

_Comment by @github-actions[bot] on 2023-11-20 18:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@konstin approved on 2023-11-20 20:41_

:tada: 

---

_Review requested from @charliermarsh by @zanieb on 2023-11-20 21:13_

---

_Review comment by @charliermarsh on `crates/ruff_cli/resources/test/fixtures/include-test/pyproject.toml`:2 on 2023-11-21 01:24_

Nit: do you mind adding a newline here, so it isn't automatically added next time someone edits the file?

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/lib.rs`:91 on 2023-11-21 01:26_

Does this mean we now allow `cat foo.py | ruff check --stdin-filename="foo.py" .`, without warning? That feels like a bug. I'm wondering if we should instead represent this default as the empty slice? I.e., at the relevant sites, if `files` is empty, use the current working directory?

---

_Review comment by @charliermarsh on `docs/configuration.md`:341 on 2023-11-21 01:28_

What would be examples of use-cases in which one would want to include an entire directory?

---

_@charliermarsh reviewed on 2023-11-21 01:28_

---

_@zanieb reviewed on 2023-11-21 01:50_

---

_Review comment by @zanieb on `docs/configuration.md`:341 on 2023-11-21 01:50_

As in #3970 e.g. `ruff check` with `include = ["src", "tests"]`

---

_@zanieb reviewed on 2023-11-21 01:52_

---

_Review comment by @zanieb on `crates/ruff_cli/src/lib.rs`:91 on 2023-11-21 01:52_

Yeah... I brought this up at https://github.com/astral-sh/ruff/pull/8791#discussion_r1399516759

I don't like either solution honestly. `.` as the Clap default is nice for the CLI help menu and such and it seems like a hassle to make sure the behavior is correct in the call sites. It does feel bad to omit the warning though so I guess I'll do the extra work.

---

_@charliermarsh reviewed on 2023-11-21 11:39_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/lib.rs`:91 on 2023-11-21 11:39_

Ahh sorry, I missed your comment! I defer to you as the author, I see there are tradeoffs here.

---

_@zanieb reviewed on 2023-11-21 16:08_

---

_Review comment by @zanieb on `crates/ruff_cli/src/lib.rs`:91 on 2023-11-21 16:08_

Okay kind of awkward but still fine https://github.com/astral-sh/ruff/pull/8791/commits/e92000d9064c9c3638124ea94d1091065c8e9e3e

---

_Merged by @zanieb on 2023-11-21 17:34_

---

_Closed by @zanieb on 2023-11-21 17:34_

---

_Branch deleted on 2023-11-21 17:34_

---
