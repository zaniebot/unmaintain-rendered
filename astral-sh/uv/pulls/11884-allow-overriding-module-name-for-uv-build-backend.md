```yaml
number: 11884
title: Allow overriding module name for uv build backend
type: pull_request
state: merged
author: csachs
labels:
  - enhancement
assignees: []
merged: true
base: main
head: feature/build-backend-module-name
created_at: 2025-03-01T14:05:38Z
updated_at: 2025-03-07T14:20:00Z
url: https://github.com/astral-sh/uv/pull/11884
synced_at: 2026-01-10T11:10:39Z
```

# Allow overriding module name for uv build backend

---

_Pull request opened by @csachs on 2025-03-01 14:05_

Thank you for uv, it has game-changer capabilities in the field of Python package and environment maangement!

## Summary

This is a small PR adding the option `module-name` (`tool.uv.build-backend.module-name`) to the uv build backend ( https://github.com/astral-sh/uv/issues/8779 ).

Currently, the uv build backend will assume that the module name matches the (dash to underdash-transformed) package name. In some packaging scenarios this is not the case, and currently there exists no possibility to override it, which this PR addresses.

From the main issue ( https://github.com/astral-sh/uv/issues/8779 ) I could not tell if there is any extensive roadmap or plans how to implement more complex scenarios, hence this PR as a suggestion for a small feature with a big impact for certain scenarios.

I am new to Rust, I hope the borrow/reference usage is correct.

## Test Plan

So far I tested this at an example, if desired I can look into extending the tests.

Fixes #11428

---

_Assigned to @konstin by @zanieb on 2025-03-01 16:34_

---

_Label `enhancement` added by @konstin on 2025-03-03 11:12_

---

_Review requested from @BurntSushi by @konstin on 2025-03-04 14:41_

---

_Review requested from @BurntSushi by @konstin on 2025-03-04 14:41_

---

_Review comment by @BurntSushi on `Cargo.lock`:1844 on 2025-03-04 15:03_

Is this version bump necessary? I think we usually let renovatebot do this.

Not a big deal to include it here I guess, but if it isn't necessary it's probably best to kick this out.

---

_Review comment by @BurntSushi on `crates/uv-build-backend/src/metadata.rs`:850 on 2025-03-04 15:05_

cc @konstin on the terminology here. I would expect this to be _unspecified behavior_ and not _undefined_.

---

_Review comment by @BurntSushi on `crates/uv-build-backend/src/source_dist.rs`:74 on 2025-03-04 15:08_

I would say, "This should never result in an error." Otherwise I think just "should never happen" is a little confusing and ambiguous. I thought it meant that `settings.module_name` could never be `None` (which confused me).

I would also therefore be tempted to do an `expect("...")` here, but I'm not 100% on the relevant invariants here. Returning an error seems fine.

---

_Review comment by @BurntSushi on `crates/uv-build-backend/src/wheel.rs`:136 on 2025-03-04 15:09_

Same comment as above applies here too I think.

---

_Review comment by @BurntSushi on `crates/uv-pypi-types/src/identifier.rs`:19 on 2025-03-04 15:11_

```suggestion
        "Invalid first character `{first}` for identifier `{identifier}`, expected an underscore or an alphabetic character"
```

---

_Review comment by @BurntSushi on `crates/uv-pypi-types/src/identifier.rs`:10 on 2025-03-04 15:11_

What does wrong if we say an identifier is valid but Python's rules says it isn't?

---

_Review comment by @BurntSushi on `crates/uv-pypi-types/src/identifier.rs`:12 on 2025-03-04 15:11_

This could probably be a `Box<str>`.

A minor point and perhaps premature, but these things have a tendency to build up over time.

---

_Review comment by @BurntSushi on `crates/uv-pypi-types/src/identifier.rs`:23 on 2025-03-04 15:12_

```suggestion
        "Invalid character `{invalid_char}` at position {pos} for identifier `{identifier}`, \
         expected an underscore or an alphanumeric character"
```

---

_Review comment by @BurntSushi on `crates/uv-pypi-types/src/identifier.rs`:124 on 2025-03-04 16:36_

```suggestion
        assert_snapshot!(
            Identifier::from_str("1foo").unwrap_err(),
            @"Invalid first character `1` at for `1foo`, expected an underscore or an alphabetic character",
        );
```

And then do the same for the other assertions. Otherwise the lines get very long. Long lines as a result of snapshot strings are fine, because those are more annoying to fix. But otherwise, adding some line breaks, makes the code generating the snapshot a little easier to read IMO.

---

_Review comment by @BurntSushi on `crates/uv/tests/it/build_backend.rs`:283 on 2025-03-04 16:37_

Can you add a little more detail about what this test is doing?

---

_Review comment by @BurntSushi on `crates/uv/tests/it/build_backend.rs`:367 on 2025-03-04 16:40_

Can you say why this doesn't work despite there being a `src/foo/__init__.py`?

---

_@BurntSushi requested changes on 2025-03-04 16:40_

I think this generally looks good to me, but I have some requests, nits and questions. :-)

---

_@konstin reviewed on 2025-03-06 08:28_

---

_Review comment by @konstin on `crates/uv-build-backend/src/source_dist.rs`:74 on 2025-03-06 08:28_

We're very permissive with things that can never happen in other place (returning errors instead of unwrapping), which I'm mirroring here.

---

_@konstin reviewed on 2025-03-06 08:31_

---

_Review comment by @konstin on `crates/uv-pypi-types/src/identifier.rs`:10 on 2025-03-06 08:31_

Python's and Rust's documentation reference different parts of the unicode standard for what's an allowed letter, but I strongly suspect that no one will actually into a case where they disagree.

---

_@BurntSushi approved on 2025-03-06 12:57_

---

_Merged by @konstin on 2025-03-07 14:20_

---

_Closed by @konstin on 2025-03-07 14:20_

---
