```yaml
number: 11293
title: fix an issue where conflict markers could instigate a very large lock file
type: pull_request
state: merged
author: BurntSushi
labels:
  - bug
  - lock
assignees: []
merged: true
base: main
head: ag/fix-9735
created_at: 2025-02-06T19:58:36Z
updated_at: 2025-02-18T12:44:16Z
url: https://github.com/astral-sh/uv/pull/11293
synced_at: 2026-01-12T16:09:46Z
```

# fix an issue where conflict markers could instigate a very large lock file

---

_@BurntSushi_

This PR fixes a problem where re-running `uv lock` could, in some
cases, exponentially expand the contents of a lock file with more and
more resolution markers. This can only happen when conflict markers are
enabled.

The problem here is a bit in the weeds, but basically, during
resolution, we only use conflict _exclusions_. That's because, during
resolution, we don't really have state that tells us whether a
particular extra (or group) is enabled at this point in the graph (and
I'm not sure such a thing is possible). Instead, we simply do _not_
follow edges corresponding to a package and an extra/group that have
been marked as an exclusion for the current fork. However, we do keep
track of inclusion rules as we go.

Once resolution completes, we translate our inclusion and exclusion
rules for each fork into our "universal" marker that combines standard
PEP 508 markers with conflict markers (the result is still purpotedly
valid PEP 508, but somewhat of a bastardization).

Now, if you then change your `pyproject.toml` in a way that causes
resolution to run again, we read the markers serialized in the lock
file as input to resolution. This helps retain stability (which is
not a concern specific to conflict markers). However, this caused
resolution to work with both inclusion and exclusion rules inside the
marker itself. Which... kinda works, but it's inconsistent with only
respecting exclusion rules. The end result is that we end up producing
extra forks that aren't technically possible, but this isn't known
until all of the constraints are combined at the end of resolution. In
reality, we should never create these forks in the first place.

The conflict forking logic does take impossible forks into account, but
because of how conflict markers were filtering back into resolution,
this wasn't working properly.

So in this PR, we fix the problem by deserializing the conflict marker
back into conflicting inclusion/exclusion rules. Resolution then starts
with these rules in the first place and our logic for not visiting
impossible forks (which were manifesting as `python_version < '0'` in
the lock file) applies.

Fixes #9735


---

_Renamed from "ag/fix 9735" to "fix an issue where conflict markers could instigate a very large lock file" by @BurntSushi on 2025-02-06 19:59_

---

_Review requested from @charliermarsh by @BurntSushi on 2025-02-06 19:59_

---

_Review comment by @BurntSushi on `crates/uv-pep508/src/marker/tree.rs`:1197 on 2025-02-06 20:00_

@ibraheemdev Is this the best way to do this? And is it even right? I spent an embarrassing amount of time on just this part. (I originally wrote it by walking the internal representation, but that ended up being wrong in a way that I didn't understand.)

Basically, the essential challenge I faced here was figuring out whether I was looking at `extra == '...'` or `extra != '...'`.

---

_@BurntSushi reviewed on 2025-02-06 20:00_

---

_@ibraheemdev reviewed on 2025-02-06 21:53_

---

_Review comment by @ibraheemdev on `crates/uv-pep508/src/marker/tree.rs`:1197 on 2025-02-06 21:53_

I guess it really depends what expressions you are looking for.

Consider the expressions `extra != 'x' or (extra == 'x' and b)` and `extra != 'x' or b`. Both have equivalent truth tables. So what does it really mean for the expression `extra == 'x'` to be in the tree?

A more meaningful question would be to ask, is `extra == 'x'` a *solution* to the expression. Or, is `extra == 'x'` *required* in every solution to the expression. Just looking for the expression itself doesn't seem very useful, because it depends on the internal tree representation.

---

_@ibraheemdev reviewed on 2025-02-06 21:58_

---

_Review comment by @ibraheemdev on `crates/uv-pep508/src/marker/tree.rs`:1197 on 2025-02-06 21:58_

I think the way you have it written now will visit `extra == 'x'` if there is no possible solution to the tree when `extra != 'x'`. And it will visit `extra != 'x'` when the tree has a potential solution when `extra != 'x'`. I'm not sure whether you meant for those checks to be mutually exclusive. Should `(extra == 'x' and a) or (extra != 'x' and b)` visit both `extra == 'x'` and `extra != 'x'`?

---

_@BurntSushi reviewed on 2025-02-06 22:19_

---

_Review comment by @BurntSushi on `crates/uv-pep508/src/marker/tree.rs`:1197 on 2025-02-06 22:19_

Yeah this is tricky. I'm not quite sure how to solve this. My _suspicion_ is that those types of expressions don't actually appear in this context. Because it would imply that there's a fork that sometimes _includes_ `x` and sometimes _excludes_ `x`. I believe that by construction that can't happen.

That's a somewhat precarious invariant to rely on, but I'm not quite sure how to fix it.

---

_Label `lock` added by @BurntSushi on 2025-02-07 17:16_

---

_Label `bug` added by @BurntSushi on 2025-02-07 17:16_

---

_@charliermarsh approved on 2025-02-14 21:40_

---

_Merged by @BurntSushi on 2025-02-18 12:44_

---

_Closed by @BurntSushi on 2025-02-18 12:44_

---

_Branch deleted on 2025-02-18 12:44_

---
