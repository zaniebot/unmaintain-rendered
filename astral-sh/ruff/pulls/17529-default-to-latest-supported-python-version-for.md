```yaml
number: 17529
title: Default to latest supported Python version for version-related syntax errors
type: pull_request
state: merged
author: ntBre
labels:
  - preview
assignees: []
merged: true
base: main
head: brent/default-python-version
created_at: 2025-04-21T21:05:08Z
updated_at: 2025-05-06T14:19:15Z
url: https://github.com/astral-sh/ruff/pull/17529
synced_at: 2026-01-10T18:57:02Z
```

# Default to latest supported Python version for version-related syntax errors

---

_Pull request opened by @ntBre on 2025-04-21 21:05_

## Summary

This PR partially addresses #16418 via the following:

- `LinterSettings::unresolved_python_version` is now a `TargetVersion`, which is a thin wrapper around an `Option<PythonVersion>`
- `Checker::target_version` now calls `TargetVersion::linter_version` internally, which in turn uses `unwrap_or_default` to preserve the current default behavior
- Calls to the parser now call `TargetVersion::parser_version`, which calls `unwrap_or_else(PythonVersion::latest)`
- The `Checker`'s implementation of `SemanticSyntaxContext::python_version` also uses `TargetVersion::parser_version` to use `PythonVersion::latest` for semantic errors

In short, all lint rule behavior should be unchanged, but we default to the latest Python version for the new syntax errors, which should minimize confusing version-related syntax errors for users without a version configured.

## Test Plan

Existing tests, which showed no changes (except for printing default settings).


---

_Label `breaking` added by @ntBre on 2025-04-21 21:05_

---

_Comment by @codspeed-hq[bot] on 2025-04-21 21:11_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/brent%2Fdefault-python-version)

### Merging #17529 will **not alter performance**

<sub>Comparing <code>brent/default-python-version</code> (3ef26d0) with <code>main</code> (a4c8e43)</sub>



### Summary

`✅ 33` untouched benchmarks  





---

_Comment by @github-actions[bot] on 2025-04-21 21:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -6 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/facebookresearch/chameleon">facebookresearch/chameleon</a> (+0 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/facebookresearch/chameleon/blob/dbbe47fdbc7a3cc4982ae931fea75e227262e70a/chameleon/viewer/backend/models/chameleon_distributed.py#L405'>chameleon/viewer/backend/models/chameleon_distributed.py:405:13:</a> SyntaxError: Cannot use `match` statement on Python 3.9 (syntax was added in Python 3.10)
- <a href='https://github.com/facebookresearch/chameleon/blob/dbbe47fdbc7a3cc4982ae931fea75e227262e70a/chameleon/viewer/backend/models/chameleon_distributed.py#L818'>chameleon/viewer/backend/models/chameleon_distributed.py:818:17:</a> SyntaxError: Cannot use `match` statement on Python 3.9 (syntax was added in Python 3.10)
- <a href='https://github.com/facebookresearch/chameleon/blob/dbbe47fdbc7a3cc4982ae931fea75e227262e70a/chameleon/viewer/backend/models/chameleon_local.py#L633'>chameleon/viewer/backend/models/chameleon_local.py:633:13:</a> SyntaxError: Cannot use `match` statement on Python 3.9 (syntax was added in Python 3.10)
- <a href='https://github.com/facebookresearch/chameleon/blob/dbbe47fdbc7a3cc4982ae931fea75e227262e70a/chameleon/viewer/backend/models/service.py#L131'>chameleon/viewer/backend/models/service.py:131:21:</a> SyntaxError: Cannot use `match` statement on Python 3.9 (syntax was added in Python 3.10)
- <a href='https://github.com/facebookresearch/chameleon/blob/dbbe47fdbc7a3cc4982ae931fea75e227262e70a/chameleon/viewer/backend/models/service.py#L154'>chameleon/viewer/backend/models/service.py:154:17:</a> SyntaxError: Cannot use `match` statement on Python 3.9 (syntax was added in Python 3.10)
- <a href='https://github.com/facebookresearch/chameleon/blob/dbbe47fdbc7a3cc4982ae931fea75e227262e70a/chameleon/viewer/backend/models/service.py#L226'>chameleon/viewer/backend/models/service.py:226:25:</a> SyntaxError: Cannot use `match` statement on Python 3.9 (syntax was added in Python 3.10)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SyntaxError: | 6 | 0 | 6 | 0 | 0 |

</p>
</details>




---

_Comment by @ntBre on 2025-04-21 22:08_

The ecosystem checks broadly look correct; these projects don't have `ruff.toml` files or `requires-python` specified in their `pyproject.toml` files (at least in the repo root, I didn't look for other files). However, for PLC2801, I should probably narrow the scope of the early version return a bit. It's only relevant for `__aiter__` and `__anext__` dunder methods, not all of them, so those are now false negatives. UP035 could probably be narrowed a bit too.

---

_Comment by @MichaReiser on 2025-04-22 07:25_

I haven't started reviewing the PR yet but just a remark on the design. 

My only *concern* with this approach is that it won't work for Red Knot because it currently always requires to know exactly which Python version it is checking. Having said that, I'm okay with ignoring this limitation for now because we may be able to lift this Red Knot restriction in the future by supporting multi-version checking. 

---

_Marked ready for review by @ntBre on 2025-04-24 20:37_

---

_Review requested from @AlexWaygood by @ntBre on 2025-04-24 20:37_

---

_Comment by @ntBre on 2025-04-24 20:37_

As mentioned above, I updated the `PLC2801` and `UP035` checks to avoid as many false negatives as possible. It didn't change the ecosystem results for `UP035`, but it will affect imports from the `collections` or `pipes` modules, which are not version-dependent.

Thanks for the additional context on red-knot's approach. I think this is ready for review if the design sounds reasonable otherwise.

---

_Comment by @MichaReiser on 2025-04-25 06:47_

I think I'd find it useful to have a small write up that walks through the different places where we now change the default and explains why it's important that we pick a different default and why that specific default. 

I've a slight concern that we now have multiple defaults and it's unclear to even me when we pick which one. I worry that it will be very hard for users to understand what's going on and even harder for us to make this self explanatory in the tool (e.g. by attaching notes in diagnostics). Could we come up with a simpler approach that assumes fewer different versions or maybe simply disables certain checks if no version's specified? 

---

_Comment by @ntBre on 2025-05-01 17:45_

I think this should now be up to date with our internal discussions. In summary (I'll update the PR summary to something like this once this is up to date):

- `LinterSettings::unresolved_python_version` is now an Option<PythonVersion>
- `Checker::target_version` now calls `unwrap_or_default` internally, so the behavior in most cases is unchanged
- Calls to the parser now call `unwrap_or_else(PythonVersion::latest)`
- The `Checker`'s implementation of `SemanticSyntaxContext::python_version` also calls `unwrap_or_else(PythonVersion::latest)`

## TODOs

I left two literal TODOs in `configuration.rs` for the `target_version` fields on `FormatterSettings` and `AnalyzeSettings`. Do we want to change anything for those, or should I just delete the comments?

My other question is: should we go ahead and merge this? I put the `breaking` label on it when I thought it would be a larger change that needed to wait for a minor release. Now that it only affects syntax errors, I think it could be more reasonable to merge anyway. Alternatively, we could just merge the `Option<PythonVersion>` changes here and only save the smaller syntax error changes for a future release. That might be easier than keeping this whole branch updated.

---

_Review requested from @MichaReiser by @ntBre on 2025-05-01 17:46_

---

_Comment by @MichaReiser on 2025-05-01 17:49_

> I left two literal TODOs in configuration.rs for the target_version fields on FormatterSettings and AnalyzeSettings. Do we want to change anything for those, or should I just delete the comments?

I don't think we need to change anything here (assuming they still default to lowest supported)

> My other question is: should we go ahead and merge this? I put the breaking label on it when I thought it would be a larger change that needed to wait for a minor release. Now that it only affects syntax errors, I think it could be more reasonable to merge anyway. Alternatively, we could just merge the Option<PythonVersion> changes here and only save the smaller syntax error changes for a future release. That might be easier than keeping this whole branch updated.

I think landing this without breaking should be fine because it only impacts version-specific syntax and semantic errors, both are behind preview.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:526 on 2025-05-01 17:50_

It might be useful to add some more documentation here of what the version represents and to what it defaults

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:586 on 2025-05-01 17:50_

```suggestion
        self.target_version()
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/filesystem.rs`:22 on 2025-05-01 17:51_

It would be nice if we could handle the default elsewhere. E.g. on the call site. I would prefer to have the default handling in as few places as possible.

---

_Comment by @ntBre on 2025-05-01 17:52_

> I don't think we need to change anything here (assuming they still default to lowest supported)

Yep, they still `unwrap_or_default`. I'll delete the comments.

> I think landing this without breaking should be fine because it only impacts version-specific syntax and semantic errors, both are behind preview.

Ah of course, I keep forgetting they're still in preview. Thanks!


---

_Label `breaking` removed by @ntBre on 2025-05-01 17:53_

---

_Label `preview` added by @ntBre on 2025-05-01 17:53_

---

_@MichaReiser reviewed on 2025-05-01 17:53_

Overall looks good but I think we should try to find a better place for the default handling. I'm worried by the amount of places where we now have to call `unwrap_or_default`. 



---

_Renamed from "Update default Python version handling in the linter" to "Default to latest supported Python version for version-related syntax errors" by @ntBre on 2025-05-01 17:53_

---

_Comment by @MichaReiser on 2025-05-01 17:54_

You also need to update the PR summary.

---

_Comment by @ntBre on 2025-05-01 17:55_

> You also need to update the PR summary.

Will do! I was just waiting to see if there were any other big changes before I rewrote it.

---

_@ntBre reviewed on 2025-05-01 17:55_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:586 on 2025-05-01 17:55_

I thought we wanted `latest` for the semantic errors? This would restore it to `PythonVersion::default()`.

---

_@MichaReiser reviewed on 2025-05-01 18:09_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:586 on 2025-05-01 18:09_

Oh right. sorry

---

_@ntBre reviewed on 2025-05-01 18:20_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:586 on 2025-05-01 18:20_

No worries, I previously had a `Checker` method for this (`target_version_or_latest`), but this ended up being the only place it was called, so I just inlined it.

---

_@ntBre reviewed on 2025-05-01 18:21_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/filesystem.rs`:22 on 2025-05-01 18:21_

I was able to move the unwrap in this case up to `check_path`, but I went through the others and didn't see any more we could fix. Anything that goes through `check_ast` needs to have the `Option` because `check_ast` constructs a `Checker`, which also contains an `Option`. That brings us down to only two `unwrap_or_default` calls, both in `check_path`.

I think the `unwrap_or_latest` calls are as restricted as possible too, with only four of them in non-test code, and two of those are outside of the linter in `ruff_wasm` and `ruff_server`.

I agree in general, though, so I'm happy to update more of these if I'm missing a nicer solution.

---

_@ntBre reviewed on 2025-05-01 18:24_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:526 on 2025-05-01 18:24_

Sounds good. I had something longer here that I deleted because the `target_version` field is more private than this method, so I was thinking that, at the `pub(crate)` level, the fact that this is an `Option` is hidden. But it makes sense to document it somewhere, and this seems like a good place.

---

_@ntBre reviewed on 2025-05-01 19:20_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/filesystem.rs`:22 on 2025-05-01 19:20_

Actually, maybe we could make `ParseOptions::with_target_version` take an option and handle the unwrap once internally, if that would be preferable. I haven't tried it yet, but I don't think that would cause problems anywhere.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/filesystem.rs`:22 on 2025-05-02 06:31_

I'm not too concerned about passing `Option`. That's just what we need to do. I'm more concerned about the number of places where we need to change to get *consistent* behavior. It seems very easy to miss updating one of those places. 

> Actually, maybe we could make ParseOptions::with_target_version take an option and handle the unwrap once internally, if that would be preferable

I think I'd prefer changing the `ParseOptions::default` to initialize to the latest version and to either:

* Only call `with_target_version` if `target_version` is `Some`
* Change `with_target_version` to take an `Option` and only set it if it isn't `None`

Unless changing the default results in a too big fall out with tests?




---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/configuration.rs`:188 on 2025-05-02 06:32_

I thikn I'd prefer to instead leave line 167 unchanged to only have a single `unwrap_or_default` in `into_settings`.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:586 on 2025-05-02 06:35_

It would be great to have a comment here explaining why we use `latest` instead of default. It's very subtle

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/linter.rs`:163 on 2025-05-02 06:42_

Would it help if `check_path` (et al) would take a `TargetVersion` struct instead that has two methods (or even in `LinterSettings` or for all tools?):

* `linter_version`
* `parser_version`
* (`formatter_version`)


It's a thin wrapper around an `Option<PythonVersion>`

It would give us a place to document the decision why we use one version for a specific tool and it doesn't repeat the decision in many different places. 

That makes me wonder if we should also pass this struct to the formatter and analyze so that they can initialize the parser with the right version.

---

_@MichaReiser reviewed on 2025-05-02 06:46_

---

_@MichaReiser approved on 2025-05-02 20:44_

---

_@ntBre reviewed on 2025-05-02 21:29_

---

_Review comment by @ntBre on `crates/ruff_linter/src/linter.rs`:163 on 2025-05-02 21:29_

Oh yeah, I think that's a great idea. That was roughly my intention earlier with two `Checker` methods, but the parser didn't get a whole `Checker`, so I couldn't get it to work out nicely. Wrapping `Option<PythonVersion>` seems like it would help a lot.

> That makes me wonder if we should also pass this struct to the formatter and analyze so that they can initialize the parser with the right version.

Ooh good catch. Although it may not be a big deal in these cases if they aren't emitting the errors. It looks like both of these don't actually pass any Python version to the parser currently, so it's just using the default.

---

_Comment by @ntBre on 2025-05-05 22:24_

Could I get one more quick review here? I think the `TargetVersion` changes helped a lot, but I want to make sure it's what you had in mind.

I think the three (minor) things I'm not sure about are
- The `Display` impl for `TargetVersion` - is there a way to do this with `display_settings!` directly? (inline the inner option while also using `| optional`)
- The `Checker::target_version` method - I feel like this should return a `TargetVersion`, but then every lint rule would have to call `.linter_version()` itself. So I think this is still a nice convenience method
- Should `TargetVersion` be in `settings::types` instead of `settings`?

We could also reuse `TargetVersion` in the formatter and analyze settings, but I think that would make sense in a follow-up PR or two.

As an even smaller note, I know we usually prefer calling `TargetVersion::from`, but tacking the `into` calls on here was so much easier. Hopefully that's okay!

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:534 on 2025-05-06 11:50_

I agree that this method is still useful. It's somewhat confusing that it's called `target-version` and returns a `PythonVersion` but renaming it to `python_version` might be equally confusing because of the method on `SemanticSyntaxContext`.

---

_@MichaReiser approved on 2025-05-06 11:52_

Looks good. Thanks for following up on this. 

I haven't used `display_settings` myself but I think it also supports calling the debug implementation instead of `Display`. But I think what you have is fine.



---

_Comment by @ntBre on 2025-05-06 14:19_

I double-checked the ecosystem results, and chameleon has a [pyproject.toml](https://github.com/facebookresearch/chameleon/blob/dbbe47fdbc7a3cc4982ae931fea75e227262e70a/pyproject.toml) but no project-level `requires-python` setting (only a `build-system.requires-python` and a `tool.mypy.python_version`, which I don't think we read), so I believe we are accurately falling back to a default value, which is now high enough to avoid the `match` syntax error.

---

_Merged by @ntBre on 2025-05-06 14:19_

---

_Closed by @ntBre on 2025-05-06 14:19_

---

_Branch deleted on 2025-05-06 14:19_

---
