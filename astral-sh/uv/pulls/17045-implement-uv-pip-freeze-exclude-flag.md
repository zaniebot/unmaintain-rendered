```yaml
number: 17045
title: "Implement `uv pip freeze --exclude` flag"
type: pull_request
state: merged
author: gaardhus
labels:
  - enhancement
assignees: []
merged: true
base: main
head: freeze-exclude
created_at: 2025-12-09T10:20:22Z
updated_at: 2026-01-17T15:34:32Z
url: https://github.com/astral-sh/uv/pull/17045
synced_at: 2026-01-17T16:15:32Z
```

# Implement `uv pip freeze --exclude` flag

---

_@gaardhus_

## Summary

Implements the `--exclude` flag to `uv pip freeze`, which allows to filter unwanted dependencies from the resulting requirements.txt file.

```bash
uv pip freeze --exclude pandas
```

part of https://github.com/astral-sh/uv/issues/3141


## Test Plan

Unit test with simple exclusion example of command argument(s)

---

_@zanieb reviewed on 2025-12-10 14:05_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip/freeze.rs`:89 on 2025-12-10 14:05_

Why do we do the `exclude.is_empty` check?

---

_@zanieb reviewed on 2025-12-10 14:05_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip/freeze.rs`:25 on 2025-12-10 14:05_

Should this be a hash set instead?

---

_Review comment by @zanieb on `crates/uv/src/settings.rs`:2755 on 2025-12-10 14:06_

I presume we'd transform to a hash set here

---

_@zanieb reviewed on 2025-12-10 14:06_

---

_Review comment by @zanieb on `crates/uv/src/commands/pip/freeze.rs`:25 on 2025-12-10 14:06_

https://github.com/astral-sh/uv/pull/17045/files#r2606798282

---

_@zanieb reviewed on 2025-12-10 14:06_

---

_@zanieb reviewed on 2025-12-10 14:07_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_freeze.rs`:590 on 2025-12-10 14:07_

I might just `pip install` these packages instead since it's simpler, and you don't need to pin versions because we use exclude-newer in the test suite.

---

_@gaardhus reviewed on 2025-12-11 08:39_

---

_Review comment by @gaardhus on `crates/uv/src/commands/pip/freeze.rs`:89 on 2025-12-11 08:39_

redundant indeed

---

_Review comment by @gaardhus on `crates/uv/tests/it/pip_freeze.rs`:590 on 2025-12-11 08:43_

changed to pip_install, and can also remove pins, however I just adopted that from the other test cases, so unsure how to move forward with that.

---

_@gaardhus reviewed on 2025-12-11 08:43_

---

_@gaardhus reviewed on 2025-12-11 08:49_

---

_Review comment by @gaardhus on `crates/uv/src/settings.rs`:2783 on 2025-12-11 08:49_

transform to hash set here seems to be how I can make it work? `exclude: exclude.into_iter().collect(),`

however, like pinning versions in pip_install test, I'm unsure if other similar cases should be changed, i.e. PipListSettings implements a similar exclude argument which is also currently a Vec.

@zanieb 

---

_@zanieb reviewed on 2025-12-12 16:47_

---

_Review comment by @zanieb on `crates/uv/src/settings.rs`:2783 on 2025-12-12 16:47_

We can pursue refactoring other cases separately, want to just open an issue to track that?

---

_@zanieb reviewed on 2025-12-12 16:48_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_freeze.rs`:591 on 2025-12-12 16:48_

I think you should drop the requirements.txt entirely and just use `MarkupSafe` and `ntomli` as arguments here.

---

_@zanieb reviewed on 2025-12-12 16:49_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_freeze.rs`:590 on 2025-12-12 16:49_

As in https://github.com/astral-sh/uv/pull/17045/files#r2614916235 we can just open an issue to simplify some of the test cases.

---

_Comment by @gaardhus on 2026-01-17 09:12_

@zanieb Sorry for the delayed response. I have rebased on main and created two issues, let me know if anything else is missing.

---

_@zanieb approved on 2026-01-17 15:34_

---

_Comment by @zanieb on 2026-01-17 15:34_

Thanks!

---

_Label `enhancement` added by @zanieb on 2026-01-17 15:34_

---

_Merged by @zanieb on 2026-01-17 15:34_

---

_Closed by @zanieb on 2026-01-17 15:34_

---
