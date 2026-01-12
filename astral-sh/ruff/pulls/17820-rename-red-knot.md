```yaml
number: 17820
title: Rename Red Knot
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/rename
created_at: 2025-05-03T16:44:36Z
updated_at: 2025-05-03T17:50:46Z
url: https://github.com/astral-sh/ruff/pull/17820
synced_at: 2026-01-12T15:56:06Z
```

# Rename Red Knot

---

_@MichaReiser_

## Summary

* Rename all crates from `red_knot_*` to `ty_`
* Rename `knot: ignore` to `ty: ignore`
* Rename `knot_extensions` to `ty_extensions`
* Rename `knot.toml` to `ty.toml` and `tool.knot` to `tool.ty`
* Rename `knot_benchmark to `ty_benchmark`
* Rename the benchmarks. This has the downside that we lose historic data. However, we'll have many more profiles in the future and the most important is the baseline compared to the previous commit
* Update mypy primer to build `ty`, https://github.com/hauntsaninja/mypy_primer/pull/166


I'm pretty sure I missed something or messed up some casing somewhere but we can fix those  cases in follow up PRs. I did check that there are no more files names `knot` and verified that all text matches for `knot` are unrelated to the type checker (or things I plan on changing later)


What I didn't rename as part of this PR:

* ~~`knot` argument in mypy primer: @sharkdp will take care of this~~ I need to do this to not break the mypy primer workflow https://github.com/hauntsaninja/mypy_primer/pull/166/commits
* Playground URL: I'll combine this with some other playground changes (new logo etc).


## CI Failures

* The fuzzer job fails because it tries to pull the `red_knot` binary but its now named `ty`. This should work again once we run from main
* The mypy primer job fails because it tries to build `ty` on main, but the binary is called `red_knot` on main. This should work once both main and PRs are past this commit.

---

_Label `red-knot` added by @MichaReiser on 2025-05-03 16:45_

---

_Comment by @codspeed-hq[bot] on 2025-05-03 16:52_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Frename)

### Merging #17820 will **not alter performance**

<sub>Comparing <code>micha/rename</code> (f9e6b7f) with <code>main</code> (e6a798b)</sub>



### Summary

`✅ 30` untouched benchmarks  
`🆕 3` new benchmarks  
`⁉️ 3 (👁 3)` dropped benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `` red_knot_check_file[cold] `` | 101.8 ms | N/A | N/A |
| 👁 | `` red_knot_check_file[incremental] `` | 5.6 ms | N/A | N/A |
| 👁 | `` red_knot_micro[many_string_assignments] `` | 58.7 ms | N/A | N/A |
| 🆕 | `` ty_check_file[cold] `` | N/A | 102 ms | N/A |
| 🆕 | `` ty_check_file[incremental] `` | N/A | 5.6 ms | N/A |
| 🆕 | `` ty_micro[many_string_assignments] `` | N/A | 58.9 ms | N/A |


---

_Comment by @github-actions[bot] on 2025-05-03 17:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/mesonbuild/meson-python">mesonbuild/meson-python</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read tests/packages/symlinks/baz.py: No such file or directory (os error 2)
error: Failed to read tests/packages/symlinks/qux.py: No such file or directory (os error 2)
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/mesonbuild/meson-python">mesonbuild/meson-python</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read tests/packages/symlinks/baz.py: No such file or directory (os error 2)
error: Failed to read tests/packages/symlinks/qux.py: No such file or directory (os error 2)
```

</p>
</details>




---

_Marked ready for review by @MichaReiser on 2025-05-03 17:22_

---

_Review requested from @carljm by @MichaReiser on 2025-05-03 17:22_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-05-03 17:22_

---

_Review requested from @sharkdp by @MichaReiser on 2025-05-03 17:22_

---

_Review requested from @dcreager by @MichaReiser on 2025-05-03 17:22_

---

_Comment by @AlexWaygood on 2025-05-03 17:38_

We need to rename the label we put on PRs -- there's this reference to the old name still on your branch because the label still has the old name:

https://github.com/astral-sh/ruff/blob/1bd849239e477594cc143644fcba9351d6172f58/pyproject.toml#L109-L113

but maybe that needs to be done as a followup? not sure

---

_Comment by @MichaReiser on 2025-05-03 17:39_

Yeah, I thought about that (and should have mentioned it in the PR summary). 

I decided to wait for when we rename the label. We can do this separately.

---

_Comment by @AlexWaygood on 2025-05-03 17:40_

we need to update this reference in the docs for running mypy_primer: https://github.com/astral-sh/ruff/blob/1bd849239e477594cc143644fcba9351d6172f58/crates/ty/docs/mypy_primer.md?plain=1#L21-L31

---

_Comment by @MichaReiser on 2025-05-03 17:40_

> we need to update this reference in the docs for running mypy_primer: 

Yes, I need to do this now that I changed the mypy primer job. 

---

_@AlexWaygood approved on 2025-05-03 17:46_

🚀 Obviously I haven't checked every line, but overall this LGTM!

---

_Merged by @MichaReiser on 2025-05-03 17:49_

---

_Closed by @MichaReiser on 2025-05-03 17:49_

---

_Branch deleted on 2025-05-03 17:49_

---
