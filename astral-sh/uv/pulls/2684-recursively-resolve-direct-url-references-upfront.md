```yaml
number: 2684
title: Recursively resolve direct URL references upfront
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/recursive-locals
created_at: 2024-03-27T03:02:11Z
updated_at: 2024-04-01T21:16:21Z
url: https://github.com/astral-sh/uv/pull/2684
synced_at: 2026-01-10T14:49:08Z
```

# Recursively resolve direct URL references upfront

---

_Pull request opened by @charliermarsh on 2024-03-27 03:02_

## Summary

This PR would enable us to support transitive URL requirements. The key idea is to leverage the fact that...

- URL requirements can only come from URL requirements.
- URL requirements identify a _specific_ version, and so don't require backtracking.

Prior to running the "real" resolver, we recursively resolve any URL requirements, and collect all the known URLs upfront, then pass those to the resolver as "lookahead" requirements. This means the resolver knows upfront that if a given package is included, it _must_ use the provided URL.

Closes https://github.com/astral-sh/uv/issues/1808.


---

_@charliermarsh reviewed on 2024-03-27 04:03_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/prerelease_mode.rs`:64 on 2024-03-27 04:03_

Genuinely not sure how constraints and overrides should work in all these different TODOs.

---

_@charliermarsh reviewed on 2024-03-27 04:03_

---

_Review comment by @charliermarsh on `crates/uv-requirements/src/lookahead.rs`:96 on 2024-03-27 04:03_

This can be extended to support wheels -- not hard.

---

_Review comment by @charliermarsh on `crates/uv-requirements/src/lookahead.rs`:57 on 2024-03-27 04:07_

Uncertain how to handle these... I don't think we want to blindly resolve them recursively, because we can't _know_ that a given constraint or override will _actually_ be applied, and so we might end up incorporating requirements that won't end up being correct here.

E.g., consider package `foo @ bar`, which has a URL requirement of `baz @ bop`. Assume `foo @ bar` is a constraint. If we include it here, then we'll also include `baz @ bop` as a required URL. But what if no one ever requests `foo`, and so the constraint should have no effect? Now we've added a `baz @ bop` requirement that shouldn't be enforced...

I think the best we can do (under this paradigm) is enforce overrides and constraints for the dependencies we see during _this_ resolution pass.


---

_@charliermarsh reviewed on 2024-03-27 04:07_

---

_Comment by @zanieb on 2024-03-28 21:22_

Sweet! I tested this on an old version of packse which has git dependencies.

```
❯ cargo run -- pip install git+https://github.com/zanieb/packse@14c55ed460d24cc3e247dd3e09506a8e75e35adcpackse@14c55ed460d24cc3e247dd3e09506a8e75e35adc
 Updated https://github.com/zanieb/packse (14c55ed)                                                                           
 Updated https://github.com/zanieb/devpi (22f71ac)
 Updated https://github.com/zanieb/waitress (d6d764b)                                                                        
Resolved 80 packages in 10.49s
   Built pyramid-chameleon==0.3
   Built waitress @ git+https://github.com/zanieb/waitress@d6d764bcc970e1e50486153588eda8a92cf5b5e4
   Built packse @ git+https://github.com/zanieb/packse@14c55ed460d24cc3e247dd3e09506a8e75e35adc
   Built devpi-server @ git+https://github.com/zanieb/devpi@22f71acb8f08a59a098e7ad434cf388a1193fc24#subdirectory=server
   Built devpi==2.2.0                                                                                                        Downloaded 79 packages in 4.48s
Installed 80 packages in 87ms
...
```

---

_Marked ready for review by @charliermarsh on 2024-04-01 15:37_

---

_Label `enhancement` added by @charliermarsh on 2024-04-01 15:37_

---

_Comment by @charliermarsh on 2024-04-01 15:37_

This PR lifts a significant limitation (transitive URL dependencies) at the cost of some parallelism (we have to build and resolve them all upfront).

---

_Review requested from @zanieb by @charliermarsh on 2024-04-01 16:00_

---

_Comment by @zanieb on 2024-04-01 16:23_

It's only going to be a performance degradation for cases where the user previously had to enumerate all the URLs, correct?

---

_@zanieb approved on 2024-04-01 16:27_

Sweet

---

_Comment by @charliermarsh on 2024-04-01 16:29_

That seems fair, yes. There are two issues:

1. In general, the use of the lookahead resolver is slower than the theoretically-optimal" behavior of resolving as we go within the resolver, because (e.g.) we're spending time resolving source distributions when we could (in parallel) be fetching metadata from PyPI for other dependencies... So we're probably not saturating the network. By doing more work in the lookahead resolver, we make that a bit worse.
2. Now, we're resolving at multiple levels rather than requiring users to enumerate all the URL requirements upfront, so what was previously parallel is now sequential (which is more directly-related to your comment).


---

_Comment by @zanieb on 2024-04-01 16:30_

Did you do any benches? Worth it? The behavior seems worth enabling regardless.

---

_Comment by @charliermarsh on 2024-04-01 16:31_

Yeah my guess is that it's pretty rare to have nested URL dependencies anyway, and in the cases that you _do_, users would definitely prefer that they work even if there's a performance hit.

---

_Comment by @charliermarsh on 2024-04-01 16:36_

I think I need to update docs before merging though.

---

_Merged by @charliermarsh on 2024-04-01 21:16_

---

_Closed by @charliermarsh on 2024-04-01 21:16_

---

_Branch deleted on 2024-04-01 21:16_

---
