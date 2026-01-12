```yaml
number: 7882
title: Add snapshot testing to contribution guide
type: pull_request
state: merged
author: j178
labels:
  - documentation
assignees: []
merged: true
base: main
head: contribute
created_at: 2024-10-03T03:11:59Z
updated_at: 2024-10-22T11:25:59Z
url: https://github.com/astral-sh/uv/pull/7882
synced_at: 2026-01-12T16:08:03Z
```

# Add snapshot testing to contribution guide

---

_@j178_

## Summary

Add a `Snapshot testing` section to `CONTRIBUTING.md`.


---

_@charliermarsh approved on 2024-10-03 12:14_

---

_@charliermarsh reviewed on 2024-10-03 12:15_

---

_Review comment by @charliermarsh on `CONTRIBUTING.md`:78 on 2024-10-03 12:15_

I actually only use `cargo insta review` -- I don't use insta as a test runner.

---

_@charliermarsh reviewed on 2024-10-03 12:15_

---

_Review comment by @charliermarsh on `CONTRIBUTING.md`:55 on 2024-10-03 12:15_

I would personally install this through `cargo install cargo-insta`. I'm curious to hear from others on the team.

---

_@zanieb reviewed on 2024-10-03 14:21_

---

_Review comment by @zanieb on `CONTRIBUTING.md`:78 on 2024-10-03 14:21_

I do use it with `--accept`, e.g.:

```
cit='cargo insta test --lib --bins --tests --accept --test-runner nextest -- '
```

---

_@zanieb reviewed on 2024-10-03 14:22_

---

_Review comment by @zanieb on `CONTRIBUTING.md`:55 on 2024-10-03 14:22_

Yeah I would as well

---

_@charliermarsh reviewed on 2024-10-03 14:39_

---

_Review comment by @charliermarsh on `CONTRIBUTING.md`:78 on 2024-10-03 14:39_

I'm mildly in favor of not suggesting it here (and just encouraging `cargo insta review`). I think it's a simpler mental model to view the `cargo insta` step as separate from testing itself.


---

_@zanieb reviewed on 2024-10-03 14:53_

---

_Review comment by @zanieb on `CONTRIBUTING.md`:78 on 2024-10-03 14:53_

That's fine with me.

---

_@zanieb reviewed on 2024-10-08 19:07_

---

_Review comment by @zanieb on `CONTRIBUTING.md`:76 on 2024-10-08 19:07_

```suggestion
```

---

_Label `documentation` added by @zanieb on 2024-10-08 19:07_

---

_Merged by @zanieb on 2024-10-08 19:08_

---

_Closed by @zanieb on 2024-10-08 19:08_

---

_Branch deleted on 2024-10-22 11:25_

---
