```yaml
number: 2834
title: Backtrack on distributions with invalid metadata
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/filter-metadata
created_at: 2024-04-05T14:23:15Z
updated_at: 2024-04-05T23:26:48Z
url: https://github.com/astral-sh/uv/pull/2834
synced_at: 2026-01-12T16:05:14Z
```

# Backtrack on distributions with invalid metadata

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/2821.


---

_Review comment by @zanieb on `crates/uv-resolver/src/resolver/mod.rs`:351 on 2024-04-05 16:30_

👍 This is the kind of thing that would be improved by generic metadata in PubGrub... I started work on it but it's a lot.

---

_@zanieb reviewed on 2024-04-05 16:30_

---

_@zanieb approved on 2024-04-05 16:31_

---

_Label `bug` added by @charliermarsh on 2024-04-05 18:12_

---

_Marked ready for review by @charliermarsh on 2024-04-05 18:12_

---

_@charliermarsh reviewed on 2024-04-05 18:28_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_compile.rs`:4573 on 2024-04-05 18:28_

@zanieb - Is there any way for me to improve this (the triple "because") without larger refactors?

---

_@charliermarsh reviewed on 2024-04-05 18:29_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_compile.rs`:4573 on 2024-04-05 18:29_

Can I make `Dependencies::Unavailable` accept an enum or something?

---

_@charliermarsh reviewed on 2024-04-05 18:29_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:1008 on 2024-04-05 18:29_

These skipped versions will appear as warnings at least with `--verbose`.

---

_@charliermarsh reviewed on 2024-04-05 18:33_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_sync.rs`:1103 on 2024-04-05 18:33_

This is slightly worse... I guess I could make the "reasons" more specific?

---

_@charliermarsh reviewed on 2024-04-05 18:38_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_sync.rs`:1103 on 2024-04-05 18:38_

Maybe the `.dist-info` thing is so broken that we shouldn't backtrack. IDK.

---

_Review requested from @zanieb by @charliermarsh on 2024-04-05 18:38_

---

_@zanieb reviewed on 2024-04-05 19:01_

---

_Review comment by @zanieb on `crates/uv/tests/pip_compile.rs`:4573 on 2024-04-05 19:01_

Maybe you could switch it from a string to an enum but uhh I'm not really sure. This is the idea behind a generic incompatibility metadata type in PubGrub.

---

_@zanieb reviewed on 2024-04-05 19:02_

---

_Review comment by @zanieb on `crates/uv/tests/pip_sync.rs`:1103 on 2024-04-05 19:02_

Hm it's a little weird to throw the no solution error this way (i.e. compared to how it's formatted elsewhere)

You could push the detailed reasons into some hints?

---

_@charliermarsh reviewed on 2024-04-05 19:08_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_sync.rs`:1103 on 2024-04-05 19:08_

> Hm it's a little weird to throw the no solution error this way (i.e. compared to how it's formatted elsewhere)

What do you mean by this?

---

_@charliermarsh reviewed on 2024-04-05 20:23_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_compile.rs`:4573 on 2024-04-05 20:23_

If I _can_ do it, do you mind if I do it?

---

_@zanieb reviewed on 2024-04-05 21:52_

---

_Review comment by @zanieb on `crates/uv/tests/pip_sync.rs`:1103 on 2024-04-05 21:52_

```
error: Because foo was found, ...
```

vs

```
× No solution found when resolving dependencies:
╰─▶ Because you require ...
```

I don't mind the lack of the arrow and such but the "No solution found" part seems relevant.

---

_@zanieb reviewed on 2024-04-05 21:53_

---

_Review comment by @zanieb on `crates/uv/tests/pip_compile.rs`:4573 on 2024-04-05 21:53_

No I don't mind it seems like a step in the right direction


---

_@charliermarsh reviewed on 2024-04-05 22:00_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_sync.rs`:1103 on 2024-04-05 22:00_

Ah yeah. I'll add separately.

---

_Merged by @charliermarsh on 2024-04-05 22:00_

---

_Closed by @charliermarsh on 2024-04-05 22:00_

---

_Branch deleted on 2024-04-05 22:00_

---

_@charliermarsh reviewed on 2024-04-05 22:00_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_compile.rs`:4573 on 2024-04-05 22:00_

I'll add separately.

---

_Comment by @charliermarsh on 2024-04-05 23:26_

Filed issues for the follow-ups.

---
