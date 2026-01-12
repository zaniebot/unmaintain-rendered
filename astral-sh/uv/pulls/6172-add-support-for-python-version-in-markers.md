```yaml
number: 6172
title: "Add support for `python_version in ...` markers"
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/python-in-markers
created_at: 2024-08-17T17:36:30Z
updated_at: 2024-08-19T14:10:21Z
url: https://github.com/astral-sh/uv/pull/6172
synced_at: 2026-01-12T16:07:15Z
```

# Add support for `python_version in ...` markers

---

_@zanieb_

Closes #3683 

Note our semantics do not exactly match the specification so we can perform algebra on the markers. See the caveats in the documentation (and in the discussion below).

---

_Label `enhancement` added by @zanieb on 2024-08-17 17:36_

---

_@zanieb reviewed on 2024-08-17 17:43_

---

_Review comment by @zanieb on `crates/uv/tests/snapshots/ecosystem__black-lock-file.snap`:551 on 2024-08-17 17:43_

Now that's interesting... ref #6168 

---

_@charliermarsh reviewed on 2024-08-17 17:45_

---

_Review comment by @charliermarsh on `crates/uv/tests/snapshots/ecosystem__black-lock-file.snap`:551 on 2024-08-17 17:45_

(My guess is that it needs to be `python_full_version` to simplify.)

---

_Review comment by @zanieb on `crates/uv/tests/snapshots/ecosystem__black-lock-file.snap`:551 on 2024-08-17 18:05_

Yep! Now it simplifies out entirely.

---

_@zanieb reviewed on 2024-08-17 18:05_

---

_Comment by @charliermarsh on 2024-08-17 19:52_

Generally looks good but will hold off on reviewing.

---

_Comment by @ibraheemdev on 2024-08-17 20:04_

Are we assuming that `python_full_version in "3.11 3.12 3.13"` only matches those 3 versions? Because that can technically match `3.1` as well. I think using `in` for this is pretty dubious in general, it's unfortunate that it's a pattern used by some packages. What about `python_full_version in "3.11,3.12,3.13"`?

---

_Comment by @charliermarsh on 2024-08-17 20:07_

Yeah, it's tough. There's really just one package that uses this (`pathlib2`) that various people have run into. So I'd say it's ok to split on whitespace, and assume each is an exact version...

---

_Comment by @zanieb on 2024-08-17 21:02_

Hm yeah it gets pretty complicated if it's a true substring match. I think we should probably be relatively strict with what we support and see if more edge-cases arise?

---

_Comment by @zanieb on 2024-08-18 14:00_

Created https://github.com/pubgrub-rs/pubgrub/issues/249 to track better range union performance, which I left a couple TODOs about.

---

_Marked ready for review by @zanieb on 2024-08-18 14:24_

---

_Review comment by @ibraheemdev on `crates/pep508-rs/src/marker/tree.rs`:1897 on 2024-08-18 17:54_

```suggestion
        // Technically these is are valid substring comparison, but we do not allow them.
```

---

_Review comment by @ibraheemdev on `crates/pep508-rs/src/marker/tree.rs`:1902 on 2024-08-18 17:55_

Why is this always `true`?

---

_Review comment by @ibraheemdev on `crates/pep508-rs/src/marker/algebra.rs`:613 on 2024-08-18 17:56_

```suggestion
    /// Only for use when the `key` is a `PythonVersion`. Normalizes to `PythonFullVersion`.
```

---

_@ibraheemdev reviewed on 2024-08-18 17:57_

---

_@zanieb reviewed on 2024-08-18 18:40_

---

_Review comment by @zanieb on `crates/pep508-rs/src/marker/tree.rs`:1902 on 2024-08-18 18:40_

Because `3.*` matches all known Python versions

---

_@charliermarsh reviewed on 2024-08-18 18:51_

---

_Review comment by @charliermarsh on `crates/pep508-rs/src/marker/tree.rs`:1902 on 2024-08-18 18:51_

It could be false in the future though right?

---

_@ibraheemdev reviewed on 2024-08-18 18:51_

---

_Review comment by @ibraheemdev on `crates/pep508-rs/src/marker/tree.rs`:1902 on 2024-08-18 18:51_

Does it? `python_version == '3.*'` simplifies to `python_full_version >= '3' and python_full_version < '4'` so I'm not sure why it would be different here. We aren't calling `simplify_python_versions` in this test case.

I suspect there is a parsing error causing it to return `None`.

---

_@zanieb reviewed on 2024-08-18 20:54_

---

_Review comment by @zanieb on `crates/pep508-rs/src/marker/tree.rs`:1902 on 2024-08-18 20:54_

Oh, I guess it's not actually a valid PEP 440 version (because it requires an operator to create the equal star).

Thanks — I'll update the comment accordingly. We might need to update the list to be `VersionSpecifier` instead of `Version` someday.

---

_@charliermarsh reviewed on 2024-08-18 22:06_

---

_Review comment by @charliermarsh on `crates/pep508-rs/src/marker/tree.rs`:1902 on 2024-08-18 22:06_

Oh yeah sorry, I guess this would never be true since it's literally a string match.

---

_@ibraheemdev approved on 2024-08-18 23:25_

---

_@zanieb reviewed on 2024-08-19 02:06_

---

_Review comment by @zanieb on `crates/pep508-rs/src/marker/tree.rs`:1902 on 2024-08-19 02:06_

Yeah.. we but someone could do `python_version in '3.10.*'` and that should be true sometimes since it's a substring match — anyway clearly some caveats around this implementation I'll try to make the tests clearer.

---

_@charliermarsh reviewed on 2024-08-19 02:16_

---

_Review comment by @charliermarsh on `crates/pep508-rs/src/marker/tree.rs`:1902 on 2024-08-19 02:16_

Lol true

---

_Comment by @konstin on 2024-08-19 10:11_

> Created https://github.com/pubgrub-rs/pubgrub/issues/249 to track better range union performance, which I left a couple TODOs about.

Is this something we need for this PR (because shows up in profiles)?

---

_Comment by @zanieb on 2024-08-19 14:04_

@konstin no, I didn't check if it has an affect on any profiles — Charlie was just concerned about the pattern (and I agree, it seems like it's probably quadratic).

---

_Merged by @zanieb on 2024-08-19 14:10_

---

_Closed by @zanieb on 2024-08-19 14:10_

---

_Branch deleted on 2024-08-19 14:10_

---
