```yaml
number: 4973
title: "Add support for requirements files in `uv run`"
type: pull_request
state: merged
author: zanieb
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: zb/run-requirements
created_at: 2024-07-10T17:58:03Z
updated_at: 2024-07-23T16:51:11Z
url: https://github.com/astral-sh/uv/pull/4973
synced_at: 2026-01-12T16:06:33Z
```

# Add support for requirements files in `uv run`

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/4824.

---

_Label `cli` added by @zanieb on 2024-07-10 17:58_

---

_Label `preview` added by @zanieb on 2024-07-10 17:58_

---

_Comment by @zanieb on 2024-07-10 18:58_

Terrifyingly easy to add. I want to add some more test coverage and I'm sure there are some niche behaviors I need to explore.

---

_@zanieb reviewed on 2024-07-10 18:58_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:1792 on 2024-07-10 18:58_

Should we say `-r` / `--with-requirements` instead?

---

_@zanieb reviewed on 2024-07-16 17:38_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:66 on 2024-07-16 17:38_

Alternatively, `uv run -r` could imply `--isolated` (which might make sense but is more limiting?)

---

_@charliermarsh reviewed on 2024-07-17 01:26_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:66 on 2024-07-17 01:26_

Am I correct that the _code_ would work, and here, you just mean that semantically `-r foo/bar/pyproject.toml` _should_ use `foo/bar` as the project?

---

_@charliermarsh approved on 2024-07-17 01:27_

Seems fine (it's good that it was easy!). Let's also add to `uv tool run`? What about `uv tool install`...?

---

_Comment by @charliermarsh on 2024-07-17 02:33_

Also closes https://github.com/astral-sh/uv/issues/4824.

---

_@zanieb reviewed on 2024-07-17 14:55_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:66 on 2024-07-17 14:55_

Yeah the code should work as-is, it's just weird since it may _imply_ a change of project.

---

_@zanieb reviewed on 2024-07-17 14:56_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:66 on 2024-07-17 14:56_

Like.. you should use `--with ./foo/bar` instead, right?

---

_Comment by @zanieb on 2024-07-17 15:01_

Yeah I think we need it everywhere. What do you think about names?

- `--with-requirements <path>`
- `--with-requirements-file <path>`
- `--requirements-file <path>`
- `--requirements <path>`

I think the `--with` prefix makes sense since it's important to note it's "adding" requirements alongside whatever else?

Per https://github.com/astral-sh/uv/issues/4824#issuecomment-2211971741 I think we want to support `--override foo==1.0.0` directly so maybe the constraint and override options are:

- `--constraint <single>`
- `--override <single>`
- `--constraints <path>`
- `--overrides <path>`

Do we want:

- `--constraint-file` / `--override-file` or
- `--constraints-path` / `--overrides-path`?

Here I think it's reasonable to drop the `--with` prefix because we could apply these constraints and overrides to _all_ project dependencies not just those being added on top?

---

_Comment by @charliermarsh on 2024-07-17 15:05_

We don't accept `--override foo==1.0.0` in the `pip` API -- is it necessary to support here? It seems fine to me to be consistent with the `pip` API on those.

---

_Comment by @charliermarsh on 2024-07-17 15:05_

Or, at least, we definitely don't need to support that to start IMO. And if we want to support it, we should probably support it in the pip API too.

---

_Comment by @zanieb on 2024-07-17 15:10_

Yeah I don't think that's in scope for this pull request, just want to nail down the plan so we can make sure it's coherent.

---

_Comment by @charliermarsh on 2024-07-17 15:16_

I think I'd be ok with `--requirements`, `--overrides`, and `--constraints` (with the short options too) -- it just feels the most intuitive coming from the other APIs. `--with-*` is also ok, but we'd probably end up wanting aliases for the shorter versions?

---

_Comment by @charliermarsh on 2024-07-17 23:12_

@zanieb - I'm happy to finish this PR if we can get consensus on the names, no trouble.

---

_Comment by @zanieb on 2024-07-18 15:10_

@charliermarsh I am comfortable finishing it up this week

---

_Marked ready for review by @zanieb on 2024-07-19 18:05_

---

_@charliermarsh reviewed on 2024-07-20 01:10_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:313 on 2024-07-20 01:10_

Typo :D

---

_@charliermarsh reviewed on 2024-07-20 01:11_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:390 on 2024-07-20 01:11_

You might be able to simplify this with a `spec.filter(|spec| !spec.is_empty())` somewhere.

---

_@charliermarsh approved on 2024-07-20 01:11_

---

_Comment by @charliermarsh on 2024-07-23 13:45_

As discussed I'll take this over and try to get it merged today.

---

_Comment by @zanieb on 2024-07-23 16:10_

Note that we're using plural `--constraints`, `--requirements` and `--overrides` here to make room for the singular interface in the future. However, this differs from the `uv pip` interface which uses singular names to refer to files.

---

_Renamed from "Add support for requirements files, constraints, and overrides in `uv run`" to "Add support for requirements files in `uv run`" by @charliermarsh on 2024-07-23 16:30_

---

_Comment by @charliermarsh on 2024-07-23 16:31_

We decided to narrow the scope to requirements files for now. Constraints and overrides require that we resolve _alongside_ the base environment, diff the result, and install any changes into the ephemeral environment. Which is doable, but a lot more complex.


---

_@zanieb reviewed on 2024-07-23 16:33_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:1840 on 2024-07-23 16:33_

This seems dubious here, is it easy to disable?

---

_@charliermarsh reviewed on 2024-07-23 16:34_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:1840 on 2024-07-23 16:34_

I don't think so...

---

_@zanieb reviewed on 2024-07-23 16:35_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:320 on 2024-07-23 16:35_

This case can't occur now, right? Still worth including?

---

_@charliermarsh reviewed on 2024-07-23 16:35_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:1840 on 2024-07-23 16:35_

Checking

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:320 on 2024-07-23 16:36_

I figured it was worth it with a TODO. I can clarify more in the comments.

---

_@charliermarsh reviewed on 2024-07-23 16:36_

---

_@charliermarsh reviewed on 2024-07-23 16:43_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:1840 on 2024-07-23 16:43_

Fixed

---

_@charliermarsh reviewed on 2024-07-23 16:44_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:320 on 2024-07-23 16:44_

Removed

---

_@zanieb reviewed on 2024-07-23 16:45_

"Approved"

---

_Comment by @zanieb on 2024-07-23 16:45_

Thanks for taking this over!

---

_Merged by @charliermarsh on 2024-07-23 16:51_

---

_Closed by @charliermarsh on 2024-07-23 16:51_

---

_Branch deleted on 2024-07-23 16:51_

---
