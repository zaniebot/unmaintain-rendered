```yaml
number: 16384
title: document MSRV policy
type: pull_request
state: merged
author: carljm
labels:
  - documentation
assignees: []
merged: true
base: main
head: cjm/msrvdoc
created_at: 2025-02-25T20:42:05Z
updated_at: 2025-02-26T15:09:25Z
url: https://github.com/astral-sh/ruff/pull/16384
synced_at: 2026-01-12T15:55:54Z
```

# document MSRV policy

---

_@carljm_

This documents our minimum supported Rust version policy. See https://github.com/astral-sh/ruff/issues/16370

---

_Label `documentation` added by @carljm on 2025-02-25 20:42_

---

_Review requested from @MichaReiser by @carljm on 2025-02-25 20:42_

---

_@dhruvmanila reviewed on 2025-02-26 03:36_

---

_Review comment by @dhruvmanila on `docs/versioning.md`:59 on 2025-02-26 03:36_

I might be missing something but I think it should be safe to assume that it _will_ (instead of "usually") install a pre-built binary.

---

_@carljm reviewed on 2025-02-26 04:41_

---

_Review comment by @carljm on `docs/versioning.md`:59 on 2025-02-26 04:41_

I assumed that there might be some obscure platforms we don't build wheels for where a PyPI install might have to build from source? But if not I'm happy to change this wording!

---

_@MichaReiser approved on 2025-02-26 07:19_

LGTM. This is now stricter than what I had in mind as it disallows us to use a newer MSRV than stable - 2 -- without exception. I wonder if we should relax the phrasing to leave us some wiggle room.

---

_@dhruvmanila reviewed on 2025-02-26 08:40_

---

_Review comment by @dhruvmanila on `docs/versioning.md`:59 on 2025-02-26 08:40_

Good point, I think it's fine to leave as-is.

---

_Comment by @carljm on 2025-02-26 13:11_

That was my understanding of the commitment we were making to downstream. If redistributors can't count on having those two releases' time to catch up, then I'm not sure what the value is of documenting N-2 as a policy?

I'll go with whatever we agree on here, but to me that seems like a significant change that I'd want to be sure we're aligned on beyond just the two of us. 

---

_Comment by @MichaReiser on 2025-02-26 13:13_

> That was my understanding of the commitment we were making to downstream. If redistributors can't count on having those two releases' time to catch up, then I'm not sure what the value is of documenting N-2 as a policy?

I was mainly looking to document that MSRV bumps aren't considered breaking in terms of semver. Not breaking downstream packagers is in our own interest

---

_Comment by @carljm on 2025-02-26 13:46_

My opinion is that the best way to not break downstream packagers is to set a clear policy and stick to it. 

If we plan to do N-2, I think that should be documented somewhere, just for the benefit of future-us remembering what we decided, if nothing else.

If you'd like to propose a different version of this PR, maybe it would be simplest for you to put up an alternative PR?

---

_Comment by @MichaReiser on 2025-02-26 13:49_

As I said. I'm okay with the current version. It simply means that we no longer have the option to make an exception

---

_Comment by @carljm on 2025-02-26 15:09_

Ok, will go ahead and land for now. If you feel it's important that we carve out an option for exceptions, we can discuss and consider that as a separate change.

---

_Merged by @carljm on 2025-02-26 15:09_

---

_Closed by @carljm on 2025-02-26 15:09_

---

_Branch deleted on 2025-02-26 15:09_

---
