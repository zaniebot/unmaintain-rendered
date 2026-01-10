```yaml
number: 8754
title: refactor resolver forking to make it easier to centralize decision making
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/fork-refactor
created_at: 2024-11-01T16:00:27Z
updated_at: 2024-11-05T12:21:00Z
url: https://github.com/astral-sh/uv/pull/8754
synced_at: 2026-01-10T11:59:59Z
```

# refactor resolver forking to make it easier to centralize decision making

---

_Pull request opened by @BurntSushi on 2024-11-01 16:00_

This PR is a step in the direction to supporting conflicting groups.
While this PR doesn't make any specific effort to support conflicting
groups, it does refactor how resolver fork state is represented such
that it is more encapsulated than what we have today. This should
hopefully make it easier to tweak the logic to include conflicting
groups. (The idea is that a `ResolverEnvironment` will represent the
conflicting group configuration and then use that in its own internal
case analysis.)

The main idea behind this refactor is to try to push direct
`MarkerTree` manipulation down into `ResolverEnvironment`. The
encpasulation isn't quite perfect (for example, we still have a
`try_markers` method that exposes the markers), but I think we can
improve upon that going forward.


---

_Review requested from @konstin by @BurntSushi on 2024-11-01 16:00_

---

_Review requested from @charliermarsh by @BurntSushi on 2024-11-01 16:00_

---

_Review comment by @konstin on `crates/uv-resolver/src/candidate_selector.rs`:144 on 2024-11-01 22:16_

sweet

---

_Review comment by @konstin on `crates/uv-resolver/src/pubgrub/package.rs`:171 on 2024-11-01 22:22_

why the `#[allow(dead_code)]`?

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/environment.rs`:11 on 2024-11-01 22:26_

I'd phrase the other round, we're building a resolution that works for the specified environment(s), ignoring dependency edge outside of it.

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/environment.rs`:120 on 2024-11-01 22:31_

i see the order of forks similar to the order of dependencies: we don't guarantee any specific treatment, but reordering can help where our prioritization fails.

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/environment.rs`:157 on 2024-11-01 22:33_

can we ever run into this branch, and if so, isn't discarding the rhs wrong?

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/environment.rs`:318 on 2024-11-01 22:35_

Are we doing unit tests inline or in an extra file?

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/environment.rs`:270 on 2024-11-01 22:36_

These messages should start with a lower case letter, so that we don't get an upper case letter mid-sentence.

---

_@konstin approved on 2024-11-01 22:39_

---

_@zanieb reviewed on 2024-11-01 23:43_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_compile.rs`:1943 on 2024-11-01 23:43_

"Marker Environment" seems pretty weird for users to see here?

---

_@zanieb reviewed on 2024-11-01 23:43_

---

_Review comment by @zanieb on `crates/uv/tests/it/branching_urls.rs`:134 on 2024-11-01 23:43_

Should not be capitalized

---

_@zanieb reviewed on 2024-11-01 23:44_

---

_Review comment by @zanieb on `crates/uv/tests/it/branching_urls.rs`:134 on 2024-11-01 23:44_

(As noted in https://github.com/astral-sh/uv/pull/8754#discussion_r1826349897)

---

_@BurntSushi reviewed on 2024-11-04 15:19_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/pubgrub/package.rs`:171 on 2024-11-04 15:19_

I think I added this before I realized I wanted to do a refactor. It isn't used yet, but I believe I'll need it for the conflicting extras work. I'll remove this for now.

---

_@BurntSushi reviewed on 2024-11-04 15:43_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/environment.rs`:11 on 2024-11-04 15:43_

Hmmm I see. It's a little tortured, but I re-phrased it like so:

```
/// Represents one or more marker environments for a resolution.
///
/// Dependencies outside of the marker environments represented by this value
/// are ignored for that particular resolution.
///
/// In normal "pip"-style resolution, one resolver environment corresponds to
/// precisely one marker environment. In universal resolution, multiple marker
/// environments may be specified via a PEP 508 marker expression. In either
/// case, as mentioned above, dependencies not in these marker environments are
/// ignored for the corresponding resolution.
```

---

_@BurntSushi reviewed on 2024-11-04 15:45_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/environment.rs`:120 on 2024-11-04 15:45_

Thanks! I've updated the wording here.

---

_@BurntSushi reviewed on 2024-11-04 15:51_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/environment.rs`:157 on 2024-11-04 15:51_

My thinking was that "narrowing environment" is not really a well defined operation when resolution is configured for one single specific marker environment. So in that sense, narrowing would be a no-op.

In practice, I think you're right that this code path isn't actually exercised, and if it was, we should _probably_ consider it a bug. So I've changed this to a panicking branch and updated the docs.

---

_@BurntSushi reviewed on 2024-11-04 15:53_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/environment.rs`:318 on 2024-11-04 15:53_

I think this is idiomatic for unit tests. And we do it pretty frequently?

(Feel free to start a thread on Discord about this. I'm curious about your thoughts on this.)

---

_@BurntSushi reviewed on 2024-11-04 15:58_

---

_Review comment by @BurntSushi on `crates/uv/tests/it/branching_urls.rs`:134 on 2024-11-04 15:58_

Yeah good catch. Fixed!

---

_Review comment by @BurntSushi on `crates/uv/tests/it/pip_compile.rs`:1943 on 2024-11-04 15:59_

Also good catch. Fixed!

---

_@BurntSushi reviewed on 2024-11-04 15:59_

---

_Merged by @BurntSushi on 2024-11-04 16:09_

---

_Closed by @BurntSushi on 2024-11-04 16:09_

---

_Branch deleted on 2024-11-04 16:09_

---

_@konstin reviewed on 2024-11-04 20:51_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/environment.rs`:318 on 2024-11-04 20:51_

i prefer the inline tests, but all other tests were moved out in the PR that merged the integration tests, I'm asking for consistency's sake

---

_@zanieb reviewed on 2024-11-04 21:58_

---

_Review comment by @zanieb on `crates/uv-resolver/src/resolver/environment.rs`:318 on 2024-11-04 21:58_

Yeah I think this is no longer idiomatic for us for performance reasons? I don't know all the reasoning.

---

_@BurntSushi reviewed on 2024-11-05 12:21_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/environment.rs`:318 on 2024-11-05 12:21_

Hmmm. Yeah I get the thinking with merging the integration tests, but I don't yet grok the thinking around unit tests being inline or in a separate file.

---
