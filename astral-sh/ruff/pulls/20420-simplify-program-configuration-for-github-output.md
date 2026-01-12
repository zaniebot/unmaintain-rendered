```yaml
number: 20420
title: "Simplify `Program` configuration for GitHub output"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
assignees: []
merged: true
base: brent/ty-github
head: brent/refactor-diagnostic-config
created_at: 2025-09-15T16:56:35Z
updated_at: 2025-09-17T13:32:32Z
url: https://github.com/astral-sh/ruff/pull/20420
synced_at: 2026-01-12T15:57:01Z
```

# Simplify `Program` configuration for GitHub output

---

_@ntBre_

## Summary

This is a follow-up to https://github.com/astral-sh/ruff/pull/20358 to avoid adding a new `DisplayDiagnosticConfig` option (with a surprising default value) just to print the program name for GitHub diagnostics.

Initially I expected this to be a larger refactor including making the various `Renderer::render` methods take a `std::io::Write` instead of a `std::fmt::Formatter` to make them easier to call directly in Ruff. However, this was a bit more painful than expected because we use the `Display` implementation of `DisplayDiagnostics` for more than writing directly to stdout. Implementing `Display` on the individual renderers seemed like a smaller step in this direction.

At this point it might make sense just to merge this into https://github.com/astral-sh/ruff/pull/20358. Alternatively, I could apply the same transformation to the other affected renderers. For example, the JSON and JSON-lines formats only use their `config` fields for the `preview` setting. Or we could update all of the renderers and call them directly in Ruff instead of assembling a config at all. I just wanted to make sure this approach looked reasonable before applying it to everything.

## Test Plan

Existing tests


---

_Label `internal` added by @ntBre on 2025-09-15 16:56_

---

_Comment by @codspeed-hq[bot] on 2025-09-15 17:09_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/brent%2Frefactor-diagnostic-config?runnerMode=Instrumentation)

### Merging #20420 will **degrade performances by 12.28%**

<sub>Comparing <code>brent/refactor-diagnostic-config</code> (c287764) with <code>brent/ty-github</code> (1db7531)</sub>



### Summary

`❌ 1` regression  
`✅ 42` untouched  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/brent%2Frefactor-diagnostic-config?runnerMode=Instrumentation)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` DateType `` | 224 ms | 255.4 ms | -12.28% |


---

_Comment by @github-actions[bot] on 2025-09-15 17:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -6 violations, +0 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/966e231f9421a103ef44f1639fec8459c2d93d7a/superset/commands/database/tables.py#L143'>superset/commands/database/tables.py:143:34:</a> SIM910 [*] Use `extra_dict_by_name.get(table.table)` instead of `extra_dict_by_name.get(table.table, None)`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/binary-husky/gpt_academic">binary-husky/gpt_academic</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/binary-husky/gpt_academic/blob/3546f8c1c44994bcc2ef3b2b187619821f2c33e9/request_llms/bridge_all.py#L1304'>request_llms/bridge_all.py:1304:31:</a> SIM910 [*] Use `dict.get()` without default value
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/9fd30a7b78a246c55751a38b9063e2d7f559dacd/zerver/models/users.py#L736'>zerver/models/users.py:736:28:</a> SIM910 [*] Use `user_data.get(field.id)` instead of `user_data.get(field.id, None)`
- <a href='https://github.com/zulip/zulip/blob/9fd30a7b78a246c55751a38b9063e2d7f559dacd/zerver/tornado/event_queue.py#L1231'>zerver/tornado/event_queue.py:1231:49:</a> SIM910 [*] Use `extra_user_data.get(client.user_profile_id)` instead of `extra_user_data.get(client.user_profile_id, None)`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/astropy/astropy/blob/e586ed83df04c193f1d8533448fa09835d3a589b/astropy/table/mixins/registry.py#L66'>astropy/table/mixins/registry.py:66:16:</a> SIM910 [*] Use `dict.get()` without default value
- <a href='https://github.com/astropy/astropy/blob/e586ed83df04c193f1d8533448fa09835d3a589b/astropy/wcs/wcsapi/fitswcs.py#L287'>astropy/wcs/wcsapi/fitswcs.py:287:34:</a> SIM910 [*] Use `CTYPE_TO_UCD1.get(ctype_name.upper())` instead of `CTYPE_TO_UCD1.get(ctype_name.upper(), None)`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM910 | 6 | 0 | 6 | 0 | 0 |

</p>
</details>




---

_Comment by @ntBre on 2025-09-15 17:20_

I'm pretty sure the ecosystem and benchmark results are just a quirk of the base branch, but I'll keep an eye on that for sure.

---

_Marked ready for review by @ntBre on 2025-09-15 17:20_

---

_Review requested from @carljm by @ntBre on 2025-09-15 17:20_

---

_Review requested from @MichaReiser by @ntBre on 2025-09-15 17:20_

---

_Review requested from @sharkdp by @ntBre on 2025-09-15 17:20_

---

_Review requested from @dcreager by @ntBre on 2025-09-15 17:20_

---

_Review request for @dcreager removed by @ntBre on 2025-09-15 17:20_

---

_Review request for @carljm removed by @ntBre on 2025-09-15 17:20_

---

_Review request for @sharkdp removed by @ntBre on 2025-09-15 17:20_

---

_@MichaReiser reviewed on 2025-09-16 07:14_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render/github.rs`:23 on 2025-09-16 07:14_

It wasn't clear to me from the summary what the motivation was for implementing `Display` for `Renderer` but, after playing with the code, the issue seems to be that ty uses an `std::fmt::Write` whereas Ruff uses an `std::io::Write`. 

I do find it odd that a `Renderer` can be displayed, especially because its display implementation shows the diagnostic and not the renderer. 

1. Introduce a `Renderer` trait that has a single `render` method that takes a `std::fmt::Formatter` and a `&[Diagnostic]` and create a new `DisplayDiagnostics<R: Renderer>` that implements `Display` by forwarding to the wrapped renderer. The main challenge with this is that we already have a `DisplayDiagnostics` type in `ruff_db`. My opinion is still that we should delete the old `DisplayDiagnostics` type and move `DiagnosticFormat` out of `ruff_db`, as which formats are supported is a concern of ty/ruff. But this is a bit a larger refactor (but I think it would be worth it because it also helps clarify which program supports which `DiagnosticFormat`s). 
2. Rename `GithubRenderer` to `GithubDiagnostics`. That's obviously a much simpler change but it's a bit more awkward because it no longer fits into the `render` module.

For this PR, I think what I'd do is to do a mix of 1 and 2 by:

* Restore the `render` method on `GithubRenderer`
* Introduce a new `DisplayGithubDiagnostics` type which takes a renderer and diagnostics as input and implements `Display`. We can use this type in places where we need to write to a `std::io::Write`



---

_@ntBre reviewed on 2025-09-16 15:43_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render/github.rs`:23 on 2025-09-16 15:43_

Ah yeah, sorry that wasn't clear. Yes, Ruff's `Emitter`s take a `&mut dyn std::io::Write`, but `DisplayDiagnostics` and the `Renderer`s in ty operate via `std::fmt::Display` and `std::fmt::Formatter`s. As I mentioned on https://github.com/astral-sh/ruff/pull/20358, I thought the best fix was moving `DisplayDiagnostics` and the renderers to use `std::io::Write` too, but there were a few places where this wasn't an easy switch. These are two of the less important cases since they're in tests:

https://github.com/astral-sh/ruff/blob/aa63c24b8f4a82f5f54dc90c4e6b5adadbf3e1b2/crates/ruff_db/src/diagnostic/render.rs#L2749-L2760

This seemed like the more painful case that made me rethink the approach and try implementing `Display` for each renderer:

https://github.com/astral-sh/ruff/blob/34ba738c19c557653567defc5cdaaa75445ad0d4/crates/ty_project/src/metadata/options.rs#L1465-L1472

I just tried fixing that one and more keep popping up, so I think your suggestions sound very helpful. Thank you!

---

_@MichaReiser approved on 2025-09-16 17:04_

---

_Merged by @ntBre on 2025-09-17 13:32_

---

_Closed by @ntBre on 2025-09-17 13:32_

---

_Branch deleted on 2025-09-17 13:32_

---
