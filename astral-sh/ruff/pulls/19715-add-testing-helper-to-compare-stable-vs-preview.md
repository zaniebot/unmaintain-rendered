```yaml
number: 19715
title: Add testing helper to compare stable vs preview snapshots
type: pull_request
state: merged
author: vivster7
labels:
  - testing
assignees: []
merged: true
base: main
head: feature/test-snapshot-diff
created_at: 2025-08-03T22:23:40Z
updated_at: 2025-08-22T17:53:31Z
url: https://github.com/astral-sh/ruff/pull/19715
synced_at: 2026-01-10T17:46:21Z
```

# Add testing helper to compare stable vs preview snapshots

---

_Pull request opened by @vivster7 on 2025-08-03 22:23_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
This PR implements a diff test helper `assert_diagnostics_diff` as described in #19351. The diff file includes both the settings ( e.g. `+linter.preview = enabled`) and the snapshot data itself.

The current implementation looks for each old diagnostic in the new snapshot. This works when the preview behavior adds/removes a couple diagnostics. This implementation does not work well when every diagnostic is modified (e.g. a "fix" is added). https://github.com/astral-sh/ruff/pull/19715#discussion_r2259410763 has ideas for future improvements to this implementation.

The example usage in this PR writes the diff to `preview_diff` file instead of `preview` file, which might be a useful convention to keep.


## Test Plan
- Included a unit test at: https://github.com/astral-sh/ruff/pull/19715/files#diff-d49487fe3e8a8585529f62c2df2a2b0a4c44267a1f93d1e859dff1d9f8771d36R523
- Example usage of this new test helper: https://github.com/astral-sh/ruff/pull/19715/files#diff-2a33ac11146d1794c01a29549a6041d3af6fb6f9b423a31ade12a88d1951b0c2R1

<!-- How was it tested? -->


---

_@vivster7 reviewed on 2025-08-03 22:27_

---

_Review comment by @vivster7 on `crates/ruff_linter/src/test.rs`:64 on 2025-08-03 22:27_

I wanted to try and include a list of the settings that differed between before/after -- but couldn't quite figure out a good way to only include the differences. Printing out all the settings was too verbose.

For now, assuming the name of the snapshot file `preview_diff__` might be enough to indicate what the difference in settings are.

---

_Marked ready for review by @vivster7 on 2025-08-03 23:06_

---

_Review requested from @dylwil3 by @MichaReiser on 2025-08-04 05:55_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/test.rs`:45 on 2025-08-04 13:41_

Do we need to keep track of this? I think we probably only want to display added/removed to the user. (One of the points of this helper is to reduce the size of some of our testing snapshots that we have to maintain)

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/test.rs`:53 on 2025-08-04 13:42_

See previous comment

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/test.rs`:77 on 2025-08-04 13:51_

I wonder if a more naive algorithm would avoid some allocations at the cost of maybe more iterations - especially since we only care about added/removed. Something like:

```rust
    let mut removed = Vec::new();

    for old_diag in before {
        let Some(pos) = after.iter().position(|diag| diag == &old_diag) else {
            removed.push(old_diag);
            continue;
        };
        after.remove(pos);
    }

    Ok(DiagnosticsDiff {
        added: after,
        removed,
    })
```
(I haven't verified this, but under the assumption that the diagnostics are roughly in order then most of the successful searches should be quick - and most of the time most of the diagnostics will be unchanged).

Of course another option is to use the functionality from the crate `similar` for diffing sequences. Probably all three will be fine, but maybe it's worth a quick benchmark on a large snapshot file to see if it makes a difference.

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/test.rs`:64 on 2025-08-04 13:56_

Have you tried something like displaying the text diff of the (pretty) debug printed settings?

---

_@dylwil3 requested changes on 2025-08-04 14:01_

Thanks this looks great! A few suggestions for improvement. Also, could you include in the PR the use of this helper in a few existing snapshots? It would be helpful to see what effect it has on the size and readability/maintainability of the snapshots. Let me know if you want me to hunt for a good example.

---

_Comment by @github-actions[bot] on 2025-08-04 14:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @vivster7 on 2025-08-06 06:09_

Thanks for the feedback! Will get this back up for review again tomorrow

---

_Label `testing` added by @MichaReiser on 2025-08-06 06:19_

---

_@vivster7 reviewed on 2025-08-07 06:47_

---

_Review comment by @vivster7 on `crates/ruff_linter/src/test.rs`:77 on 2025-08-07 06:47_

I wasn't totally sure how to benchmark this -- but running the test with either implementation runs in 0.03s, so I think more or less they are both performant enough. Happy to go with the simpler approach here.
```
(ruff) ➜  ruff git:(feature/test-snapshot-diff) cargo test --package ruff_linter --lib -- rules::flake8_commas::tests::preview_rules
    Finished `test` profile [unoptimized + debuginfo] target(s) in 0.12s
     Running unittests src/lib.rs (target/debug/deps/ruff_linter-7958dd100e6e3896)

running 2 tests
test rules::flake8_commas::tests::preview_rules::path_new_com81_py_expects ... ok
test rules::flake8_commas::tests::preview_rules::path_new_com81_syntax_error_py_expects ... ok

test result: ok. 2 passed; 0 failed; 0 ignored; 0 measured; 2478 filtered out; finished in 0.03s
```

I did try to implement a third approach with `similar::capture_diff_slices` but `Diagnostics` doesn't implement `std::cmp::Ord`.  We could maybe implement `std::cmp::Ord` based on `diagnostic.expect_range().start()`, but wasn't confident enough to go down that route.

We could also serialize all the Diagnostics to Text and run `similar::TextDiff` -- see more on this idea at: https://github.com/astral-sh/ruff/pull/19715#discussion_r2259260904

---

_@vivster7 reviewed on 2025-08-07 06:47_

---

_Review comment by @vivster7 on `crates/ruff_linter/src/test.rs`:64 on 2025-08-07 06:47_

Ah, great idea! I think it works quite well: https://github.com/astral-sh/ruff/pull/19715/files#diff-2a33ac11146d1794c01a29549a6041d3af6fb6f9b423a31ade12a88d1951b0c2R4-R6

---

_@vivster7 reviewed on 2025-08-07 06:49_

---

_Review comment by @vivster7 on `crates/ruff_linter/src/rules/flake8_commas/snapshots/ruff_linter__rules__flake8_commas__tests__preview_diff__COM81.py.snap`:1 on 2025-08-07 06:49_

this file replaces crates/ruff_linter/src/rules/flake8_commas/snapshots/ruff_linter__rules__flake8_commas__tests__preview__COM81.py.snap and works quite well. replaces 1000 line snapshot with 130 lines. I think it's much clearer what the preview rule is doing.

---

_Review comment by @vivster7 on `crates/ruff_linter/src/rules/perflint/snapshots/ruff_linter__rules__perflint__tests__preview_diff__PERF401_PERF401.py.snap`:1 on 2025-08-07 06:51_

This snapshot did not work well and I didn't delete the corresponding preview snapshot file. This might be a case where doing just a text diff would work better? Or maybe `assert_diagnostics_diff` is only useful for some preview snapshots, but not all of them?

---

_@vivster7 reviewed on 2025-08-07 06:51_

---

_Review requested from @dylwil3 by @vivster7 on 2025-08-07 06:52_

---

_@MichaReiser reviewed on 2025-08-07 07:37_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/perflint/snapshots/ruff_linter__rules__perflint__tests__preview_diff__PERF401_PERF401.py.snap`:1 on 2025-08-07 07:37_

If I understand it correctly, the difference here is only due to the rule having a fix in preview. 

I'm not suggesting that we have to do this as part of this PR (but we can). The test utility could try to match up diagnostics by rule code, line, and column number. If they're identical, put them into a `changed` section. 

That raises the question of how we render changed diagnostics. A simple diffing of the output is probably the easiest to start. I suspect that reviewing the fixes might be a bit confusing because it becomes a diff of a diff. 

We could try to be clever and try to determine for each matched diagnostic pair:

* Is it a new fix (if so, render the new fix with a comment saying that this is the case)
* Was a fix removed
* Did the fix change: Render both fixes separately
* Did any other part of the diagnostic change

The challenge here is that you also have to deal with combinations. 

---

_@dylwil3 requested changes on 2025-08-11 03:53_

Thanks this looks excellent! It's up to you whether you'd like to implement Micha's suggested improvements as part of this PR - I think what you have is already a big improvement in many cases. 

I'm happy to approve once the merge conflicts are resolved.

---

_Comment by @vivster7 on 2025-08-16 07:28_

I’ve spent some time thinking of ways to address Micha’s feedback, but I don’t have something concrete yet.

If this is useful in its current form, I’d be happy to get it merged as well.

Sorry for the slow turnaround, I’m out of town for a bit, but will have conflicts resolved on Sunday.

---

_Review requested from @dylwil3 by @vivster7 on 2025-08-20 03:48_

---

_@dylwil3 approved on 2025-08-22 17:49_

Thanks for your work on this, looks great!

---

_Merged by @dylwil3 on 2025-08-22 17:49_

---

_Closed by @dylwil3 on 2025-08-22 17:49_

---
