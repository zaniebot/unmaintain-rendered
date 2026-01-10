```yaml
number: 21623
title: apply range suppressions to filter diagnostics
type: pull_request
state: merged
author: amyreese
labels:
  - internal
  - suppression
assignees: []
merged: true
base: main
head: amy/suppress-diagnostics
created_at: 2025-11-25T02:02:01Z
updated_at: 2025-12-09T00:12:02Z
url: https://github.com/astral-sh/ruff/pull/21623
synced_at: 2026-01-10T16:42:11Z
```

# apply range suppressions to filter diagnostics

---

_Pull request opened by @amyreese on 2025-11-25 02:02_

Builds on range suppressions from https://github.com/astral-sh/ruff/pull/21441 

Filters diagnostics based on parsed valid range suppressions.

Issue: #3711 


---

_Comment by @astral-sh-bot[bot] on 2025-11-25 02:07_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Review comment by @MichaReiser on `crates/ruff_linter/src/linter.rs`:331 on 2025-11-25 10:47_

I think we should only apply the filtering if `noqa_is_enabled`. I'm not a 100% sure but I think this is more a terminology issue. The CLI has a `--ignore-noqa` comment which you can use to show all diagnostics even if noqa's are disabled (https://github.com/astral-sh/ruff/issues/3070). I think we want this flag to also toggle `ruff:disable`/`ruff:enable`

I also suggest reviewing the other call sites where we set `noqa::Disabled` to see if that should apply to `ruff:disable` as well

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/linter.rs`:335 on 2025-11-25 10:48_

I think we should move the logic into `check_noqa` and rename the method to `check_suppressions`. This should also avoid having to iterate over all diagnostics twice, which can be expensive if there are many

---

_@MichaReiser reviewed on 2025-11-25 10:48_

I think we should add some tests now. `check_path` is called from our testing framework, meaning we should now be able to write a test similar to

https://github.com/astral-sh/ruff/blob/850398dada852843e10f6a118127a6b801ed476d/crates/ruff_linter/src/rules/ruff/mod.rs#L294-L305

This could also be used as a replacement for some of the tests I asked for in your previous PR.

I also suggest adding some CLI tests verifying that this PR doesn't break in combination with `--add-noqa`, `--remove-noqa` `--ignore-noqa` etc.

---

_Label `suppression` added by @MichaReiser on 2025-11-25 10:48_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/suppression.rs`:110 on 2025-11-25 17:47_

We should probably at a fast path here to exit immediately if `self.valid` is empty 

---

_@MichaReiser reviewed on 2025-11-25 17:52_

---

_Renamed from "[ruff] apply range suppressions to filter diagnostics" to "apply range suppressions to filter diagnostics" by @MichaReiser on 2025-11-26 07:06_

---

_Label `internal` added by @MichaReiser on 2025-11-26 07:06_

---

_Comment by @amyreese on 2025-12-02 01:29_

> I also suggest adding some CLI tests verifying that this PR doesn't break in combination with `--add-noqa`, `--remove-noqa` `--ignore-noqa` etc.

Is there an existing set of CLI tests for noqa that I can use as an example here?

---

_Review requested from @MichaReiser by @amyreese on 2025-12-02 01:31_

---

_Comment by @MichaReiser on 2025-12-02 08:28_

> Is there an existing set of CLI tests for noqa that I can use as an example here?

There are some here https://github.com/astral-sh/ruff/blob/81f6ec16579151d6ad41b2a071120ccc7ea1261f/crates/ruff/tests/cli/lint.rs#L1444

I couldn't find any for `--ignore-noqa`, and `--remove-noqa` doesn't exist, it's just fix and you can find some tests in the above mentioned test file

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/ruff/suppressions.py`:25 on 2025-12-02 08:29_

```suggestion
    # One should be ignored by the range suppression, and
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/ruff/suppressions.py`:33 on 2025-12-02 08:29_

```suggestion
    # Neither of these are ignored and a warning is
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/ruff/suppressions.py`:21 on 2025-12-02 08:30_

I think it would be good to add/move some of the more advanced test cases involving blocks and make them integration tests instead. 

---

_@MichaReiser approved on 2025-12-02 08:33_

This looks good. But we should look into the performance regression before landing this PR (can be fixed as a separate PR but we can't land this until we fixed the regression)

---

_Comment by @amyreese on 2025-12-02 17:00_

Note: add `--preview` gating so we can land this without waiting for diagnostics etc

---

_@MichaReiser reviewed on 2025-12-04 07:46_

---

_Review comment by @MichaReiser on `crates/ruff/tests/cli/lint.rs`:1744 on 2025-12-04 07:46_

We try to avoid reading fixture files in CLI tests as it makes the tests depend on each other and it can also become harder to reason about what we're testing here. Would it be possible to extract the specific case you want to test from `noqa.py`?

The tests here also don't need to be exhaustive. We can add more exhaustive tests to `add_noqa` (I think we have some integration tests, right?)

---

_@MichaReiser reviewed on 2025-12-04 07:50_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/linter.rs`:432 on 2025-12-04 07:50_

We now end up extracting the suppressions twice: Once in `check_path` and once here. We might need to pass the suppressions as part of `check_path`

---

_@MichaReiser reviewed on 2025-12-04 07:56_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:42 on 2025-12-04 07:56_

We also call this function from our tests. Would it make sense to lift `Suppressions` out of `generate_noqa_edits` and instead require the caller to pass the suppressions?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/mod.rs`:333 on 2025-12-04 07:57_

This might be a good case for `assert_diagnostics_diff` which snapshots the difference between two settings: E.g. between preview on and off

---

_@MichaReiser reviewed on 2025-12-04 07:57_

---

_@amyreese reviewed on 2025-12-04 16:52_

---

_Review comment by @amyreese on `crates/ruff/tests/cli/lint.rs`:1744 on 2025-12-04 16:52_

This was a copy-paste of the tests directly above and below it that exercise `--add-noqa`.

---

_@MichaReiser approved on 2025-12-06 12:37_

---

_Merged by @amyreese on 2025-12-09 00:11_

---

_Closed by @amyreese on 2025-12-09 00:11_

---

_Branch deleted on 2025-12-09 00:12_

---
