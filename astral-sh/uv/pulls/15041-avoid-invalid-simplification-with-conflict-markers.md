```yaml
number: 15041
title: " Avoid invalid simplification with conflict markers "
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/conflict-simplifcation-extras
created_at: 2025-08-03T11:26:53Z
updated_at: 2025-08-06T09:26:28Z
url: https://github.com/astral-sh/uv/pull/15041
synced_at: 2026-01-10T06:44:33Z
```

#  Avoid invalid simplification with conflict markers 

---

_Pull request opened by @konstin on 2025-08-03 11:26_

Previously, `simplify_conflict_markers` assumed that it can remove all conflict set together, when we need to look at each conflict set individually. Specifically, `(platform_machine == 'x86_64' and extra == 'extra-5-foo-b') or extra == 'extra-5-foo-a'` can't be reduced `platform_machine == 'x86_64'` only because it reduces to true when both conflict extras are activated.

This case applied in https://github.com/astral-sh/uv/issues/14805, where a jax 0.5.3 version was used for `platform_machine != 'aarch64' or sys_platform != 'linux'` and the conflict extra `cu128`, but jax 0.7.0 for the conflict extra `cpu`.

Only removing the faulty inference regresses lockfiles to much more verbose markers. To balance the much more conservative inference, I added `unify_inference_sets` to simplify cases where all conflict branches reduce to the same marker.

This still regresses some markers. For example `sys_platform == 'win32'` regresses to `sys_platform == 'win32' or (extra == 'extra-3-pkg-x1' and extra == 'extra-3-pkg-x2')` in `extra_inferences`, even through x1 and x2 conflict and the second conjunction could be simplified away.

Fixes https://github.com/astral-sh/uv/issues/14805

---

_Review requested from @BurntSushi by @konstin on 2025-08-03 11:26_

---

_Label `bug` added by @konstin on 2025-08-03 11:26_

---

_Review comment by @konstin on `crates/uv-resolver/src/universal_marker.rs`:144 on 2025-08-03 11:28_

@BurntSushi Is it correct to assume that if we have inferences, we must be in one of them, i.e. there can't be a missing "empty" inference set?

---

_@konstin reviewed on 2025-08-03 11:28_

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/environment.rs`:403 on 2025-08-03 11:29_

We were previously treating forks with a true marker as non-distinct, even if they were conflict forks.

---

_@konstin reviewed on 2025-08-03 11:29_

---

_@konstin reviewed on 2025-08-03 11:35_

---

_Review comment by @konstin on `crates/uv/tests/it/lock_conflict.rs`:15251 on 2025-08-03 11:35_

The marker is exactly the reachability of python-dateutil 2.8.0, we should be able to elide it.

---

_Review comment by @BurntSushi on `crates/uv-pep508/src/marker/tree.rs`:1165 on 2025-08-04 13:57_

Is it correct that this is calling `Self::evaluate_extras` recursively instead of itself? If so, it's surprising enough (to me anyway) that it might deserve a special comment saying why.

Also, is it worth adding some unit tests for this?

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/environment.rs`:403 on 2025-08-04 14:43_

Ah right because `markers` isn't a "universal" marker yet.

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/mod.rs`:721 on 2025-08-04 14:53_

Should `let packages: ...` be dropped and this reverted back to `resolution.nodes.len()`? (I'm guessing you debug printed `packages` at one point..?)

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/universal_marker.rs`:144 on 2025-08-04 14:59_

Can you say more? What do you mean by a missing empty inference set?

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/universal_marker.rs`:450 on 2025-08-04 15:04_

Nice to be able to get rid of this!

---

_@BurntSushi approved on 2025-08-04 15:05_

Thanks for working on this! I think this all LGTM.

Some of the markers getting a little more complex is a bummer, but it doesn't seem too bad (at least on our existing tests).

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/mod.rs`:721 on 2025-08-04 18:18_

The `nodes` length counts extra packages and such separately, so it prints a number that's not actually packages as you would expect. I considered this a drive-by fix, if it's controversial I'll split out a PR.

---

_@konstin reviewed on 2025-08-04 18:18_

---

_@konstin reviewed on 2025-08-04 18:28_

---

_Review comment by @konstin on `crates/uv-pep508/src/marker/tree.rs`:1165 on 2025-08-04 18:28_

nope, good catch!

---

_@konstin reviewed on 2025-08-04 18:31_

---

_Review comment by @konstin on `crates/uv-resolver/src/universal_marker.rs`:144 on 2025-08-04 18:31_

I.e. we can't reach this node without having either conflict extra(/group/etc.) activated, so we know that at least on of the sets applies. If that assumption is not true, we have to compute another marker where we set all conflict markers to true, and that also has to match the `previous_marker`.

---

_@zanieb reviewed on 2025-08-04 21:45_

---

_Review comment by @zanieb on `crates/uv/tests/it/lock_conflict.rs`:555 on 2025-08-04 21:45_

This sort of seems like noise to a user?

---

_@BurntSushi reviewed on 2025-08-05 12:19_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/mod.rs`:721 on 2025-08-05 12:19_

Ah I see. No, I'm fine with it. Makes sense.

---

_@BurntSushi reviewed on 2025-08-05 12:25_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/universal_marker.rs`:144 on 2025-08-05 12:25_

Hmmm okay. I believe (80% confidence) this is safe to assume. If it weren't, wouldn't the `all_paths_satisfied` check be flawed?

---

_@konstin reviewed on 2025-08-05 13:57_

---

_Review comment by @konstin on `crates/uv/tests/it/lock_conflict.rs`:555 on 2025-08-05 13:57_

Without this information, it's not possible to follow along with the error message. If there is a general conflict, that is not dependent on the conflict fork we're in, this information is redundant, but we don't have that information (we'd need to run the other branch too to check). That is similar to showing the markers in the error message, where in many cases the fork is irrelevant (non of the forks is solvable), but we show it anyway because we need it explain why specific dependencies were in- or excluded.

---

_@konstin reviewed on 2025-08-05 13:59_

---

_Review comment by @konstin on `crates/uv-resolver/src/universal_marker.rs`:144 on 2025-08-05 13:59_

Good point

---

_@zanieb reviewed on 2025-08-05 19:57_

---

_Review comment by @zanieb on `crates/uv/tests/it/lock_conflict.rs`:555 on 2025-08-05 19:57_

I don't think this context makes the following error message any easier to understand? I think it makes it more confusing, even. I think showing the splits for the markers are confusing too though... so I guess we can track improving this as a larger goal.

---

_@konstin reviewed on 2025-08-06 07:54_

---

_Review comment by @konstin on `crates/uv/tests/it/lock_conflict.rs`:555 on 2025-08-06 07:54_

Following up in https://github.com/astral-sh/uv/issues/15092

---

_Comment by @konstin on 2025-08-06 09:07_

Added some notes on a better solution in https://github.com/astral-sh/uv/issues/15101

---

_Merged by @konstin on 2025-08-06 09:26_

---

_Closed by @konstin on 2025-08-06 09:26_

---

_Branch deleted on 2025-08-06 09:26_

---
