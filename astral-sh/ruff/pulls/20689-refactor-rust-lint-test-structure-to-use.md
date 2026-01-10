```yaml
number: 20689
title: Refactor Rust lint test structure to use RuffTestFixture
type: pull_request
state: merged
author: Renkai
labels:
  - testing
assignees: []
merged: true
base: main
head: renkai19016
created_at: 2025-10-03T00:58:06Z
updated_at: 2025-10-07T09:33:54Z
url: https://github.com/astral-sh/ruff/pull/20689
synced_at: 2026-01-10T17:34:34Z
```

# Refactor Rust lint test structure to use RuffTestFixture

---

_Pull request opened by @Renkai on 2025-10-03 00:58_

Part of: https://github.com/astral-sh/ruff/issues/19016, for lint.rs

---

_Comment by @github-actions[bot] on 2025-10-03 01:06_

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

_Comment by @MichaReiser on 2025-10-03 06:04_

Thanks for working on this. Yes, this is what I had in mind.

---

_Marked ready for review by @Renkai on 2025-10-04 03:14_

---

_Comment by @Renkai on 2025-10-04 03:21_

Hi @MichaReiser , not sure why I can't modify the reviewers label, PTAL

---

_Comment by @Renkai on 2025-10-04 03:24_

Can't help on modify the windows insta snapshots very well, because I don't own a windows computer

---

_Comment by @MichaReiser on 2025-10-04 11:36_

> Can't help on modify the windows insta snapshots very well, because I don't own a windows computer

You can edit snapshots by hand. It's a bit pain but it should be okay if it's only that `/` is replaced with `TMP/`. 

For the other changes. It looks like there's a missing filter somewhere where the base path isn't filtered out anymore. 



---

_Comment by @Renkai on 2025-10-05 01:16_

> > Can't help on modify the windows insta snapshots very well, because I don't own a windows computer
> 
> You can edit snapshots by hand. It's a bit pain but it should be okay if it's only that `/` is replaced with `TMP/`.
> 
> For the other changes. It looks like there's a missing filter somewhere where the base path isn't filtered out anymore.

I fixed the filter problem, but for some tests, windows and linux have different behavior, the linux tests expect `ruff.toml` but windows expect `[TMP]/ruff.toml`. I doubt the `fs::relativize_path` method does not work well in some cases for windows.

https://github.com/Renkai/ruff/blob/2d3458f242188f9a3cdda46ada0aefacafd7f406/crates/ruff_workspace/src/configuration.rs#L1605

---

_Comment by @Renkai on 2025-10-05 01:50_

In the new pattern, there are four functions have different behavior between windows and linux
- line_too_long_width_override
- mixed_levels
- precedence
- top_level_options
I reverted them to the main branch version. It's most likely caused by `fs::relativize_path` can't work well in some cases, I recommend to create a new issue for them.

---

_Review comment by @MichaReiser on `crates/ruff/tests/test_fixture.rs`:6 on 2025-10-06 07:45_

Let's remove the `allow` dead code and remove any unusede code instead

---

_Review comment by @MichaReiser on `crates/ruff/tests/test_fixture.rs`:30 on 2025-10-06 07:46_

What's the reason why you aren't using dunce here (you can add it as a dev-only dependency)

---

_Review comment by @MichaReiser on `crates/ruff/tests/cli/main.rs`:38 on 2025-10-06 07:47_

Rather than handling UNC paths here, I suggest to do the same as ty and strip any UNC prefix in `new`

---

_Review comment by @MichaReiser on `crates/ruff/tests/test_fixture.rs`:79 on 2025-10-06 07:48_

Let's strip the UNC prefix here using dunce (you can add it as a dev dependency)

---

_Review comment by @MichaReiser on `crates/ruff/tests/test_fixture.rs`:140 on 2025-10-06 07:48_

Can we use `dedent` here instead of trimming start new lines only?

---

_@MichaReiser reviewed on 2025-10-06 07:51_

Nice. This is great. I only have a few questions around using dunce/dedent

---

_@Renkai reviewed on 2025-10-06 23:53_

---

_Review comment by @Renkai on `crates/ruff/tests/test_fixture.rs`:6 on 2025-10-06 23:53_

The current functions are actually all used, not sure why the clippy believe they are dead, add some unit tests to make sure they are used now.

---

_@Renkai reviewed on 2025-10-06 23:53_

---

_Review comment by @Renkai on `crates/ruff/tests/test_fixture.rs`:30 on 2025-10-06 23:53_

Nice crate, I didn't know it exists before.

---

_Comment by @github-actions[bot] on 2025-10-07 00:08_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-07 00:09_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review requested from @MichaReiser by @Renkai on 2025-10-07 00:32_

---

_Label `testing` added by @MichaReiser on 2025-10-07 06:50_

---

_Merged by @MichaReiser on 2025-10-07 09:28_

---

_Closed by @MichaReiser on 2025-10-07 09:28_

---
