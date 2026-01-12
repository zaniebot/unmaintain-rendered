```yaml
number: 5723
title: Rewrite resolver docs
type: pull_request
state: merged
author: konstin
labels:
  - documentation
  - preview
assignees: []
merged: true
base: main
head: konsti/resolver-docs
created_at: 2024-08-02T11:34:09Z
updated_at: 2024-08-06T08:12:34Z
url: https://github.com/astral-sh/uv/pull/5723
synced_at: 2026-01-12T16:06:59Z
```

# Rewrite resolver docs

---

_@konstin_

This PR rewrites the resolver concept and adds a resolver internals page targeted at power users.

The new resolution concept documentation has three parts:
* An introduction for people how never heard of "resolution" before, and a motivating example what it does. I've also shoved the part about equally valid resolutions in there.
* Features you commonly use: Non-universal vs. universal resolution, lowest resolution amd pre-releases.
* Expert features, we don't advertise them, you'll only need them in complex cases when you already know and i kept them to the reference points you need in this case: Constraints, overrides and exclude-newer.

I intentionally didn't lay out any detail of the resolution itself, the idea is that users get a vague sense of "uv has to select fitting versions", but then they learn the options they have to use and some common failure points without ever coming near SAT or even graphs.

The resolution internals reference page is targeted at power users who need to understand what is going on behind the scenes. It assumes ample prior knowledge and exists to explain the uv-specific behaviors for someone who already understands dependency resolution conceptually and has interacted with their dependency tree before. I had a section on the lockfile but removed it because it found the lockfile to be too self-documenting.

I haven't touched the readme.

Closes #5603
Closes #5238
Closes #5237

---

_Label `documentation` added by @konstin on 2024-08-02 11:37_

---

_Label `preview` added by @konstin on 2024-08-02 11:37_

---

_Review requested from @zanieb by @konstin on 2024-08-02 11:37_

---

_Review requested from @BurntSushi by @konstin on 2024-08-02 11:37_

---

_Review requested from @charliermarsh by @konstin on 2024-08-02 11:37_

---

_Marked ready for review by @konstin on 2024-08-02 13:39_

---

_Review comment by @zanieb on `BENCHMARKS.md`:12 on 2024-08-03 13:42_

We will need to update this separately.

---

_@zanieb reviewed on 2024-08-03 13:42_

---

_@zanieb approved on 2024-08-03 13:43_

Thanks! I'll post edits separately.

---

_@konstin reviewed on 2024-08-03 13:45_

---

_Review comment by @konstin on `docs/concepts/resolution.md`:25 on 2024-08-03 13:45_

There is overlap with the pip page here but it's hard not to have that

---

_Merged by @zanieb on 2024-08-03 13:47_

---

_Closed by @zanieb on 2024-08-03 13:47_

---

_Branch deleted on 2024-08-03 13:47_

---

_Review comment by @BurntSushi on `docs/concepts/resolution.md`:25 on 2024-08-05 15:49_

It might make sense to link to the pip page here?

---

_Review comment by @BurntSushi on `docs/concepts/resolution.md`:56 on 2024-08-05 15:53_

I wonder if it also makes sense to include a note here making explicit the assumptions `uv` makes. For example, that package metadata doesn't change between platforms for the same version. And crucially, if a package only has an sdist, then universal resolution still may need to build it to get metadata. So e.g., if a Windows-only package whose metadata can only be extracted by building an sdist and building the sdist can only be done on Windows, then that package I believe will prevent universal resolution from non-Linux platforms. (I don't have any real world examples of such a package though.)

---

_Review comment by @BurntSushi on `docs/concepts/resolution.md`:56 on 2024-08-05 16:06_

This is somewhat addressed in the "expert" docs below. But it might be worth a brief mention here.

---

_@BurntSushi reviewed on 2024-08-05 16:06_

Nice! Looks great. :-)

---

_Comment by @zanieb on 2024-08-05 21:31_

I'll take @BurntSushi's comments into account.

---

_@konstin reviewed on 2024-08-06 08:12_

---

_Review comment by @konstin on `docs/concepts/resolution.md`:56 on 2024-08-06 08:12_

I think this something we should communicate in the error message (specifically, add a note in the build error when we're on a different platform), it's not something you need to think upfront in the concept docs

---
