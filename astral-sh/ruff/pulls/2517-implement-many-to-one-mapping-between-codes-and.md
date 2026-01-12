```yaml
number: 2517
title: Implement many-to-one mapping between codes and rules
type: pull_request
state: merged
author: not-my-profile
labels: []
assignees: []
merged: true
base: main
head: many-to-one
created_at: 2023-02-03T05:56:39Z
updated_at: 2023-02-14T21:16:13Z
url: https://github.com/astral-sh/ruff/pull/2517
synced_at: 2026-01-12T15:55:08Z
```

# Implement many-to-one mapping between codes and rules

---

_@not-my-profile_

Implements #2186.

---

_Comment by @charliermarsh on 2023-02-03 21:13_

This is really exciting.

I've read through some of the code and commits but not the entirety of the change (yet). From my testing, it seems like every rule has one "primary" code. So `useless-object-inheritance` is mapped to `PLR0205`, `PIE792`, and `UP004`. And no matter how you `--select` it, it's always reported as `PIE792`. Similarly, you have to use `PIE792` to `# noqa` it. I thinks this could be confusing, to `--select UP` and see `PIE792` violations. How do you view it? Is that an accurate description of current behavior?


---

_Comment by @charliermarsh on 2023-02-03 21:24_

I'm not sure what I would expect as a user. I could imagine a few things:

1. If I `--select UP --select PIE`, I see the error twice, once under both codes. (If I `--select UP`, I see the error once, under `UP004`; if I `--select PIE`, I see the error once, under `PIE792`.)
2. If I `--select UP --select PIE`, I see the error once, under the "first" (?) code, or under the preferred code as is implicitly implemented here. (If I `--select UP`, I see the error once, under `UP004`; if I `--select PIE`, I see the error once, under `PIE792`.)

I don't know how I'd expect `noqa` violations to work: should _any_ matching code ignore the violation despite the selector that was used to enable it? Or should I be required to add a `noqa` for every matching code individually?


---

_Comment by @not-my-profile on 2023-02-04 03:20_

> From my testing, it seems like every rule has one "primary" code. I thinks this could be confusing, to --select UP and see PIE792 violations. How do you view it? Is that an accurate description of current behavior?

Yes that's how it currently works. I'd expect us to switch to human-friendly rule names #1773, so no matter which code you used to select in the future ruff will always only report the human-friendly rule name. Until we make that switch the behavior will be a bit confusing, but I don't see a good way around that.

> see the error twice

I am positive that we absolutely do not want to report any error multiple times under any circumstance.

Note that it's totally possible that a rule is enabled via multiple codes, I don't think we want to report multiple codes for a single violation, so we have to pick one ... until we have come up with rule naming guidelines and renamed our rules accordingly.

> should any matching code ignore the violation despite the selector that was used to enable it?

Yes. A rule can be identified via multiple codes. Which code you used to enable a rule doesn't matter.

---

_Comment by @charliermarsh on 2023-02-04 03:46_

I worry that running `--select UP` and seeing a `PIE792` violation is a confusing enough experience that I'd want to really consider whether we enable these at all prior to migrating to human-friendly rule names.

---

_Comment by @charliermarsh on 2023-02-12 05:22_

I'm hesitant to merge this as-is due to some of the confusing behaviors that users will experience around aliasing (e.g., the `--select UP`-to-`PIE` scenario described above).

I don't have great answers for them yet, but I know that if we ship this as-is, it will be confusing for users, and we'll get a lot of feedback and questions stemming from that confusion.

If we want to merge this while giving ourselves time to figure out the best solution for aliasing, what we _could_ do is merge this change but not yet implement any of the actual aliases (apart from, perhaps, deprecating rules, like `BLE001`).


---

_Comment by @not-my-profile on 2023-02-12 06:04_

Right that makes sense. I think the course of action is:

1. Rename our rules to match the naming convention ... this does entail the merging of several rules (see [#2714](https://www.github.com/charliermarsh/ruff/issues/2714)).
   (We do this first since after the next step renaming rules will be a breaking change.)
2. Allow rules to be selected by their name and report violations by their name.
3. Enable the many-to-one mapping.

I already rebased this PR yesterday, which was quite work-intensive since #2583 had landed in between. Further rebasing shouldn't take that much effort, so I'd be alright with leaving this PR open ... but yeah it would be nice to merge this just so that I don't have to deal with merging other changes that may crop up.

I could add an assert statement to the `map_codes` proc macro to assert that at most one code is mapped to one rule ... then we could merge this without any UX changes[^1] and simply remove the assert statements once we get to the above mentioned 3rd step.

[^1]: Aside from the C, C9, T, T1, T2 prefix deprecations in the 7th commit.

---

_Comment by @sbrugman on 2023-02-13 18:45_

(Heads-up: I will rename the pathlib rules in https://github.com/charliermarsh/ruff/pull/2348 to os-path - the thing we detect)

---

_Comment by @not-my-profile on 2023-02-14 04:44_

I think that landing this PR, will take some coordination that this will be merged before any other `registry.rs` changes, since this PR changes the format of the frequently edited `registry.rs` file.

---

_Comment by @charliermarsh on 2023-02-14 05:02_

👍 I’m happy to merge this next assuming it doesn’t change the UX right now — is that the case? I have to re-read the code but I’ll review tomorrow after the JetBrains webinar, and hopefully can merge tomorrow afternoon or evening.

---

_Comment by @not-my-profile on 2023-02-14 05:06_

Ok great. Yes after I have dropped the last commit demonstrating the mapping and added the aforementioned assert statement there shouldn't be any UX changes.

---

_Comment by @charliermarsh on 2023-02-14 20:43_

(Returning to this now.)

---

_Comment by @not-my-profile on 2023-02-14 20:54_

Let me know if you have questions.

---

_@charliermarsh reviewed on 2023-02-14 20:59_

---

_Review comment by @charliermarsh on `ruff.schema.json`:1399 on 2023-02-14 20:59_

So these work on the CLI (based on my testing), what's the rationale for excluding them?

---

_@charliermarsh reviewed on 2023-02-14 21:00_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/printer.rs`:605 on 2023-02-14 21:00_

Removed from inline just for clarity? Or some other reason?

---

_@charliermarsh reviewed on 2023-02-14 21:01_

---

_Review comment by @charliermarsh on `crates/ruff/src/settings/pyproject.rs`:228 on 2023-02-14 21:01_

So this is a result of the prefix now being encoded via `codes::Ruff`, and so the identifier itself starts with a number, and so we need the underscore. Is that correct?

---

_Review comment by @not-my-profile on `ruff.schema.json`:1399 on 2023-02-14 21:02_

They only continue to work because I added redirects, see the "Update JSON schema" commit.

They are no longer generated by `cargo dev generate-json-schema` since these are partial linter prefixes ... I'd say we should consider them deprecated.

---

_@not-my-profile reviewed on 2023-02-14 21:02_

---

_Review comment by @charliermarsh on `ruff.schema.json`:1399 on 2023-02-14 21:04_

Is this not the McCabe-equivalent of (e.g.) `--select B` for `flake8-bugbear`?

---

_@charliermarsh reviewed on 2023-02-14 21:04_

---

_@not-my-profile reviewed on 2023-02-14 21:04_

---

_Review comment by @not-my-profile on `crates/ruff/src/settings/pyproject.rs`:228 on 2023-02-14 21:04_

Yes, see `get_prefix_ident` in `ruff_macros/src/rule_code_prefix.rs`:

> // Identifiers in Rust may not start with a number.

---

_@charliermarsh reviewed on 2023-02-14 21:05_

---

_Review comment by @charliermarsh on `ruff.schema.json`:1399 on 2023-02-14 21:05_

(As an aside, would be nice to just remove these conflicts at some point.)

---

_@not-my-profile reviewed on 2023-02-14 21:06_

---

_Review comment by @not-my-profile on `crates/ruff_cli/src/printer.rs`:605 on 2023-02-14 21:06_

Removed from inline due to borrow checking reasons. Otherwise borrowchk complains due to returning a temporary reference to an owned value.

---

_@not-my-profile reviewed on 2023-02-14 21:07_

---

_Review comment by @not-my-profile on `ruff.schema.json`:1399 on 2023-02-14 21:07_

> Is this not the McCabe-equivalent of (e.g.) --select B for flake8-bugbear?

No as per `#[prefix = "C90"] McCabe` in `registry.rs` the equivalent is C90.

---

_@not-my-profile reviewed on 2023-02-14 21:08_

---

_Review comment by @not-my-profile on `ruff.schema.json`:1399 on 2023-02-14 21:08_

Note that this is also consistent with the upstream mccabe:

https://github.com/PyCQA/mccabe/blob/master/setup.py#L38

---

_Comment by @charliermarsh on 2023-02-14 21:10_

Cool this looks good to me -- merging...

---

_Merged by @charliermarsh on 2023-02-14 21:16_

---

_Closed by @charliermarsh on 2023-02-14 21:16_

---
