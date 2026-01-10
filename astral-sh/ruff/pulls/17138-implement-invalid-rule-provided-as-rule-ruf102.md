```yaml
number: 17138
title: "Implement `Invalid rule provided` as rule RUF102 with `--fix`"
type: pull_request
state: merged
author: maxmynter
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: rule-noqa
created_at: 2025-04-02T03:20:33Z
updated_at: 2025-04-04T21:16:26Z
url: https://github.com/astral-sh/ruff/pull/17138
synced_at: 2026-01-10T19:40:37Z
```

# Implement `Invalid rule provided` as rule RUF102 with `--fix`

---

_Pull request opened by @maxmynter on 2025-04-02 03:20_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

Closes #17084

## Summary
This PR adds a new rule (RUF102) to detect and fix invalid rule codes in `noqa` comments. 
Invalid rule codes in `noqa` directives serve no purpose and may indicate outdated code suppressions.

This extends the previous behaviour originating from `crates/ruff_linter/src/noqa.rs` which would only emit a warnigs.
With this rule a `--fix` is available.

The rule:
1. Analyzes all `noqa` directives to identify invalid rule codes
2. Provides autofix functionality to:
   - Remove the entire comment if all codes are invalid
   - Remove only the invalid codes when mixed with valid codes
3. Preserves original comment formatting and whitespace where possible

Example cases:
- `# noqa: XYZ111` → Remove entire comment (keep empty line)
- `# noqa: XYZ222, XYZ333` → Remove entire comment (keep empty line)
-  `# noqa: F401, INVALID123` → Keep only valid codes (`# noqa: F401`)

## Test Plan
- Added tests in `crates/ruff_linter/resources/test/fixtures/ruff/RUF102.py` covering different example cases.

<!-- How was it tested? -->

## Notes 
- This does not handle cases where parsing fails. E.g. `# noqa: NON_EXISTENT, ANOTHER_INVALID` causes a `LexicalError` and the diagnostic is not propagated and we cannot handle the diagnostic. I am also unsure what proper `fix` handling would be and making the user aware we don't understand the codes is probably the best bet.
- The rule is added to the Preview rule group as it's a new addition

## Questions
- Should we remove the warnings, now that we have a rule?
- Is the current fix behavior appropriate for all cases, particularly the handling of whitespace and line deletions?
- I'm new to the codebase; let me know if there are rule utilities which could have used but didn't. 


---

_Label `rule` added by @ntBre on 2025-04-02 03:23_

---

_Comment by @github-actions[bot] on 2025-04-02 03:27_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/ba0d3cd77331d27f39d7e69748c8a0865daf9b87/providers/edge/src/airflow/providers/edge/worker_api/auth.py#L34'>providers/edge/src/airflow/providers/edge/worker_api/auth.py:34:78:</a> RUF102 [*] Invalid rule code in `# noqa`: TCH001
+ <a href='https://github.com/apache/airflow/blob/ba0d3cd77331d27f39d7e69748c8a0865daf9b87/providers/edge/src/airflow/providers/edge/worker_api/datamodels.py#L28'>providers/edge/src/airflow/providers/edge/worker_api/datamodels.py:28:72:</a> RUF102 [*] Invalid rule code in `# noqa`: TCH001
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF102 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>




---

_@ntBre reviewed on 2025-04-02 03:32_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/mod.rs`:318 on 2025-04-02 03:32_

Can this be part of the general `rules` test up above?

---

_Comment by @maxmynter on 2025-04-02 03:46_

Sorry for the CI issues, I thought that clippy runs with checking the pre-commit hooks. 

~~I thought I generated the docs with `cargo dev generate-all` and they are in the comment in the rule file. Am I supposed to do that differently?~~
Edit: I missed a `#` and had the wrong header level in the docstring. It should work now. 

---

_Label `preview` added by @MichaReiser on 2025-04-02 06:42_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/invalid_rule_code.rs`:59 on 2025-04-02 06:44_

Having to always collect all codes only to be able to generate the fix isn't ideal for performance (this requires a fair amount of writes). 

I'd rewrite this to only track if all codes are invalid and then re-iterate over the noqa codes when generating the fix and skip over the known invalid codes.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/invalid_rule_code.rs`:102 on 2025-04-02 06:46_

Does this work with `# test # noqa: invalid`? Could we use `line.directive.range` (assuming `directive` is the `Codes` variant) here?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/invalid_rule_code.rs`:63 on 2025-04-02 06:46_

Can you test that this rule respects https://docs.astral.sh/ruff/settings/#lint_external

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/invalid_rule_code.rs`:146 on 2025-04-02 06:48_

Nit: I think there's a `split_at(prefix_end)` method that gives you both the before and after parts

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/invalid_rule_code.rs`:151 on 2025-04-02 06:49_

This will panic if there is any non-ASCII whitespace because you pass the character count instead of the length in bytes. You'll have to use find or similar to find the end.

---

_@MichaReiser requested changes on 2025-04-02 06:49_

---

_@maxmynter reviewed on 2025-04-02 16:04_

---

_Review comment by @maxmynter on `crates/ruff_linter/src/rules/ruff/rules/invalid_rule_code.rs`:102 on 2025-04-02 16:04_

Thanks for the directive suggestion. It's much cleaner (adressed in [f7014db](https://github.com/astral-sh/ruff/pull/17138/commits/f7014dbeb31f9d0805723358d57f442bfa7b020c)).

Regarding multiple comments on a line it removes the invalid parts
- `# test # noqa: INVALID111` → `# test`
- `# test # noqa: INVALID111, VALID111` → `# test # noqa: VALID111`

I've added tests for this in [5cc8305](https://github.com/astral-sh/ruff/pull/17138/commits/5cc83056e74b67c10fa889ae722947cfc5b99dcc). 

I guess this is the desired behaviour. Having stale comments isn't nice, but auto removing ones that should stay is worse. What do you think. 

Note: I'm writing it with uppercase ASCII plus a number because otherwise it's not parsed as a rule code; in this case the comment remains untouched.

---

_@maxmynter reviewed on 2025-04-02 16:44_

---

_Review comment by @maxmynter on `crates/ruff_linter/src/rules/ruff/rules/invalid_rule_code.rs`:63 on 2025-04-02 16:44_

It didn't. I added a fix ([3f50765](https://github.com/astral-sh/ruff/pull/17138/commits/3f5076561ccae439d5fda1e2317e18a1582de184)) and test ([e734d3d](https://github.com/astral-sh/ruff/pull/17138/commits/e734d3db6e81383484f43c7de8a2b74041892a32)).

---

_Review comment by @maxmynter on `crates/ruff_linter/src/rules/ruff/rules/invalid_rule_code.rs`:146 on 2025-04-02 17:12_

Adressed in [2be49a0](https://github.com/astral-sh/ruff/pull/17138/commits/2be49a000fb06a45e1ea3cbca1bbb1d44c32eac2)

---

_@maxmynter reviewed on 2025-04-02 17:12_

---

_@maxmynter reviewed on 2025-04-02 17:12_

---

_Review comment by @maxmynter on `crates/ruff_linter/src/rules/ruff/rules/invalid_rule_code.rs`:151 on 2025-04-02 17:12_

Adressed in [e3f4e07](https://github.com/astral-sh/ruff/pull/17138/commits/e3f4e07105f3c733b1fc1617cc7284ab18e5915a)

---

_@maxmynter reviewed on 2025-04-02 18:25_

---

_Review comment by @maxmynter on `crates/ruff_linter/src/rules/ruff/rules/invalid_rule_code.rs`:59 on 2025-04-02 18:25_

Adressed in [52abe14](https://github.com/astral-sh/ruff/pull/17138/commits/52abe145626186fea7ed68f6012ed791f085f269)

---

_@maxmynter reviewed on 2025-04-02 18:52_

---

_Review comment by @maxmynter on `crates/ruff_linter/src/rules/ruff/mod.rs`:318 on 2025-04-02 18:52_

I moved the test you were refering to to the general rules test. The additional logic handling externals (see below) need (i think) separate handling so i kept these parts standalone. Let me know if that is not the case.

Changes in [d34476f](https://github.com/astral-sh/ruff/pull/17138/commits/d34476f430d7a2417aa2929749fa19174f86fa8a)

---

_Review requested from @MichaReiser by @maxmynter on 2025-04-02 21:03_

---

_@ntBre reviewed on 2025-04-02 21:51_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/mod.rs`:318 on 2025-04-02 21:51_

I think that does need to be separate, thanks!

---

_@MichaReiser reviewed on 2025-04-03 10:16_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/mod.rs`:319 on 2025-04-03 10:16_

NIt:
``` suggestion
    fn invalid_rule_code_external_rules() -> Result<()> {
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/invalid_rule_code.rs`:30 on 2025-04-03 10:17_

```suggestion
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/invalid_rule_code.rs`:30 on 2025-04-03 10:18_

It would be great to link to the `external` setting similar to `unused-noqa`: https://docs.astral.sh/ruff/rules/unused-noqa/

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/invalid_rule_code.rs`:65 on 2025-04-03 10:19_

```suggestion
            if Rule::from_code(code.as_str()).is_ok() || external.iter().any(|ext| code_str.starts_with(ext))
```

This might be faster because it checks the common case first.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/invalid_rule_code.rs`:65 on 2025-04-03 10:19_

```suggestion
            if external.iter().any(|ext| code_str.starts_with(ext))
                || Rule::from_code(code_str)).is_ok()
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/invalid_rule_code.rs`:85 on 2025-04-03 10:20_

Nit: Maybe consider moving this to a small helper function and use it here and when checking above.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/invalid_rule_code.rs`:102 on 2025-04-03 10:21_

Could we reuse the collected `invalid_code_refs` and pass them to this function instead of collecting them again?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/invalid_rule_code.rs`:98 on 2025-04-03 10:22_

Nit: To reduce the number of arguments:
```suggestion
fn create_all_codes_invalid_diagnostic(directive: &Codes<'_>) -> Diagnostic {
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/invalid_rule_code.rs`:136 on 2025-04-03 10:27_

And another case for multispan diagnostics... 

I think I'd be surprised that applying the fix for a specific code replaces all invalid codes and not just the one where I positioned my cursor on. 

That's why I think we should either:

* Only create one diagnostic that fixes all invalid codes at once (multispan diagnostics would be great)
* Create a diagnostic for each invalid code, the fix only removes that one code.

---

_@MichaReiser reviewed on 2025-04-03 10:28_

Thanks for incorporating external. This mostly looks good. The main thing we've to figure out is how to handle noqa comments where more than one code is invalid. Maybe @dylwil3 has an opinion on this?

---

_Review requested from @carljm by @maxmynter on 2025-04-03 22:01_

---

_Review requested from @AlexWaygood by @maxmynter on 2025-04-03 22:01_

---

_Review requested from @sharkdp by @maxmynter on 2025-04-03 22:01_

---

_Review requested from @dcreager by @maxmynter on 2025-04-03 22:01_

---

_Review requested from @BurntSushi by @maxmynter on 2025-04-03 22:01_

---

_Review requested from @dhruvmanila by @maxmynter on 2025-04-03 22:01_

---

_Comment by @github-actions[bot] on 2025-04-03 22:03_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @maxmynter on 2025-04-03 22:08_

Sorry everyone who just got pinged for a code review. 

I accidentally pushed from the tip of main and it autorequested from everyone who contributed since forked.
Git is hard...

My bad :/ 

---

_@maxmynter reviewed on 2025-04-03 22:10_

---

_Review comment by @maxmynter on `crates/ruff_linter/src/rules/ruff/mod.rs`:319 on 2025-04-03 22:10_

Adressed in [3050304](https://github.com/astral-sh/ruff/pull/17138/commits/3050304436fe33ca7e2fff3edf4b557edc6bc777)

---

_@maxmynter reviewed on 2025-04-03 22:11_

---

_Review comment by @maxmynter on `crates/ruff_linter/src/rules/ruff/rules/invalid_rule_code.rs`:30 on 2025-04-03 22:11_

Adressed in [2fe49fb](https://github.com/astral-sh/ruff/pull/17138/commits/2fe49fb1b94dd11666f5bb841dfc6fb2fb9c3044)

---

_Review comment by @maxmynter on `crates/ruff_linter/src/rules/ruff/rules/invalid_rule_code.rs`:30 on 2025-04-03 22:11_

Adressed in [2fe49fb](https://github.com/astral-sh/ruff/pull/17138/commits/2fe49fb1b94dd11666f5bb841dfc6fb2fb9c3044)

---

_@maxmynter reviewed on 2025-04-03 22:11_

---

_Review request for @dcreager removed by @carljm on 2025-04-03 22:43_

---

_Review request for @carljm removed by @carljm on 2025-04-03 22:43_

---

_Review request for @BurntSushi removed by @carljm on 2025-04-03 22:43_

---

_Review request for @sharkdp removed by @carljm on 2025-04-03 22:43_

---

_Review request for @dhruvmanila removed by @carljm on 2025-04-03 22:43_

---

_Review request for @AlexWaygood removed by @carljm on 2025-04-03 22:44_

---

_@maxmynter reviewed on 2025-04-04 03:20_

---

_Review comment by @maxmynter on `crates/ruff_linter/src/rules/ruff/rules/invalid_rule_code.rs`:102 on 2025-04-04 03:20_

We need to loop over the `directive` if we want to preserve the total order rule with only filtering codes for single code fixes.

---

_@maxmynter reviewed on 2025-04-04 03:46_

---

_Review comment by @maxmynter on `crates/ruff_linter/src/rules/ruff/rules/invalid_rule_code.rs`:65 on 2025-04-04 03:46_

Adressed in [e9de061](https://github.com/astral-sh/ruff/pull/17138/commits/e9de061507c31e6087ac3cdfa5bcb551b5ebbc04)

---

_@maxmynter reviewed on 2025-04-04 03:47_

---

_Review comment by @maxmynter on `crates/ruff_linter/src/rules/ruff/rules/invalid_rule_code.rs`:85 on 2025-04-04 03:47_

Adressed in [921f066](https://github.com/astral-sh/ruff/pull/17138/commits/921f0663f93257af10cdc8a5aec7fb7f34bad877)

---

_@maxmynter reviewed on 2025-04-04 03:47_

---

_Review comment by @maxmynter on `crates/ruff_linter/src/rules/ruff/rules/invalid_rule_code.rs`:98 on 2025-04-04 03:47_

Adressed in [921f066](https://github.com/astral-sh/ruff/pull/17138/commits/921f0663f93257af10cdc8a5aec7fb7f34bad877)

---

_@maxmynter reviewed on 2025-04-04 03:49_

---

_Review comment by @maxmynter on `crates/ruff_linter/src/rules/ruff/rules/invalid_rule_code.rs`:136 on 2025-04-04 03:49_

I'm unsure what you mean by multispan diagnostics or what the exact requirements are. Is that already established in Ruff or something you plan on integrating.

In [921f066](https://github.com/astral-sh/ruff/pull/17138/commits/921f0663f93257af10cdc8a5aec7fb7f34bad877) i fixed the handling to create one diagnostic for each invalid code.

---

_Review requested from @MichaReiser by @maxmynter on 2025-04-04 03:59_

---

_@MichaReiser reviewed on 2025-04-04 06:57_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/invalid_rule_code.rs`:136 on 2025-04-04 06:57_

> I'm unsure what you mean by multispan diagnostics or what the exact requirements are. Is that already established in Ruff or something you plan on integrating.

Sorry, it's something we plan on implementing. This was mainly directed at @ntBre 

---

_@MichaReiser approved on 2025-04-04 08:03_

---

_Merged by @MichaReiser on 2025-04-04 08:05_

---

_Closed by @MichaReiser on 2025-04-04 08:05_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/invalid_rule_code.rs`:136 on 2025-04-04 12:22_

Yeah sorry I should have acknowledged and resolved this. I'm keeping a list of places to use multispan diagnostics once they're added to Ruff!

---

_@ntBre reviewed on 2025-04-04 12:22_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/rules/invalid_rule_code.rs`:136 on 2025-04-04 12:42_

We could create a tracking issue for it which can then be referenced by the reviewers and the references itself becomes a list of known places which can be improved.

---

_@dhruvmanila reviewed on 2025-04-04 12:42_

---

_@ntBre reviewed on 2025-04-04 12:54_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/invalid_rule_code.rs`:136 on 2025-04-04 12:54_

Oh yes, of course!

---

_Comment by @maxmynter on 2025-04-04 21:16_

Thanks for merging @MichaReiser !

If you have the bandwith, I would love a short explanation on your rationale in [e1998a1](https://github.com/astral-sh/ruff/pull/17138/commits/e1998a1e3426ef2140f6c18bdc87b9a8ddc5c35c) (no worries if not). 

I agree that it reads much nicer, so I'm curious if you have any tips / resources on how you think about intended changes so i can deliver more quality out of the gate :)  

---
