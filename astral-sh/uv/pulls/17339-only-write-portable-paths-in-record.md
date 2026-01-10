```yaml
number: 17339
title: Only write portable paths in RECORD
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
assignees: []
merged: true
base: main
head: konsti/record-portable-path
created_at: 2026-01-06T20:26:19Z
updated_at: 2026-01-07T19:40:22Z
url: https://github.com/astral-sh/uv/pull/17339
synced_at: 2026-01-10T05:49:14Z
```

# Only write portable paths in RECORD

---

_Pull request opened by @konstin on 2026-01-06 20:26_

The spec allows both, but we're already using forward paths for paths that are not created by uv.

See
* https://github.com/astral-sh/uv/issues/14446
* https://github.com/python/importlib_metadata/issues/528

Closes #14446

---

_Label `enhancement` added by @konstin on 2026-01-06 20:26_

---

_Review comment by @EliteTK on `crates/uv/tests/it/pip_install.rs`:13697 on 2026-01-06 23:02_

Might be nonsense but: Maybe we should also that there _are_ forward slashes (e.g. that we didn't accidentally just remove all slashes)?

```suggestion
    assert!(record_lines.iter().any(|line| line.contains("/my_script")));
    assert!(record_lines.iter().any(|line| line.contains("/project.h")));
```

---

_@EliteTK approved on 2026-01-06 23:08_

I read some of the background on this, but despite the fact that we were conformant before and are conformant now, it seems like a breaking change...?

---

_Comment by @konstin on 2026-01-07 08:34_

> I read some of the background on this, but despite the fact that we were conformant before and are conformant now, it seems like a breaking change...?

Both are valid formats for transporting the same information, we're only really changing the normalization of the data we write, so no workflows should break (plus we fix importlib.metadata compatibility)

---

_Comment by @EliteTK on 2026-01-07 09:52_

I didn't mean to imply that we shouldn't change it, I think this is strictly a good change. I just mean that, just like `importlib.metadata` relied on pip's decision to always use portable paths, someone might have relied on uv's decision to use windows paths on windows.

But, having thought about it a bit more, I agree it's probably too unlikely to make much noise about it beyond the issue title (which is perfectly clear as is).

---

_Comment by @konstin on 2026-01-07 10:02_

That's true; Given that pip always wrote portable paths, I hope that no parser reads just the uv format in a non-portable way. With the spec allowing portable paths and Windows applications usually understanding forward slashes, I'm confident enough that we don't need to consider this a breaking change.

---

_@zanieb approved on 2026-01-07 15:32_

---

_Merged by @konstin on 2026-01-07 19:40_

---

_Closed by @konstin on 2026-01-07 19:40_

---

_Branch deleted on 2026-01-07 19:40_

---
