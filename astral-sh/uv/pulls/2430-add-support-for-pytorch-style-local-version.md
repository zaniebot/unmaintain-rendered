```yaml
number: 2430
title: Add support for PyTorch-style local version semantics
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - enhancement
assignees: []
merged: true
base: main
head: charlie/local
created_at: 2024-03-13T20:58:42Z
updated_at: 2024-03-18T12:16:00Z
url: https://github.com/astral-sh/uv/pull/2430
synced_at: 2026-01-10T14:49:08Z
```

# Add support for PyTorch-style local version semantics

---

_Pull request opened by @charliermarsh on 2024-03-13 20:58_

## Summary

This PR adds limited support for PEP 440-compatible local version testing. Our behavior is _not_ comprehensively in-line with the spec. However, it does fix by _far_ the biggest practical limitation, and resolves all the issues that've been raised on uv related to local versions without introducing much complexity into the resolver, so it feels like a good tradeoff for me.

I'll summarize the change here, but for more context, see [Andrew's write-up](https://github.com/astral-sh/uv/issues/1855#issuecomment-1967024866) in the linked issue.

Local version identifiers are really tricky because of asymmetry. `==1.2.3` should allow `1.2.3+foo`, but `==1.2.3+foo` should not allow `1.2.3`. It's very hard to map them to PubGrub, because PubGrub doesn't think of things in terms of individual specifiers (unlike the PEP 440 spec) -- it only thinks in terms of ranges. 

Right now, resolving PyTorch and friends fails, because...

- The user provides requirements like `torch==2.0.0+cu118` and `torchvision==0.15.1+cu118`.
- We then match those exact versions.
- We then look at the requirements of `torchvision==0.15.1+cu118`, which includes `torch==2.0.0`.
- Under PEP 440, this is fine, because `torch @ 2.0.0+cu118` should be compatible with `torch==2.0.0`.
- In our model, though, it's not, because these are different versions. If we change our comparison logic in various places to allow this, we risk breaking some fundamental assumptions of PubGrub around version continuity.
- Thus, we fail to resolve, because we can't accept both `torch @ 2.0.0` and `torch @ 2.0.0+cu118`.

As compared to the solutions we explored in https://github.com/astral-sh/uv/issues/1855#issuecomment-1967024866, at a high level, this approach differs in that we lie about the _dependencies_ of packages that rely on our local-version-using package, rather than lying about the versions that exist, or the version we're returning, etc.

In short:

- When users specify local versions upfront, we keep track of them. So, above, we'd take note of `torch` and `torchvision`.
- When we convert the dependencies of a package to PubGrub ranges, we check if the requirement matches `torch` or `torchvision`. If it's an`==`, we check if it matches (in the above example) for `torch==2.0.0`. If so, we _change_ the requirement to `torch==2.0.0+cu118`. (If it's `==` some other version, we return an incompatibility.)

In other words, we selectively override the declared dependencies by making them _more specific_ if a compatible local version was specified upfront.

The net effect here is that the motivating PyTorch resolutions all work. And, in general, transitive local versions work as expected.

The thing that still _doesn't_ work is: imagine if there were _only_ local versions of `torch` available. Like, `torch @ 2.0.0` didn't exist, but `torch @ 2.0.0+cpu` did, and `torch @ 2.0.0+gpu` did, and so on. `pip install torch==2.0.0` would arbitrarily choose one one `2.0.0+cpu` or `2.0.0+gpu`, and that's correct as per PEP 440 (local version segments should be completely ignored on `torch==2.0.0`). However, uv would fail to identify a compatible version. I'd _probably_ prefer to fix this, although candidly I think our behavior is _ok_ in practice, and it's never been reported as an issue.

Closes https://github.com/astral-sh/uv/issues/1855.

Closes https://github.com/astral-sh/uv/issues/2080.

Closes https://github.com/astral-sh/uv/issues/2328.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-03-13 21:09_

---

_Review requested from @zanieb by @charliermarsh on 2024-03-13 21:09_

---

_Comment by @charliermarsh on 2024-03-13 21:09_

No need to review yet. I need to work on the tests.

---

_@charliermarsh reviewed on 2024-03-13 21:19_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_install_scenarios.rs`:1533 on 2024-03-13 21:19_

Okay, not that impressive, but this is the critical test that changes. (This is the PyTorch resolution case.)

---

_Review comment by @charliermarsh on `scripts/scenarios/generate.py`:153 on 2024-03-14 01:07_

I think this one is fixable with tricks similar to what we do for prereleases with "min version" thing.

---

_@charliermarsh reviewed on 2024-03-14 01:07_

---

_Label `bug` added by @charliermarsh on 2024-03-14 01:13_

---

_Label `compatibility` added by @charliermarsh on 2024-03-14 01:13_

---

_Label `compatibility` removed by @charliermarsh on 2024-03-14 01:13_

---

_Label `enhancement` added by @charliermarsh on 2024-03-14 01:13_

---

_Marked ready for review by @charliermarsh on 2024-03-14 01:13_

---

_@charliermarsh reviewed on 2024-03-14 01:14_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/dependencies.rs`:146 on 2024-03-14 01:14_

`locals.get` should be free (the hash map short-circuits if it's empty) unless local dependencies were declared.

---

_Comment by @charliermarsh on 2024-03-14 01:25_

What's the right order of operations for packse? These tests all pass locally, but I need to publish the updated index for them to pass on CI.

---

_Review comment by @zanieb on `crates/uv-resolver/src/pubgrub/dependencies.rs`:22 on 2024-03-15 03:25_

Why do we have this lint on? I feel like we just ignore it.

---

_@zanieb reviewed on 2024-03-15 03:25_

---

_@charliermarsh reviewed on 2024-03-15 03:30_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/pubgrub/dependencies.rs`:22 on 2024-03-15 03:30_

We should probably have fewer arguments 😂 

---

_@zanieb reviewed on 2024-03-15 03:47_

Wow this is pretty epic. I'll defer to Andrew for the approval, but this _feels_ reasonable?

---

_Review comment by @BurntSushi on `crates/pep440-rs/src/version_specifier.rs`:348 on 2024-03-15 13:49_

What about having this return a `Result` so that callers can choose whether to allow invalid combinations or not?

Or is it intended that this specifically allows combinations of operator and version that are specifically invalid? If so, I think the docs could probably clarify this a bit more.

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/locals.rs`:173 on 2024-03-15 14:36_

What do you think about adding some units tests for the routines in this module?

---

_Review comment by @BurntSushi on `crates/pep440-rs/src/version_specifier.rs`:348 on 2024-03-15 14:38_

I see, reading the code below, this does seem to actually be used to build a `VersionSpecifier` that is otherwise considered invalid.

I find this to be somewhat odd and an unfortunate breakage in encapsulation. But I think I see why you're doing it. As a mitigation, what do you think about changing `from_pattern` back to `new`, and renaming your `new` routine something like, `from_possibly_invalid_parts` or something similarish in name? I would also add to the `VersionSpecifier` docs that callers cannot necessarily rely on the operator/version combination to be valid. That is, it is an intended and supported use case that a `VersionSpecifier` is a _superset_ of what's in PEP 440.

Also, are we sure that things that were previously not allowed but now are (like `VersionSpecifier::new(Operator::LessThanEqual, version_with_local)`) have expected behavior in the various routines on `VersionSpecifier`? Previously, the implementation could assume that the invalid combinations never exist, but that assumption no longer holds. (It's possible everything is fine as-is, but maybe some increased test coverage would be helpful here.)

---

_@BurntSushi approved on 2024-03-15 14:48_

This is really clever. I don't think I would have thought of changing the dependency specifications themselves. Nice work.

> The thing that still doesn't work is: imagine if there were only local versions of torch available. Like, torch @ 2.0.0 didn't exist, but torch @ 2.0.0+cpu did, and torch @ 2.0.0+gpu did, and so on. pip install torch==2.0.0 would arbitrarily choose one one 2.0.0+cpu or 2.0.0+gpu, and that's correct as per PEP 440 (local version segments should be completely ignored on torch==2.0.0). However, uv would fail to identify a compatible version. I'd probably prefer to fix this, although candidly I think our behavior is ok in practice, and it's never been reported as an issue.

I _think_ this is what #1497 is. Although the issue starts with `uv pip install torch==2.1.0+cpu --find-links https://download.pytorch.org/whl/torch_stable.html` (which I think is fixed by this PR), they also list `uv pip install torch==2.1.0 --index-url https://download.pytorch.org/whl/cpu` (which I think is _not_ fixed by this PR). It's not clear to me whether the latter is something they specifically want to do or not though.

---

_@charliermarsh reviewed on 2024-03-15 16:03_

---

_Review comment by @charliermarsh on `crates/pep440-rs/src/version_specifier.rs`:348 on 2024-03-15 16:03_

I'll do all these things.

---

_@charliermarsh reviewed on 2024-03-15 16:04_

---

_Review comment by @charliermarsh on `crates/pep440-rs/src/version_specifier.rs`:348 on 2024-03-15 16:04_

It's also possible that I can remove the actual invalid ones.

---

_Review comment by @charliermarsh on `crates/pep440-rs/src/version_specifier.rs`:348 on 2024-03-16 00:38_

Okay, I removed all the invalid versions, re-added the validation, and threaded through the errors.

---

_@charliermarsh reviewed on 2024-03-16 00:38_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/locals.rs`:173 on 2024-03-16 14:15_

Good call.

---

_@charliermarsh reviewed on 2024-03-16 14:15_

---

_Merged by @charliermarsh on 2024-03-16 14:24_

---

_Closed by @charliermarsh on 2024-03-16 14:24_

---

_Branch deleted on 2024-03-16 14:24_

---

_Comment by @charliermarsh on 2024-03-16 14:25_

BTW `pip install torch==2.1.0 --index-url https://download.pytorch.org/whl/cpu` _does_ work, because there's a version in there without a local tag!

---

_Comment by @BurntSushi on 2024-03-18 12:15_

> BTW `pip install torch==2.1.0 --index-url https://download.pytorch.org/whl/cpu` _does_ work, because there's a version in there without a local tag!

Interesting. It doesn't work for me:

```
$ uv pip install torch==2.1.0 --index-url https://download.pytorch.org/whl/cpu
  x No solution found when resolving dependencies:
  `-> Because torch==2.1.0 is unusable because no wheels are available with a matching Python ABI and you require torch==2.1.0, we can conclude that the requirements are unsatisfiable.
```

IIRC, it's because there aren't any wheels for x86-64 Linux without local version segments. But there are wheels without local version segments for aarch64 Linux (and macOS IIRC).

---
