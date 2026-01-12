```yaml
number: 15927
title: "Document why uv discards upper bounds on `requires-python`"
type: pull_request
state: merged
author: konstin
labels:
  - documentation
assignees: []
merged: true
base: main
head: konsti/document-discarding-reuqires-python-upper-bounds
created_at: 2025-09-18T09:04:54Z
updated_at: 2025-10-10T15:45:10Z
url: https://github.com/astral-sh/uv/pull/15927
synced_at: 2026-01-12T16:12:01Z
```

# Document why uv discards upper bounds on `requires-python`

---

_@konstin_

We're regularly get questions about this. The DPO thread is the best ressource, but it's also a long read, so I summarized some points for uv's decision.


---

_Label `documentation` added by @konstin on 2025-09-18 09:04_

---

_@zanieb reviewed on 2025-09-18 12:59_

---

_Review comment by @zanieb on `docs/reference/internals/resolver.md`:170 on 2025-09-18 12:59_

```suggestion
For most projects, it's not possible to determine whether they will be compatible with a new Python
```

---

_Review comment by @zanieb on `docs/reference/internals/resolver.md`:171 on 2025-09-18 12:59_

maybe?
```suggestion
version before it's released, so blocking newer versions in advance would block users from
```

---

_@zanieb reviewed on 2025-09-18 12:59_

---

_@zanieb reviewed on 2025-09-18 13:00_

---

_Review comment by @zanieb on `docs/reference/internals/resolver.md`:172 on 2025-09-18 13:00_

```suggestion
upgrading or testing newer Python versions. The exceptions are packages which use the unstable C ABI
```

---

_@zanieb reviewed on 2025-09-18 13:01_

---

_Review comment by @zanieb on `docs/reference/internals/resolver.md`:173 on 2025-09-18 13:01_

"some of which uv handles" is confusing here. Isn't this part of how _Python packaging_ represents this?

---

_Review comment by @zanieb on `docs/reference/internals/resolver.md`:178 on 2025-09-18 13:04_

This paragraph is pretty confusing. Maybe you should say "Transitioning from not using upper bounds to including them is also problematic. For example, when a project adds a new upper bound to `requires-python`, attempts to install it on a new Python version will resolve to an older version of the project _without_ the bound instead of failing."

---

_@zanieb reviewed on 2025-09-18 13:04_

---

_@zanieb reviewed on 2025-09-18 13:05_

---

_Review comment by @zanieb on `docs/reference/internals/resolver.md`:168 on 2025-09-18 13:05_

"informing uv's choice as a combination of aspects" is confusing to me, what are you trying to say there?

---

_@zanieb reviewed on 2025-09-18 13:06_

---

_Review comment by @zanieb on `docs/reference/internals/resolver.md`:165 on 2025-09-18 13:06_

Should we just say "ignored"? "propagation" is a more complicated idea.

---

_@zanieb reviewed on 2025-09-18 13:07_

---

_Review comment by @zanieb on `docs/reference/internals/resolver.md`:182 on 2025-09-18 13:07_

This paragraph is also confusing :)

Can you tell me more about "would potentially resolve differently for different upper bounds"?

---

_@zanieb reviewed on 2025-09-18 13:08_

---

_Review comment by @zanieb on `docs/reference/internals/resolver.md`:189 on 2025-09-18 13:08_

"This choice on the other hand is a problem for packages that ..."

This is a wordy qualifier.

I'd say something like "Ignoring upper bounds is problematic for packages that ..."

---

_@zanieb reviewed on 2025-09-18 13:09_

---

_Review comment by @zanieb on `docs/reference/internals/resolver.md`:195 on 2025-09-18 13:09_

"reject a version due to a too high Python requirement" is confusing. Maybe "reject a package version when it requires a newer Python version"?

---

_@zanieb reviewed on 2025-09-18 13:11_

---

_Review comment by @zanieb on `docs/reference/internals/resolver.md`:195 on 2025-09-18 13:11_

maybe instead of "fork on that Python version" we should just say "fork the resolution to find appropriate package versions for both ranges of Python versions"

---

_Renamed from "Document why uv discards upper bounds" to "Document why uv discards upper bounds on `requires-python`" by @zanieb on 2025-09-18 13:11_

---

_Review comment by @konstin on `docs/reference/internals/resolver.md`:168 on 2025-09-23 13:16_

It's like, go read the full discussion if you think we should change this, these docs won't (can't) cover all of it, but here's a summary of the parts most relevant to uv.

---

_@konstin reviewed on 2025-09-23 13:16_

---

_@konstin reviewed on 2025-09-23 14:11_

---

_Review comment by @konstin on `docs/reference/internals/resolver.md`:179 on 2025-09-23 14:11_

This section is basically saying that we tried that with poetry and it wasn't helpful

---

_@konstin reviewed on 2025-09-23 14:14_

---

_Review comment by @konstin on `docs/reference/internals/resolver.md`:183 on 2025-09-23 14:14_

I'm ignoring the option to split this into two fields here, or other advanced options such as ignoring the upper bound only on the current workspace, this is better documented in the DPO thread, and the other answer is that it's not worth doing that, if we wanted to add new metadata for e.g. the numba case then this should be a new PEP imho, so it's also rather for the DPO discussion.

---

_@konstin reviewed on 2025-09-23 14:16_

---

_Review comment by @konstin on `docs/reference/internals/resolver.md`:202 on 2025-09-23 14:16_

The irony that we're using an upper bound on Python here is not lost on me, though for package selection we're only using the lower bound again, the upper bound exists for internal coherence tracking (and for a lockfile optimization where we drop wheels outside the range).

---

_Merged by @konstin on 2025-10-09 15:15_

---

_Closed by @konstin on 2025-10-09 15:15_

---

_Branch deleted on 2025-10-09 15:24_

---

_Review comment by @Molkree on `docs/reference/internals/resolver.md`:197 on 2025-10-10 14:09_

I think there's a typo here, supposed to be
> This means that

@konstin 

---

_@Molkree reviewed on 2025-10-10 14:09_

---

_@konstin reviewed on 2025-10-10 15:45_

---

_Review comment by @konstin on `docs/reference/internals/resolver.md`:197 on 2025-10-10 15:45_

Thanks!

---
