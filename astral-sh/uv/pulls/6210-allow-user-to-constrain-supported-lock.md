```yaml
number: 6210
title: Allow user to constrain supported lock environments
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - preview
  - lock
assignees: []
merged: true
base: main
head: charlie/supported-environments
created_at: 2024-08-19T16:31:44Z
updated_at: 2024-08-20T13:28:05Z
url: https://github.com/astral-sh/uv/pull/6210
synced_at: 2026-01-12T16:07:16Z
```

# Allow user to constrain supported lock environments

---

_@charliermarsh_

## Summary

The strategy here is: if the user provides supported environments, we use those as the initial forks when resolving. As a result, we never add or explore branches that are disjoint with the supported environments. (If the supported environments change, we ignore the lockfile entirely, so we don't have to worry about any interactions between supported environments and the preference forks.)

Closes https://github.com/astral-sh/uv/issues/6184.


---

_@charliermarsh reviewed on 2024-08-19 16:32_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/lock.rs`:983 on 2024-08-19 16:32_

I am tempted to rename these to: `resolution-markers` and `supported-markers` (i.e., rename `environment-markers`).

---

_Review requested from @BurntSushi by @charliermarsh on 2024-08-19 16:32_

---

_Review requested from @zanieb by @charliermarsh on 2024-08-19 16:32_

---

_Review requested from @konstin by @charliermarsh on 2024-08-19 16:32_

---

_Label `enhancement` added by @charliermarsh on 2024-08-19 16:32_

---

_Label `preview` added by @charliermarsh on 2024-08-19 16:32_

---

_Label `lock` added by @charliermarsh on 2024-08-19 16:32_

---

_@charliermarsh reviewed on 2024-08-19 16:33_

---

_Review comment by @charliermarsh on `crates/uv/tests/lock.rs`:9980 on 2024-08-19 16:33_

I think there's another solution where we don't treat the `supported-markers` as the input forks, and instead just test for disjointness everywhere (which would avoid having to write these markers on dependencies). But this is much simpler to implement, and we can always change the strategy later.

---

_Review comment by @konstin on `crates/uv/tests/lock.rs`:9955 on 2024-08-19 16:50_

Do the filters change anything about the lockfile we snapshot?

---

_Review comment by @konstin on `crates/uv-workspace/src/environments.rs`:23 on 2024-08-19 16:55_

Is the difference from the auto-derive here that we drop empty markers that the user may have written?

---

_Review comment by @konstin on `crates/uv-workspace/src/environments.rs`:84 on 2024-08-19 16:56_

This code is dead, is it supposed to be used above?

---

_Review comment by @konstin on `crates/uv/src/commands/project/lock.rs`:650 on 2024-08-19 16:58_

Can we not reuse the preferences still?

---

_@konstin approved on 2024-08-19 17:01_

---

_Review comment by @konstin on `crates/uv/src/commands/project/lock.rs`:513 on 2024-08-19 17:05_

This is actually critical: iirc the resolver currently assumes these are non-overlapping, and if they aren't we can create a conflicting resolution

---

_@konstin reviewed on 2024-08-19 17:06_

---

_@charliermarsh reviewed on 2024-08-19 17:07_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/lock.rs`:650 on 2024-08-19 17:07_

Would we ignore the fork preferences though?

---

_@charliermarsh reviewed on 2024-08-19 17:08_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/lock.rs`:513 on 2024-08-19 17:08_

Wait, what? Is that true? Where does that assumption come into play?

---

_@konstin reviewed on 2024-08-19 17:08_

---

_Review comment by @konstin on `crates/uv/src/commands/project/lock.rs`:650 on 2024-08-19 17:08_

I think we should ignore fork preferences but reuse the package preferences

---

_@charliermarsh reviewed on 2024-08-19 17:51_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/lock.rs`:513 on 2024-08-19 17:51_

I think you're right that they need to be disjoint. We can either (1) require that the user provides disjoint markers, or (2) `and` with the negation of the preceding markers when constructing this.

---

_Comment by @zanieb on 2024-08-19 17:52_

Ideally would be documented in the project or resolver concept document too.

---

_Review comment by @BurntSushi on `crates/uv/src/commands/project/lock.rs`:513 on 2024-08-19 17:56_

I somewhat favor (1), since it makes the behavior a little more explicit and we can give a good error message. I also suspect it would be easier to move from (1) to (2) than it would be to move from (2) to (1).

---

_Review comment by @BurntSushi on `crates/uv/tests/lock.rs`:9980 on 2024-08-19 17:59_

Yeah that was how I was thinking this would be implemented. But I think this is probably fine for now? I agree it's simpler.

---

_@BurntSushi reviewed on 2024-08-19 18:02_

---

_@charliermarsh reviewed on 2024-08-20 03:45_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/environments.rs`:23 on 2024-08-20 03:45_

We support providing a string or a list of strings -- either is fine.

---

_@charliermarsh reviewed on 2024-08-20 04:14_

---

_Review comment by @charliermarsh on `crates/uv/tests/lock.rs`:9955 on 2024-08-20 04:14_

No, shouldn't be.

---

_Comment by @charliermarsh on 2024-08-20 04:15_

Ok, modified the behavior to _require_ disjoint forks.

---

_Comment by @charliermarsh on 2024-08-20 04:19_

Also added docs.

---

_@konstin reviewed on 2024-08-20 08:53_

---

_Review comment by @konstin on `crates/uv/src/commands/project/lock.rs`:513 on 2024-08-20 08:53_

Putting this here also for reference: The problem is: If we resolve foo==1 for `(sys_platform == "linux" or sys_platform == "darwin")` and foo==2 for `(sys_platform == "linux" or sys_platform == "nt")`, which one would we install on linux?

---

_@zanieb reviewed on 2024-08-20 13:04_

---

_Review comment by @zanieb on `docs/concepts/projects.md`:72 on 2024-08-20 13:04_

Any note that these need to be disjoint?

---

_@zanieb reviewed on 2024-08-20 13:04_

---

_Review comment by @zanieb on `docs/concepts/projects.md`:72 on 2024-08-20 13:04_

(and what that means)

---

_@BurntSushi approved on 2024-08-20 13:13_

---

_Merged by @charliermarsh on 2024-08-20 13:28_

---

_Closed by @charliermarsh on 2024-08-20 13:28_

---

_Branch deleted on 2024-08-20 13:28_

---
