```yaml
number: 15440
title: "feat: uv format from project root"
type: pull_request
state: merged
author: jorgehermo9
labels:
  - preview
assignees: []
merged: true
base: main
head: feature/uv-format-project-root
created_at: 2025-08-21T23:04:00Z
updated_at: 2025-08-22T20:09:47Z
url: https://github.com/astral-sh/uv/pull/15440
synced_at: 2026-01-12T16:11:44Z
```

# feat: uv format from project root

---

_@jorgehermo9_

Closes https://github.com/astral-sh/uv/issues/15430

This PR is a branch from https://github.com/astral-sh/uv/pull/15438, opening it as a draft until that is merged to main and I'll merge main with this branch to have a cleaner diff

---

_Review comment by @jorgehermo9 on `crates/uv/src/commands/project/format.rs`:43 on 2025-08-22 18:48_

Inspired from https://github.com/astral-sh/uv/blob/main/crates/uv/src/commands/project/export.rs#L108

---

_@jorgehermo9 reviewed on 2025-08-22 18:48_

---

_Review comment by @jorgehermo9 on `crates/uv/src/commands/project/format.rs`:66 on 2025-08-22 18:49_

I wonder how this implementation behaves for a workspace with multiple package. running just `uv format` would always run the format from the workspace root. What is the intended way of working with specific packages of a workspace?
using `uv run format --project package_a` and `uv run format --project package_b`?

---

_@jorgehermo9 reviewed on 2025-08-22 18:49_

---

_@jorgehermo9 reviewed on 2025-08-22 18:53_

---

_Review comment by @jorgehermo9 on `crates/uv/tests/it/format.rs`:84 on 2025-08-22 18:53_

Should we add another test similar to this, but using also the `--project` option? Maybe we could set up something like I depicted in https://github.com/astral-sh/uv/pull/15440/files#r2294435556 and have something like
```bash
.
├── main.py
├── package_a
│   ├── main.py
│   ├── pyproject.toml
│   └── README.md
├── package_b
│   ├── main.py
│   ├── pyproject.toml
│   └── README.md
├── pyproject.toml
└── README.md
```

and check that when running `uv run format --project package_a` only `package_a` gets formatted and `package_b` does not? Maybe this overlaps a bit with the current `format_relative_project` test so i'm not sure

Maybe I'm overthinking this :smile: but I'm open to include a test for this if this makes sense

---

_@jorgehermo9 reviewed on 2025-08-22 19:02_

---

_Review comment by @jorgehermo9 on `crates/uv/tests/it/format.rs`:84 on 2025-08-22 19:02_

And also, another test such as
```bash
.
├── main.py
├── package_a
│   ├── main.py
│   ├── package_a_subdir
│   ├── pyproject.toml
│   └── README.md
├── package_b
│   ├── main.py
│   ├── pyproject.toml
│   └── README.md
├── pyproject.toml
└── README.md
```
and runing `uv format` with current dir as `package_a/package_a_subdir` only formats `package_a` files? But this may be testing `VirtuslProject::discover`'s behaviour, so i'm also not sure

---

_Marked ready for review by @jorgehermo9 on 2025-08-22 19:07_

---

_@zanieb reviewed on 2025-08-22 19:08_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/format.rs`:66 on 2025-08-22 19:08_

I think if you wanted to run it in a workspace member it'd be `uv format --package <name>`

---

_Comment by @jorgehermo9 on 2025-08-22 19:08_

Marked PR as ready!

I see the CI is failing for windows https://github.com/astral-sh/uv/actions/runs/17163604154/job/48698604290?pr=15440#step:10:2995 but seems unrelated

---

_@zanieb reviewed on 2025-08-22 19:09_

---

_Review comment by @zanieb on `crates/uv/tests/it/format.rs`:84 on 2025-08-22 19:09_

It seems helpful to add a test case for a workspace if you're interested! Even if we don't handle workspace members "correctly" yet, having coverage for the behavior is helpful. 



---

_@jorgehermo9 reviewed on 2025-08-22 19:16_

---

_Review comment by @jorgehermo9 on `crates/uv/src/commands/project/format.rs`:66 on 2025-08-22 19:16_

Okay, perfect then! 

Thanks @zanieb, I'm kind of new with uv and I'm sorry for those basic questions

---

_@jorgehermo9 reviewed on 2025-08-22 19:18_

---

_Review comment by @jorgehermo9 on `crates/uv/tests/it/format.rs`:84 on 2025-08-22 19:18_

Okay! Going to add that later/tomorrow (CET timezone). Feel free to merge if you don't want to wait till that, I'll open a new PR for tests if you decide to merge this before

---

_@zanieb reviewed on 2025-08-22 19:21_

---

_Review comment by @zanieb on `crates/uv/tests/it/format.rs`:85 on 2025-08-22 19:21_


```suggestion
    // Using format from a subdirectory should still run in the project root
    uv_snapshot!(context.filters(), context.format().current_dir(&subdir), @r"
```

---

_@zanieb reviewed on 2025-08-22 19:21_

---

_Review comment by @zanieb on `crates/uv/tests/it/format.rs`:110 on 2025-08-22 19:21_

We should have a newline here
```suggestion
}

#[test]
```

---

_@zanieb reviewed on 2025-08-22 19:21_

---

_Review comment by @zanieb on `crates/uv/tests/it/format.rs`:59 on 2025-08-22 19:21_

We should also add a `--no-project`  test case. I'd expect that to _not_ use the project root.

---

_@zanieb reviewed on 2025-08-22 19:22_

---

_Review comment by @zanieb on `crates/uv/tests/it/format.rs`:59 on 2025-08-22 19:22_

We don't support that yet actually, so that's a follow-up

---

_@zanieb reviewed on 2025-08-22 19:22_

---

_Review comment by @zanieb on `crates/uv/tests/it/format.rs`:59 on 2025-08-22 19:22_

https://github.com/astral-sh/uv/issues/15462

---

_@jorgehermo9 reviewed on 2025-08-22 19:31_

---

_Review comment by @jorgehermo9 on `crates/uv/tests/it/format.rs`:59 on 2025-08-22 19:31_

I may implement that as well! if no one takes it first 😃 

---

_@zanieb reviewed on 2025-08-22 19:32_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/format.rs`:66 on 2025-08-22 19:32_

No problem!

---

_@zanieb approved on 2025-08-22 19:33_

This looks ready to go besides my nits.

---

_Comment by @jorgehermo9 on 2025-08-22 19:36_

Thanks for your review @zanieb!
Feel free to merge if you want, if so I'll open another with the remaining tests,otherwise I'll do those changes tomorrow 😀 

---

_Merged by @zanieb on 2025-08-22 20:09_

---

_Closed by @zanieb on 2025-08-22 20:09_

---

_Label `preview` added by @zanieb on 2025-08-22 20:09_

---
