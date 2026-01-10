```yaml
number: 6126
title: "Normalize `python_version` markers to `python_full_version`"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - lock
assignees: []
merged: true
base: main
head: ibraheem/marker-python-versions
created_at: 2024-08-15T19:06:03Z
updated_at: 2024-08-16T02:07:33Z
url: https://github.com/astral-sh/uv/pull/6126
synced_at: 2026-01-10T13:09:50Z
```

# Normalize `python_version` markers to `python_full_version`

---

_Pull request opened by @ibraheemdev on 2024-08-15 19:06_

## Summary

Normalize all `python_version` markers to their equivalent `python_full_version` form. This avoids false positives in forking because we currently cannot detect any relationships between the two forms. It also avoids subtle bugs due to the truncating semantics of `python_version`. For example, given `requires-python = ">3.12"`, we currently simplify the marker `python_version <= 3.12` to `false`. However, the version `3.12.1` will be truncated to `3.12` for `python_version` comparisons, and thus it satisfies the python requirement and evaluates to `true`.

It is possible to simplify back to `python_version` when writing markers to the lockfile. However, the equivalent `python_full_version` markers are often clearer and easier to simplify, so I lean towards leaving them as `python_full_version`.

There are *a lot* of snapshot updates from this change. I'd like more eyes on the transformation logic in `python_version_to_full_version` to ensure that they are all correct.

Resolves https://github.com/astral-sh/uv/issues/6125.

---

_Label `lock` added by @ibraheemdev on 2024-08-15 19:06_

---

_Review requested from @BurntSushi by @ibraheemdev on 2024-08-15 19:06_

---

_Review requested from @charliermarsh by @ibraheemdev on 2024-08-15 19:06_

---

_@ibraheemdev reviewed on 2024-08-15 19:14_

---

_Review comment by @ibraheemdev on `crates/uv/tests/snapshots/ecosystem__transformers-lock-file.snap`:2887 on 2024-08-15 19:14_

This change stands out.. I verified that this expression on it's own simplifies to `python_full_version >= '3.11'`. I guess that it's redundant with the narrowed requires-python in the fork?

---

_Review comment by @charliermarsh on `crates/pep508-rs/src/marker/algebra.rs`:820 on 2024-08-15 19:19_

Hmm... should `python_version != 3.7` be: `python_version_full <= 3.6 or python_version_full >= 3.8`?

`python_version_full != 3.7` would allow `3.7.1`.

---

_@charliermarsh reviewed on 2024-08-15 19:19_

---

_@charliermarsh reviewed on 2024-08-15 19:23_

---

_Review comment by @charliermarsh on `crates/uv/tests/snapshots/ecosystem__transformers-lock-file.snap`:2887 on 2024-08-15 19:23_

It looks like the declared metadata is:

```
Requires-Dist: numpy>=1.22.4; python_version < "3.11"
Requires-Dist: numpy>=1.23.2; python_version == "3.11"
Requires-Dist: numpy>=1.26.0; python_version >= "3.12"
```

So I think it's _right_ to remove this marker? (Was it a bug that it was omitted before on Python < 3.11?)

---

_Comment by @charliermarsh on 2024-08-15 19:27_

I'm surprised that there aren't more simplifications here.

---

_@charliermarsh reviewed on 2024-08-15 19:29_

---

_Review comment by @charliermarsh on `crates/pep440-rs/src/version_specifier.rs`:514 on 2024-08-15 19:29_

I think this isn't true _in general_... I think it's only true if you ignore pre-releases and such.

---

_@ibraheemdev reviewed on 2024-08-15 19:40_

---

_Review comment by @ibraheemdev on `crates/pep508-rs/src/marker/algebra.rs`:820 on 2024-08-15 19:40_

Yes, that happens in a branch above; `python_version != 3.7` becomes `python_full_version != 3.7.*`. The comment here is wrong, it applies to `~=` not `!=`. `python_version ~= 3.7` is equivalent to `python_full_version ~= 3.7` I believe.

---

_Review comment by @ibraheemdev on `crates/pep440-rs/src/version_specifier.rs`:514 on 2024-08-15 19:51_

Renamed this to `VersionSpecifier::from_release_only_bounds`.

---

_@ibraheemdev reviewed on 2024-08-15 19:51_

---

_@charliermarsh reviewed on 2024-08-15 20:54_

---

_Review comment by @charliermarsh on `crates/pep508-rs/src/marker/algebra.rs`:820 on 2024-08-15 20:54_

So `python_version ~= 3.7` means `python_version >= 3.7, < 4` -- is that right?

I think you're right then...

---

_@zanieb reviewed on 2024-08-15 21:38_

---

_Review comment by @zanieb on `crates/pep508-rs/src/marker/algebra.rs`:761 on 2024-08-15 21:38_

```suggestion
    // does not match `python_full_version == 3.9.0a0`. However, its negation,
```

---

_@zanieb reviewed on 2024-08-15 21:39_

---

_Review comment by @zanieb on `crates/pep508-rs/src/marker/algebra.rs`:763 on 2024-08-15 21:39_

```suggestion
    // `3.9.0a0`, and always evaluates to `false`.
```
Is this what you're saying?

---

_@zanieb reviewed on 2024-08-15 21:40_

---

_Review comment by @zanieb on `crates/pep508-rs/src/marker/algebra.rs`:766 on 2024-08-15 21:40_

What does it mean to "take on pre-release values"? I also don't follow "so this is necessary for simplifying ranges".

---

_@zanieb reviewed on 2024-08-15 21:40_

---

_Review comment by @zanieb on `crates/pep508-rs/src/marker/algebra.rs`:772 on 2024-08-15 21:40_

```suggestion
    // The [`Version`] type ignores trailing `0`s for equality, but still preserves them in its
```
:)

---

_@zanieb reviewed on 2024-08-15 21:41_

---

_Review comment by @zanieb on `crates/pep508-rs/src/marker/algebra.rs`:775 on 2024-08-15 21:41_

Maybe you mean
```suggestion
    // [`Display`] output. We must normalize all versions by stripping trailing `0`s to remove the
    // distinction between versions like `3.9` and `3.9.0`, otherwise the output would depend on which form
    // was added to the global marker interner first.
```

---

_@zanieb reviewed on 2024-08-15 21:44_

---

_Review comment by @zanieb on `crates/pep508-rs/src/marker/algebra.rs`:789 on 2024-08-15 21:44_

```suggestion
/// Returns the equivalent `python_full_version` specifier for a `python_version` specifier.
```
Is this correct?



---

_@zanieb reviewed on 2024-08-15 21:49_

---

_Review comment by @zanieb on `crates/pep508-rs/src/marker/algebra.rs`:793 on 2024-08-15 21:49_

Maybe something like
```suggestion
    // Add a trailing `0` for the minor version, if previously implied
    let major_minor = match *specifier.version().release() {
```
to help clarify the future comment "and adding a trailing `0` would be incorrect."

---

_@zanieb reviewed on 2024-08-15 21:51_

---

_Review comment by @zanieb on `crates/pep508-rs/src/marker/algebra.rs`:800 on 2024-08-15 21:51_

Maybe it'd be helpful to suggest what we'll do in this case? Its quite separated.
```suggestion
        // Specifiers including segments beyond the minor version require separate handling
        _ => None,
```

---

_@zanieb reviewed on 2024-08-15 21:56_

---

_Review comment by @zanieb on `crates/pep508-rs/src/marker/algebra.rs`:807 on 2024-08-15 21:56_

Is there a resource describing these equivalences?

---

_@zanieb reviewed on 2024-08-15 22:00_

---

_Review comment by @zanieb on `crates/pep508-rs/src/marker/algebra.rs`:808 on 2024-08-15 22:00_

What was the basis for `===` meaning `.*`? Seems reasonable just not seeing clear answers online.

---

_@zanieb reviewed on 2024-08-15 22:05_

---

_Review comment by @zanieb on `crates/pep508-rs/src/marker/tree.rs`:1072 on 2024-08-15 22:05_

When is it normalized? It doesn't look like it is here. Before here? later?

---

_@zanieb reviewed on 2024-08-15 22:08_

---

_Review comment by @zanieb on `crates/pep508-rs/src/marker/tree.rs`:1713 on 2024-08-15 22:08_

nice

---

_Review comment by @zanieb on `crates/pep508-rs/src/marker/tree.rs`:1819 on 2024-08-15 22:10_

Is there a reason we prefer the latter form instead of `python_full_version == '3.*'`?

---

_@zanieb reviewed on 2024-08-15 22:10_

---

_@zanieb reviewed on 2024-08-15 22:12_

---

_Review comment by @zanieb on `crates/pep508-rs/src/marker/tree.rs`:1960 on 2024-08-15 22:12_

Really appreciate all this test coverage, super helpful.

---

_@zanieb reviewed on 2024-08-15 22:17_

---

_Review comment by @zanieb on `crates/pep508-rs/src/marker/tree.rs`:2163 on 2024-08-15 22:17_

This one was surprising out of context (and a similar change in the transformers test). I understand this is due to the distributions Python requirement providing a lower bound?

---

_@zanieb approved on 2024-08-15 22:18_

Nice work. I'm not up to date on the markers work at all, but I read this pretty carefully.

---

_@zanieb reviewed on 2024-08-15 22:20_

---

_Review comment by @zanieb on `crates/pep508-rs/src/marker/tree.rs`:1072 on 2024-08-15 22:20_

 I guess this is a note that this only takes `full_python_version` because we perform the normalization upstream of here?

---

_@charliermarsh reviewed on 2024-08-15 23:00_

---

_Review comment by @charliermarsh on `crates/pep508-rs/src/marker/algebra.rs`:807 on 2024-08-15 23:00_

I think there probably isn’t because we had to reason through them haha.

---

_@zanieb reviewed on 2024-08-15 23:07_

---

_Review comment by @zanieb on `crates/pep508-rs/src/marker/algebra.rs`:807 on 2024-08-15 23:07_

(I tried to find one and failed)

---

_@charliermarsh approved on 2024-08-16 00:11_

---

_Merged by @charliermarsh on 2024-08-16 01:42_

---

_Closed by @charliermarsh on 2024-08-16 01:42_

---

_Branch deleted on 2024-08-16 01:42_

---

_@ibraheemdev reviewed on 2024-08-16 02:03_

---

_Review comment by @ibraheemdev on `crates/pep508-rs/src/marker/tree.rs`:2163 on 2024-08-16 02:03_

Yes, it would have been `python_full_version >= 3.8 and python_full_version < '3.9'`. We could simplify this to `python_version == 3.8.*`, but this just ends up happening because of the order in which we try transformations (removing lower bounds before attempting to convert to star equality).

---

_Review comment by @ibraheemdev on `crates/pep508-rs/src/marker/tree.rs`:1819 on 2024-08-16 02:05_

Mostly because I've never seen a bound like this in practice and it didn't seem worth adding a special case for. Can add it if you think it's important.

---

_@ibraheemdev reviewed on 2024-08-16 02:05_

---

_@zanieb reviewed on 2024-08-16 02:07_

---

_Review comment by @zanieb on `crates/pep508-rs/src/marker/tree.rs`:1819 on 2024-08-16 02:07_

I don't, was just curious since had similar transformations for the `*` patch version case

---
