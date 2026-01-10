```yaml
number: 5405
title: Merge identical forks
type: pull_request
state: merged
author: konstin
labels:
  - preview
assignees: []
merged: true
base: main
head: konsti/merge-identical-forks
created_at: 2024-07-24T12:16:12Z
updated_at: 2024-07-25T09:54:55Z
url: https://github.com/astral-sh/uv/pull/5405
synced_at: 2026-01-10T13:37:23Z
```

# Merge identical forks

---

_Pull request opened by @konstin on 2024-07-24 12:16_

Consider these requirements from pylint 3.2.5:

```
Requires-Dist: dill >=0.3.6 ; python_version >= "3.11"
Requires-Dist: dill >=0.3.7 ; python_version >= "3.12"
```

We will split on the python version, but then we may pick a version of `dill` that's `>=0.3.7` in both branches and also have an otherwise identical resolution in both forks. In this case, we merge both forks and store only their conjoined markers.

---

_Label `preview` added by @konstin on 2024-07-24 12:16_

---

_Review requested from @BurntSushi by @konstin on 2024-07-24 12:16_

---

_@konstin reviewed on 2024-07-24 12:16_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:2465 on 2024-07-24 12:16_

That method gets removed upstack

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/mod.rs`:414 on 2024-07-24 13:13_

I buy this, but two thoughts come to mind.

Firstly, is it possible for more than one duplicate resolution to exist at any given point in time? If so, this would I believe only find one of them. But, I do not think this is case, since this is run for every resolution before it is "saved." So it should never be the case that more than one duplicate resolution appears.

Secondly, this is doing an exhaustive search over all existing resolutions to find a possible duplicate. And I suspect that the `Resolution::same_graph` routine is itself not especially cheap. I think this ends up being quadratic in the number of forks (which are themselves exponential in the number of dependencies I think? or possibly in the depth in the dependency tree). I don't have a good feel for how big of an issue that is in practice. Do we have a sense of what the common case is? I would guess the common case is that there aren't any duplicates. So perhaps we can optimize for that path. (To be clear, I don't mean to suggest that be done in this PR.)

---

_@BurntSushi approved on 2024-07-24 13:13_

---

_@konstin reviewed on 2024-07-25 09:54_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:414 on 2024-07-25 09:54_

> Firstly, is it possible for more than one duplicate resolution to exist at any given point in time? If so, this would I believe only find one of them. But, I do not think this is case, since this is run for every resolution before it is "saved." So it should never be the case that more than one duplicate resolution appears.

We fork every time we see conflicting markers, but in many of those cases the requirements themselves are not conflicting (say `numpy >= 1.16` for one and `numpy >= 1.19` for the other). When forking, we can't yet know whether we'll find a compatible `numpy` for both of. I've also seen cases where we end up rejecting the package version we forked on in both branches, removing the conflicting requirements. By copying over preferences from previous forks, we try to coerce two forks to resolving the same package version. Basically, our strategy is to fork often to avoid failing on avoidable conflicts, but still having a solution with as few divergences as possibles.

Re perf: I agree that this is potentially costly, but i think we have to do this to get desirable resolution. We have some short-cuts that we get from std that makes this cheaper: When two forks have a different number of packages, the check is a single `usize` comparison. We also usually have a small number of forks (and the more specific a fork is, the more likely it is we skip future fork points because we're already more specific), so i see this more as a fixed cost of maybe 10^2/2=50 checks. There are of course pathological cases, for those i think we just have to be a bit slow here avoid redundant forks in the lockfile.

---

_Merged by @konstin on 2024-07-25 09:54_

---

_Closed by @konstin on 2024-07-25 09:54_

---

_Branch deleted on 2024-07-25 09:54_

---
