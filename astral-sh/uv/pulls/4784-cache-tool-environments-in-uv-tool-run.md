```yaml
number: 4784
title: "Cache tool environments in `uv tool run`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: charlie/cache-tool-run
created_at: 2024-07-03T17:46:44Z
updated_at: 2024-07-03T23:25:42Z
url: https://github.com/astral-sh/uv/pull/4784
synced_at: 2026-01-12T16:06:27Z
```

# Cache tool environments in `uv tool run`

---

_@charliermarsh_

## Summary

The basic strategy:

- When the user does `uv tool run`, we resolve the `from` and `with` requirements (always).
- After resolving, we generate a hash of the requirements. For now, I'm just converting to a lockfile and hashing _that_, but that's an implementation detail.
- Once we have a hash, we _also_ hash the interpreter.
- We then store environments in `${CACHE_DIR}/${INTERPRETER_HASH}/${RESOLUTION_HASH}`.

Some consequences:

- We cache based on the interpreter, so if you request a different Python, we'll create a new environment (even if they're compatible). This has the nice side-effect of ensuring that we don't use environments for interpreters that were later deleted.
- We cache the `from` and `with` together. In practice, we may want to cache them separately, then layer them? But this is also an implementation detail that we could change later.
- Because we use the lockfile as the cache key, we will invalidate the cache when the format changes. That seems ok, but we could improve it in the future by generating a stable hash from a lockfile that's independent of the schema.

Closes https://github.com/astral-sh/uv/issues/4752.


---

_Review requested from @zanieb by @charliermarsh on 2024-07-03 17:46_

---

_Review requested from @konstin by @charliermarsh on 2024-07-03 17:46_

---

_Label `enhancement` added by @charliermarsh on 2024-07-03 17:46_

---

_Label `preview` added by @charliermarsh on 2024-07-03 17:46_

---

_Comment by @charliermarsh on 2024-07-03 21:20_

Not sure what to do about the Windows failure. Hide the counts? Because we don't end up installing `colorama`, so it doesn't get deducted from the total.

---

_Comment by @zanieb on 2024-07-03 22:45_

@charliermarsh we have a `TestContext::with_filtered_counts` utility for this exact purpose

---

_Comment by @charliermarsh on 2024-07-03 22:46_

Yeah I did see that, but wasn't sure if it was ok that it obscures the numbers entirely. It makes sense though.

---

_Review comment by @zanieb on `crates/uv-cache/src/lib.rs`:639 on 2024-07-03 22:46_

👍 

---

_@zanieb reviewed on 2024-07-03 22:46_

---

_@zanieb reviewed on 2024-07-03 22:50_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/ephemeral.rs`:39 on 2024-07-03 22:50_

Once we have #4729 we should probably pass around a `BaseClientBuilder` instead of these individual options

---

_Comment by @zanieb on 2024-07-03 22:51_

I feel like in most cases the numbers are redundant information 🤷‍♂️ 

---

_@zanieb reviewed on 2024-07-03 22:53_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/ephemeral.rs`:125 on 2024-07-03 22:53_

So the receipt in this case is just an empty file? Kind of fun.

---

_@charliermarsh reviewed on 2024-07-03 22:53_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/ephemeral.rs`:125 on 2024-07-03 22:53_

Haha. We do the same thing (with the same name) in the Git checkouts so I just borrowed that pattern.

---

_@zanieb approved on 2024-07-03 22:55_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/ephemeral.rs`:125 on 2024-07-03 22:57_

Nice that seems like it might be a helpful pattern for more scoped locks in #4720 

---

_@zanieb reviewed on 2024-07-03 22:57_

---

_Merged by @charliermarsh on 2024-07-03 23:25_

---

_Closed by @charliermarsh on 2024-07-03 23:25_

---

_Branch deleted on 2024-07-03 23:25_

---
