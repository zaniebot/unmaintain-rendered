```yaml
number: 15483
title: "[`ruff`] `itertools.starmap(..., zip(...))` (`RUF058`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: RUF058
created_at: 2025-01-15T00:22:01Z
updated_at: 2025-01-16T14:58:10Z
url: https://github.com/astral-sh/ruff/pull/15483
synced_at: 2026-01-10T20:34:00Z
```

# [`ruff`] `itertools.starmap(..., zip(...))` (`RUF058`)

---

_Pull request opened by @InSyncWithFoo on 2025-01-15 00:22_

## Summary

Resolves #15481, resolves #14835.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-01-15 00:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 2 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/a15a4b5e6f0397906f619ce8888670eadcf3af55/pandas/tests/arithmetic/test_datetime64.py#L2214'>pandas/tests/arithmetic/test_datetime64.py:2214:32:</a> RUF058 [*] `itertools.starmap` called on `zip` iterable
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/fe715540195e94d6836bc639c9a312aceaf99849/astropy/table/soco.py#L81'>astropy/table/soco.py:81:34:</a> RUF058 [*] `itertools.starmap` called on `zip` iterable
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF058 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read examples/Assistants_API_overview_python.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: expected `,` or `]` at line 197 column 8
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read examples/Assistants_API_overview_python.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: expected `,` or `]` at line 197 column 8
```

</p>
</details>




---

_@MichaReiser requested changes on 2025-01-15 09:06_

Thank you. I don't think we can implement this right now without changing `FURB140`. You can see in this [playground](https://play.ruff.rs/a9af7dfb-6dc5-4eee-b8c9-f41437eb24bf) that `FURB140` recommends using `starmap` for `map(..., zip)`. I think this is what I meant that we have to change `FURB140` first but I incorrectly wrote `map` instead of saying that it shouldn't apply to `zip`. 

---

_Comment by @MichaReiser on 2025-01-15 09:07_

I'll close this PR for now so that we can have the discussion in a single place. We can re-open it once we decide that the rule should be added and `FURB140` is changed.

---

_Closed by @MichaReiser on 2025-01-15 09:07_

---

_Reopened by @MichaReiser on 2025-01-15 13:54_

---

_Label `rule` added by @MichaReiser on 2025-01-15 13:54_

---

_Label `preview` added by @MichaReiser on 2025-01-15 13:54_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/reimplemented_starmap.rs`:32 on 2025-01-15 13:55_

Thanks for updating the documentation. That means this PR also fixes https://github.com/astral-sh/ruff/issues/14835

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/starmap_zip.rs`:13 on 2025-01-15 14:01_

I find the explanation here a bit confusing because it uses the term `zip`. It suggests that the iterable should be zipped (with `zip`) before calling `starmap` but it actually tests for the exact opposite. 

I suggest to rephrase it by putting the focus on saying that `zip`ing an iterable only to unpack the elements using `starmap` is unnecessary and that `map` should be used instead.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/starmap_zip.rs`:55 on 2025-01-15 14:04_

Nit: Resolving the qualified name is more expensive than some of the other tests
```suggestion
    if !call.arguments.keywords.is_empty() {
        return;
    }

    let [map_func, Expr::Call(iterable_call)] = call.arguments.args.as_ref() else {
        return;
    };


    let Some(qualified_name) = semantic.resolve_qualified_name(&call.func) else {
        return;
    };

    if !matches!(qualified_name.segments(), ["itertools", "starmap"]) {
        return;
    }
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/starmap_zip.rs`:70 on 2025-01-15 14:05_

Would you mind creating an issue so that we don't forget adding support for it

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/starmap_zip.rs`:82 on 2025-01-15 14:10_

What's the reason for using the range from `starmap.start` to the arguments here instead of using `starmap.func.range`. Using `starmap.func.range()` allows to preserve most of the existing formating

```suggestion
    let starmap_func_range = TextRange::new(starmap.start(), starmap.arguments.start());
    let change_func_to_map = Edit::range_replacement("map".to_string(), starmap_func_range);
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/starmap_zip.rs`:88 on 2025-01-15 14:10_

```suggestion
        zip.arguments.start() + "(".text_len(),
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/starmap_zip.rs`:122 on 2025-01-15 14:19_

```suggestion
    let before_zip_end = zip_end - ")".text_len();
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/starmap_zip.rs`:119 on 2025-01-15 14:20_

Could we add some comments here. E.g what's this block doing? 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/starmap_zip.rs`:84 on 2025-01-15 14:20_

Would it be possible to use a combination of `remove_argument` and `add_argument` instead of rolling our own fix here? 

---

_@MichaReiser reviewed on 2025-01-15 14:20_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/ruff/rules/starmap_zip.rs`:84 on 2025-01-15 15:16_

I don't think so. `add_argument()` [hardcodes `", {argument}"`](https://github.com/astral-sh/ruff/blob/55a7f72035390cf1093e4da0cf2462e793bfbce1/crates/ruff_linter/src/fix/edits.rs#L278) as the new content, while this rule aims to preserve styling as much as possible.

---

_@InSyncWithFoo reviewed on 2025-01-15 15:16_

---

_@InSyncWithFoo reviewed on 2025-01-15 15:44_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/ruff/rules/starmap_zip.rs`:70 on 2025-01-15 15:44_

Opened #15506.

---

_@MichaReiser approved on 2025-01-16 11:01_

I pushed a commit that simplifies the fix a bit. I'm mainly trying to avoid using the tokens where they aren't strictly necessary because they add a fair bit of complexity and it's very easy to get them wrong. 

---

_Comment by @InSyncWithFoo on 2025-01-16 13:25_

The changes are nice, especially the extra `Arguments` methods. Why isn't CI passing, though? I can't reproduce this locally.

---

_Comment by @MichaReiser on 2025-01-16 13:33_

I think it requires a rebase (github rebases your changes on main before running the checks)

---

_@InSyncWithFoo reviewed on 2025-01-16 14:06_

---

_Review comment by @InSyncWithFoo on `crates/ruff_text_size/src/range.rs`:47 on 2025-01-16 14:06_

@MichaReiser By the way, what is this for? Seems to be a debug-only change.


---

_@MichaReiser reviewed on 2025-01-16 14:17_

---

_Review comment by @MichaReiser on `crates/ruff_text_size/src/range.rs`:47 on 2025-01-16 14:17_

It changes where Rust reports any panics in this method. Without `track_caller`, the top stack frame is `TextRange::new` which isn't very helpful when debugging why `start > end`. Adding `track_caller` makes Rust report the call site instead of the assertion location 

---

_Merged by @MichaReiser on 2025-01-16 14:18_

---

_Closed by @MichaReiser on 2025-01-16 14:18_

---

_Branch deleted on 2025-01-16 14:58_

---
