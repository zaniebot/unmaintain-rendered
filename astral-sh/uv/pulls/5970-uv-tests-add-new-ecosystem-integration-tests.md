```yaml
number: 5970
title: "uv/tests: add new 'ecosystem' integration tests"
type: pull_request
state: merged
author: BurntSushi
labels:
  - testing
assignees: []
merged: true
base: main
head: ag/ecosystem
created_at: 2024-08-09T17:59:19Z
updated_at: 2024-08-13T13:48:02Z
url: https://github.com/astral-sh/uv/pull/5970
synced_at: 2026-01-10T13:31:54Z
```

# uv/tests: add new 'ecosystem' integration tests

---

_Pull request opened by @BurntSushi on 2024-08-09 17:59_

At a high level, this PR adds a smattering of new tests that
effectively snapshot the output of `uv lock` for a selection of
"ecosystem" projects. That is, real Python projects for which we expect
`uv` to work well with.

The main idea with these tests is to get a better idea of how changes
in `uv` impact the lock files of real world projects. For example,
we're hoping that these tests will help give us data for how #5733
differs from #5887.

This has already revealed some bugs. Namely, re-running `uv lock` for a
second time will produce a different lock file for some projects. So to
prioritize getting the tests added, for those projects, we don't do the
deterministic checking.


---

_Review requested from @konstin by @BurntSushi on 2024-08-09 17:59_

---

_Review requested from @charliermarsh by @BurntSushi on 2024-08-09 17:59_

---

_Review requested from @zanieb by @BurntSushi on 2024-08-09 17:59_

---

_@charliermarsh approved on 2024-08-09 18:18_

Great idea.

---

_Comment by @zanieb on 2024-08-09 18:20_

Could we note in a README or something what commits these were pulled from for each project?

---

_@zanieb approved on 2024-08-09 18:20_

---

_Comment by @BurntSushi on 2024-08-09 19:05_

I marked the `warehouse` and `transformers` tests to run on Linux only. The former because it seems to rely on running `pg_config` (so I'm not sure how one would produce a universal resolution on, say, Windows) and the latter because it times out on macOS/Windows.

---

_Comment by @konstin on 2024-08-09 19:14_

I love having ecosystem test for the universal resolver!

Are there options to trim them down? 30k lines is a lot.

---

_Comment by @zanieb on 2024-08-09 19:18_

Alternatively, we could have them in a separate repository and test them in a separate workflow than `cargo test`?

---

_Comment by @BurntSushi on 2024-08-09 20:54_

> I love having ecosystem test for the universal resolver!
> 
> Are there options to trim them down? 30k lines is a lot.

I can't think of one. You could compress them, but then you inhibit human readability.

@zanieb I'm not a huge fan of having them in a separate repo if we can avoid it, mostly because I think it's a big pain to have tests hosted elsewhere. Then any updates need multiple PRs. And we're already doing that for packse scenarios.

---

_Label `testing` added by @BurntSushi on 2024-08-13 13:47_

---

_Merged by @BurntSushi on 2024-08-13 13:48_

---

_Closed by @BurntSushi on 2024-08-13 13:48_

---

_Branch deleted on 2024-08-13 13:48_

---
