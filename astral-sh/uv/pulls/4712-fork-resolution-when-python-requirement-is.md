```yaml
number: 4712
title: Fork resolution when Python requirement is narrowed
type: pull_request
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
base: main
head: charlie/fork-python
created_at: 2024-07-01T21:27:56Z
updated_at: 2024-10-27T18:58:05Z
url: https://github.com/astral-sh/uv/pull/4712
synced_at: 2026-01-10T12:53:31Z
```

# Fork resolution when Python requirement is narrowed

---

_Pull request opened by @charliermarsh on 2024-07-01 21:27_

## Summary

This is similar to #4707, but in this case, we don't actually fork, because there are no divergent requirements. Consider:

```text
uv ; python_version >= "3.8"
```

With `-p 3.7`. In this case, there's no version of `uv` that satisfies Python 3.7, but it's only required on Python 3.8 anyway.

The solution I settled on here is to... fork anyway? I'd like to get some input before setting it out, but it makes some sense to me: we want to solve with a different set of constraints than in the rest of the resolution.

Initially, I tried to just merge the fork markers with the requirement markers (i.e., the `python_version >= "3.8"` on the requirement itself) -- but that's insufficient. Imagine if instead we had a package `foo` that depended on `uv`, and `foo ; python_version >= "3.8"`.

Closes https://github.com/astral-sh/uv/issues/4668.


---

_Label `bug` added by @charliermarsh on 2024-07-01 21:28_

---

_Label `preview` added by @charliermarsh on 2024-07-01 21:28_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-07-01 21:28_

---

_Review requested from @konstin by @charliermarsh on 2024-07-01 21:28_

---

_Comment by @charliermarsh on 2024-07-01 22:11_

I'm honestly not sure if this is sound.

---

_Review comment by @charliermarsh on `crates/uv/tests/branching_urls.rs`:400 on 2024-07-01 22:28_

Ok yeah this is just wrong.

---

_@charliermarsh reviewed on 2024-07-01 22:28_

---

_Comment by @charliermarsh on 2024-07-01 22:28_

Open to better ideas here. I think what I have in this PR is wrong actually.

---

_Comment by @konstin on 2024-07-02 11:16_

I think the splitting can work, but we must only split when there the python marker is stricter than the current requires-python and be mindful if we're already splitting on the python version marker, which is happening `root_package_splits_other_dependencies_too`.

---

_Comment by @konstin on 2024-08-20 11:50_

Is this still applicable with #6143?

---

_Label `preview` removed by @zanieb on 2024-08-20 19:05_

---

_Closed by @charliermarsh on 2024-10-27 18:58_

---
