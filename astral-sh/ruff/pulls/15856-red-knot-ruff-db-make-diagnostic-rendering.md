```yaml
number: 15856
title: "[red-knot] ruff_db: make diagnostic rendering prettier"
type: pull_request
state: merged
author: BurntSushi
labels:
  - ty
assignees: []
merged: true
base: main
head: ag/red-knot-prototype-diagnostics
created_at: 2025-01-31T16:49:57Z
updated_at: 2025-02-01T14:15:50Z
url: https://github.com/astral-sh/ruff/pull/15856
synced_at: 2026-01-12T15:55:52Z
```

# [red-knot] ruff_db: make diagnostic rendering prettier

---

_@BurntSushi_

This change does a simple swap of the existing renderer for one that
uses our vendored copy of `annotate-snippets`. We don't change anything
about the diagnostic data model, but this alone already makes
diagnostics look a lot nicer!

Here's an example of what it looks like now:

![diag-example](https://github.com/user-attachments/assets/5c66779c-caeb-4254-bbd2-68fd2d76081c)


---

_Review requested from @carljm by @BurntSushi on 2025-01-31 16:49_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-01-31 16:49_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-01-31 16:49_

---

_Review requested from @sharkdp by @BurntSushi on 2025-01-31 16:49_

---

_Renamed from "ruff_db: make diagnostic rendering prettier" to "[red-knot] ruff_db: make diagnostic rendering prettier" by @BurntSushi on 2025-01-31 16:50_

---

_Label `red-knot` added by @AlexWaygood on 2025-01-31 16:50_

---

_Comment by @github-actions[bot] on 2025-01-31 16:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@AlexWaygood approved on 2025-01-31 17:12_

This looks fantastic to me!

---

_Review comment by @carljm on `crates/red_knot/tests/cli.rs`:241 on 2025-01-31 18:06_

Ok really dumb question, but why does annotate-snippets choose `^^^^` in some cases but `----` here?

---

_@carljm approved on 2025-01-31 18:08_

So good!

---

_@BurntSushi reviewed on 2025-01-31 18:22_

---

_Review comment by @BurntSushi on `crates/red_knot/tests/cli.rs`:241 on 2025-01-31 18:22_

Good question actually! It chooses a different mark based on the severity level:

https://github.com/astral-sh/ruff/blob/b0b8b062410afdee0c9948a23fc99de8e5a406b6/crates/ruff_annotate_snippets/src/renderer/display_list.rs#L716-L723

Honestly kinda meh on this myself since it's somewhat non-obvious to me.

---

_Comment by @carljm on 2025-01-31 18:32_

Oh, you're gonna need to update the output-validation in our benchmark somehow...

---

_Comment by @BurntSushi on 2025-01-31 18:35_

Yeah working on that hah. Trying `insta`.

---

_Comment by @codspeed-hq[bot] on 2025-01-31 19:48_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/ag%2Fred-knot-prototype-diagnostics)

### Merging #15856 will **not alter performance**

<sub>Comparing <code>ag/red-knot-prototype-diagnostics</code> (07c80e8) with <code>main</code> (fab86de)</sub>



### Summary

`✅ 32` untouched benchmarks  





---

_@carljm reviewed on 2025-01-31 20:07_

---

_Review comment by @carljm on `crates/ruff_benchmark/benches/red_knot.rs`:254 on 2025-01-31 20:07_

Given that we have the diagnostic data structures here, why should we render them using `display` at all, now that that representation is so inconvienent for comparison and update? (Not that it was ever convenient.) Couldn't we just compare against, like, a tuple of the key fields instead?

---

_@sharkdp reviewed on 2025-01-31 20:11_

---

_Review comment by @sharkdp on `crates/red_knot/tests/cli.rs`:241 on 2025-01-31 20:11_

I think it makes more sense once you have e.g. (multiple) "Info" and "Error" annotations in a single diagnostic. Example from my own project (thinking about it, the function parameter annotation on top here should probably also be "info", but anyway):

![image](https://github.com/user-attachments/assets/e538e634-c2f6-4c38-953a-cf9634e57a27)

I kind of like how the informational parts are just underlined with `---` and the actual error is underlined with `^^^` as if to say: look, this is what you need to change.

---

_@carljm reviewed on 2025-01-31 20:17_

---

_Review comment by @carljm on `crates/ruff_benchmark/benches/red_knot.rs`:160 on 2025-01-31 20:17_

On second thought, I think the bogus codspeed numbers are telling us why we can't make this change. It turns the "cold" benchmark into a "no-op incremental" benchmark, which, not surprisingly, is very fast indeed. That's probably why the full diagnostics assertion was included in the benchmark. I think we either need to put it back the way it was, or do it as a totally separate prior step, not part of the per-iteration `setup_case` at all.

---

_Review comment by @BurntSushi on `crates/ruff_benchmark/benches/red_knot.rs`:160 on 2025-01-31 20:32_

Aye. I'll switch it back. I guess it should be fine now since I'm not using `insta` any more. Although I guess the diagnostic strings are now longer, so comparisons will be longer.

---

_@BurntSushi reviewed on 2025-01-31 20:32_

---

_@BurntSushi reviewed on 2025-01-31 20:33_

---

_Review comment by @BurntSushi on `crates/ruff_benchmark/benches/red_knot.rs`:254 on 2025-01-31 20:33_

Yeah I'll look into this. I was mostly just looking to do the most minimal thing possible, but yeah, if you're cool with switching this out to just doing a comparison of key fields (range and diagnostic id I guess) then that seems fine.

---

_@carljm reviewed on 2025-01-31 20:35_

---

_Review comment by @carljm on `crates/ruff_benchmark/benches/red_knot.rs`:254 on 2025-01-31 20:35_

Yeah that seems definitely better to me -- the full diagnostic strings look like they'd be an absolute nightmare to update without help from `insta`.

---

_@carljm reviewed on 2025-01-31 20:36_

---

_Review comment by @carljm on `crates/ruff_benchmark/benches/red_knot.rs`:160 on 2025-01-31 20:36_

If we go with key fields instead of the full string, it should be pretty similar to before?

---

_Review comment by @carljm on `crates/ruff_benchmark/benches/red_knot.rs`:160 on 2025-01-31 20:39_

Though doing it as a separate step (not in `setup_case`) also seems pretty easy and fine.

---

_@carljm reviewed on 2025-01-31 20:39_

---

_@BurntSushi reviewed on 2025-01-31 20:39_

---

_Review comment by @BurntSushi on `crates/ruff_benchmark/benches/red_knot.rs`:160 on 2025-01-31 20:39_

Yeah sorry I made that comment before the other one.

---

_@BurntSushi reviewed on 2025-01-31 21:10_

---

_Review comment by @BurntSushi on `crates/red_knot/tests/cli.rs`:241 on 2025-01-31 21:10_

Ah yeah that's a good point to consider too. I'll leave it as-is for now. This was just meant to get some decent rendering in. We can absolutely iterate on the exact format and what not.

---

_Review requested from @carljm by @BurntSushi on 2025-01-31 21:10_

---

_Review comment by @carljm on `crates/ruff_benchmark/benches/red_knot.rs`:35 on 2025-01-31 21:14_

I think we may want `.message()` string in here too, but I'm not sure how much difference it will make going forward, and we can always add it back in as a separate change.

---

_@carljm approved on 2025-01-31 21:14_

---

_@BurntSushi reviewed on 2025-01-31 21:21_

---

_Review comment by @BurntSushi on `crates/ruff_benchmark/benches/red_knot.rs`:35 on 2025-01-31 21:21_

Done!

Can you say more about why `.message()` is desirable here? Do you mean it's desirable to test and validate, or do you mean it's desirable from a measurement perspective?

---

_Merged by @BurntSushi on 2025-01-31 21:37_

---

_Closed by @BurntSushi on 2025-01-31 21:37_

---

_Branch deleted on 2025-01-31 21:37_

---

_@carljm reviewed on 2025-01-31 21:43_

---

_Review comment by @carljm on `crates/ruff_benchmark/benches/red_knot.rs`:35 on 2025-01-31 21:43_

It's desirable from a usability perspective of validating in PR review that the diagnostics removed from (or added to) this assertion list are sensibly related to the red-knot changes.

---

_@BurntSushi reviewed on 2025-02-01 00:39_

---

_Review comment by @BurntSushi on `crates/ruff_benchmark/benches/red_knot.rs`:35 on 2025-02-01 00:39_

Ah I think I understand now. Thank you for explaining!

---

_@MichaReiser reviewed on 2025-02-01 08:52_

---

_Review comment by @MichaReiser on `crates/red_knot/tests/cli.rs`:109 on 2025-02-01 08:52_

This demonstrates that we should start capturing at least some full diagnostics in mdtests because highlighting the entire range here seems incorrect (compared to the `import utils` case below where it only highlights the name. I'll create an issue

---

_@BurntSushi reviewed on 2025-02-01 14:13_

---

_Review comment by @BurntSushi on `crates/red_knot/tests/cli.rs`:109 on 2025-02-01 14:13_

Yeah I agree. We need a fair bit of test coverage beyond what we have. I think a nice short-term target would be at least one test for every type of diagnostic.

---

_@AlexWaygood reviewed on 2025-02-01 14:15_

---

_Review comment by @AlexWaygood on `crates/red_knot/tests/cli.rs`:109 on 2025-02-01 14:15_

I agree that we need a lot more tests that capture the full diagnostic output, but I'm not convinced this should necessarily be an mdtest feature (https://github.com/astral-sh/ruff/issues/15867#issuecomment-2628959022)

---
