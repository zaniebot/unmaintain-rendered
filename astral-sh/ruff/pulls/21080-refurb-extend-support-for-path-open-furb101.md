```yaml
number: 21080
title: "[`refurb`] Extend support for `Path.open` (`FURB101`, `FURB103`)"
type: pull_request
state: merged
author: 11happy
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: extend_furb103
created_at: 2025-10-26T10:43:15Z
updated_at: 2025-12-17T14:18:14Z
url: https://github.com/astral-sh/ruff/pull/21080
synced_at: 2026-01-10T16:42:11Z
```

# [`refurb`] Extend support for `Path.open` (`FURB101`, `FURB103`)

---

_Pull request opened by @11happy on 2025-10-26 10:43_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
This PR fixes https://github.com/astral-sh/ruff/issues/18409

## Test Plan

<!-- How was it tested? -->
I have added tests in FURB103.


---

_Comment by @github-actions[bot] on 2025-10-26 10:53_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+15 -0 violations, +0 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/PlasmaPy/PlasmaPy">PlasmaPy/PlasmaPy</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8f90993a45b0a722f4ed22dda676718be0230913/.github/scripts/authors_in_cff.py#L40'>.github/scripts/authors_in_cff.py:40:10:</a> FURB101 `Path.open()` followed by `read()` can be replaced by `pathlib.Path("CITATION.cff").read_text()`
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8f90993a45b0a722f4ed22dda676718be0230913/.github/scripts/citation_updater.py#L69'>.github/scripts/citation_updater.py:69:10:</a> FURB103 [*] `Path.open()` followed by `write()` can be replaced by `citation_rst_file.write_text(citation_rst_text)`
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8f90993a45b0a722f4ed22dda676718be0230913/docs/_author_list_from_cff.py#L193'>docs/_author_list_from_cff.py:193:10:</a> FURB103 [*] `Path.open()` followed by `write()` can be replaced by `pathlib.Path(rst_file).write_text(authors_rst, encoding="utf-8")`
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8f90993a45b0a722f4ed22dda676718be0230913/docs/_global_substitutions.py#L217'>docs/_global_substitutions.py:217:10:</a> FURB103 [*] `Path.open()` followed by `write()` can be replaced by `pathlib.Path(rst_file).write_text(content, encoding="utf-8")`
+ <a href='https://github.com/PlasmaPy/PlasmaPy/blob/8f90993a45b0a722f4ed22dda676718be0230913/tests/utils/data/test_downloader.py#L191'>tests/utils/data/test_downloader.py:191:10:</a> FURB103 [*] `Path.open()` followed by `write()` can be replaced by `filepath.write_text("Not data")`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/71778cb72116ab896e89e288fcdede868ea4f395/libs/partners/chroma/langchain_chroma/vectorstores.py#L489'>libs/partners/chroma/langchain_chroma/vectorstores.py:489:14:</a> FURB101 `Path.open()` followed by `read()` can be replaced by `Path(uri).read_bytes()`
+ <a href='https://github.com/langchain-ai/langchain/blob/71778cb72116ab896e89e288fcdede868ea4f395/libs/standard-tests/langchain_tests/conftest.py#L70'>libs/standard-tests/langchain_tests/conftest.py:70:14:</a> FURB101 [*] `Path.open()` followed by `read()` can be replaced by `cassette_path.read_bytes()`
+ <a href='https://github.com/langchain-ai/langchain/blob/71778cb72116ab896e89e288fcdede868ea4f395/libs/standard-tests/langchain_tests/conftest.py#L88'>libs/standard-tests/langchain_tests/conftest.py:88:14:</a> FURB103 [*] `Path.open()` followed by `write()` can be replaced by `cassette_path.write_bytes(data)`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/20153a10c0e967198cd1a47b4b0796a6aae5d6d0/src/scikit_build_core/ast/ast.py#L88'>src/scikit_build_core/ast/ast.py:88:10:</a> FURB101 `Path.open()` followed by `read()` can be replaced by `Path(sys.argv[1]).read_text(encoding="utf-8-sig")`
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/20153a10c0e967198cd1a47b4b0796a6aae5d6d0/src/scikit_build_core/ast/tokenizer.py#L77'>src/scikit_build_core/ast/tokenizer.py:77:10:</a> FURB101 `Path.open()` followed by `read()` can be replaced by `Path(sys.argv[1]).read_text(encoding="utf-8-sig")`
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/20153a10c0e967198cd1a47b4b0796a6aae5d6d0/src/scikit_build_core/metadata/regex.py#L58'>src/scikit_build_core/metadata/regex.py:58:10:</a> FURB101 `Path.open()` followed by `read()` can be replaced by `Path(input_filename).read_text(encoding="utf-8")`
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/20153a10c0e967198cd1a47b4b0796a6aae5d6d0/tests/test_dynamic_metadata.py#L274'>tests/test_dynamic_metadata.py:274:10:</a> FURB103 [*] `Path.open()` followed by `write()` can be replaced by `Path("__init__.py").write_text("__version__ = '0.1.0'")`
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/20153a10c0e967198cd1a47b4b0796a6aae5d6d0/tests/test_dynamic_metadata.py#L290'>tests/test_dynamic_metadata.py:290:10:</a> FURB103 [*] `Path.open()` followed by `write()` can be replaced by `Path("version.hpp")....`
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/20153a10c0e967198cd1a47b4b0796a6aae5d6d0/tests/test_dynamic_metadata.py#L326'>tests/test_dynamic_metadata.py:326:10:</a> FURB103 [*] `Path.open()` followed by `write()` can be replaced by `Path("version.hpp")....`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/indico/indico">indico/indico</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/indico/indico/blob/158b2a603253b4099d4943b3efa31b33e4fbe908/indico/vendor/django_mail/message.py#L369'>indico/vendor/django_mail/message.py:369:14:</a> FURB101 `Path.open()` followed by `read()` can be replaced by `path.read_bytes()`
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB103 | 8 | 8 | 0 | 0 | 0 |
| FURB101 | 7 | 7 | 0 | 0 | 0 |

</p>
</details>





---

_Comment by @11happy on 2025-10-29 19:31_

@MichaReiser would appreciate your feedback on this whenever you get some time.

Thank you : )

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/snapshots/ruff_linter__rules__refurb__tests__preview_FURB103_FURB103.py.snap`:1 on 2025-10-30 19:39_

Oh, I didn't notice this when reviewing #20520, but `FURB101` and `FURB103` are already preview rules, so we don't actually need the additional preview checks on their fixes (or these preview tests/snapshot files). Feel free to follow up on that, I think it's fine to leave it for a separate PR, though!

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/snapshots/ruff_linter__rules__refurb__tests__preview_FURB103_FURB103.py.snap`:285 on 2025-10-30 19:41_

I don't think we need to add the pathlib import for cases where the input was already a `Path`. We're just adding another method call onto the existing `Path`.

Ah, this might actually be highlighting a different issue. If `pathlib.Path` hasn't been imported, we shouldn't be applying the rule since this could be something else that just happens to be named `Path`.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/helpers.rs`:270 on 2025-10-30 19:45_

Ah yeah, I think we need to be resolving the qualified name here. We may also be able to use our existing pathlib typing check:

https://github.com/astral-sh/ruff/blob/16b9b92182b7457478281e0cdfe4d14c74f33d13/crates/ruff_python_semantic/src/analyze/typing.rs#L1120-L1124

https://github.com/astral-sh/ruff/blob/16b9b92182b7457478281e0cdfe4d14c74f33d13/crates/ruff_python_semantic/src/analyze/typing.rs#L925-L928

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/helpers.rs`:293 on 2025-10-30 19:46_

It feels like we should probably be able to reuse some more code from `find_file_open` instead of duplicating the whole thing, but maybe they're different enough that that's not true.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/write_whole_file.rs`:73 on 2025-10-30 19:48_

I don't feel too strongly about this, but I kind of think the old message applies well enough to both cases and we could just leave it alone and not need to track `is_path_open` in the `Violation` struct.

---

_@ntBre requested changes on 2025-10-30 19:50_

Thanks for working on this!

I think we need to be a bit more careful with how we identify existing `Path` instances and hopefully we can deduplicate some code too.

---

_Label `rule` added by @ntBre on 2025-10-30 19:50_

---

_Label `preview` added by @ntBre on 2025-10-30 19:50_

---

_Comment by @amyreese on 2025-10-30 20:08_

It might be helpful to reuse `is_open_call_from_pathlib` from ASYNC230: https://github.com/astral-sh/ruff/blob/1c7ea690a820deaa0e17ecf72593ebc4781f3752/crates/ruff_linter/src/rules/flake8_async/rules/blocking_open_call.rs#L73

---

_Comment by @amyreese on 2025-10-30 20:11_

Alternately PathlibPathChecker which can also match annotations: https://github.com/astral-sh/ruff/blob/1c7ea690a820deaa0e17ecf72593ebc4781f3752/crates/ruff_linter/src/rules/flake8_async/rules/blocking_path_methods.rs#L161

---

_@11happy reviewed on 2025-11-02 13:15_

---

_Review comment by @11happy on `crates/ruff_linter/src/rules/refurb/snapshots/ruff_linter__rules__refurb__tests__preview_FURB103_FURB103.py.snap`:285 on 2025-11-02 13:15_

with the new name resolution utilizing the `is_open_call_from_pathlib` its not firing if pathlib.path has not been imported.

---

_@11happy reviewed on 2025-11-02 13:16_

---

_Review comment by @11happy on `crates/ruff_linter/src/rules/refurb/helpers.rs`:270 on 2025-11-02 13:16_

sure, I have used `is_open_call_from_pathlib` as suggested by @amyreese howevr had to make the function public.

---

_@11happy reviewed on 2025-11-02 13:17_

---

_Review comment by @11happy on `crates/ruff_linter/src/rules/refurb/rules/write_whole_file.rs`:73 on 2025-11-02 13:17_

I personally feel its a little bit more clear, however would be happy to change if you insist.
Thank  you : )

---

_Comment by @11happy on 2025-11-02 13:18_

the common helper has too many arguments causing the clippy error, should I break it down ?

---

_@11happy reviewed on 2025-11-02 13:19_

---

_Review comment by @11happy on `crates/ruff_linter/src/rules/refurb/helpers.rs`:293 on 2025-11-02 13:19_

yes have extracted the common logic from both functions. currently named it `resolve_file_open` would be happy to change if you have any suggestions.
Thank you : )

---

_Comment by @ntBre on 2025-11-03 15:05_

> the common helper has too many arguments causing the clippy error, should I break it down ?

I think it's okay just to add `#[expect(clippy::too_many_arguments)]` on the function.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/write_whole_file.rs`:73 on 2025-11-03 15:05_

Sounds good, we can leave it then!

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/refurb/FURB103.py`:1 on 2025-11-03 15:11_

I think we might want a separate file for these new tests and this import. We're losing a lot of snapshot changes where the fix needed to add this import.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/helpers.rs`:5 on 2025-11-03 15:14_

Can you move these back down to a separate `crate` import block like we had before? Stable rustfmt still doesn't handle this, unfortunately.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/helpers.rs`:271 on 2025-11-03 15:47_

Are we missing some of the argument validation code from `find_file_open`, especially these lines:

```rust
    if args.iter().any(Expr::is_starred_expr)
        || keywords.iter().any(|keyword| keyword.arg.is_none())
    {
        return None;
    }
```

I also think we're doing too much inspection on the `attr.value`. For example, I think `is_open_call_from_pathlib` should be able to identify a case like this:

```py
p = Path("file.txt")
with p.open("r") as f:
    f.write("test")
```

but the current implementation expects a literal `Path()` call, not a name that resolves to a `Path`.

We might need to make the `filename` field optional on the `FileOpen` struct.

---

_@ntBre reviewed on 2025-11-03 15:58_

---

_@11happy reviewed on 2025-11-06 12:37_

---

_Review comment by @11happy on `crates/ruff_linter/resources/test/fixtures/refurb/FURB103.py`:1 on 2025-11-06 12:37_

done I have added a seperate file suffixed _EXT

---

_Review comment by @11happy on `crates/ruff_linter/src/rules/refurb/helpers.rs`:5 on 2025-11-06 12:37_

done

---

_@11happy reviewed on 2025-11-06 12:37_

---

_@11happy reviewed on 2025-11-06 12:38_

---

_Review comment by @11happy on `crates/ruff_linter/src/rules/refurb/helpers.rs`:271 on 2025-11-06 12:38_

done , also the message is formatted accordingly but i feel its getting nested & dosent feel good , not sure although

---

_@11happy reviewed on 2025-11-06 12:41_

---

_Review comment by @11happy on `crates/ruff_linter/src/rules/refurb/helpers.rs`:271 on 2025-11-06 12:41_

also I have not added any comments yet , should i add similar comments like `find_file_open`

---

_Comment by @MichaReiser on 2025-11-10 12:32_

Would you mind rebasing your PR? Doing so will allow us to merge your PR without delayed once reviewed.

---

_Comment by @11happy on 2025-11-10 13:27_

done rebased it & resolved merge conflicts : )
Thank you

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/helpers.rs`:283 on 2025-11-10 15:33_

I don't think this is quite right. [`pathlib.Path`](https://docs.python.org/3/library/pathlib.html#pathlib.Path) takes `*args`, so it can be called like this:

```pycon
>>> from pathlib import Path
>>> Path("foo", "bar", "baz")
PosixPath('foo/bar/baz')
```

Taking only the first element would give the wrong result.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/snapshots/ruff_linter__rules__refurb__tests__FURB103_FURB103_1.py.snap`:131 on 2025-11-10 15:49_

Should this case have a fix?

---

_@ntBre reviewed on 2025-11-10 15:59_

Thanks for rebasing and for your work on this! I pushed a commit fixing a few nits but then noticed a few more minor issues. The implementation feels a bit complicated to me now, I want to keep thinking about it.

---

_@11happy reviewed on 2025-11-12 06:28_

---

_Review comment by @11happy on `crates/ruff_linter/src/rules/refurb/helpers.rs`:283 on 2025-11-12 06:28_

actually this is handeled correctly already as we are we are using `path_obj` in fix generation
also have changed to require filename only in none case
```
let target = if let Some(path_obj) = open.path_obj {
        locator.slice(path_obj.range()).to_string()
    } else {
        let filename = open.filename?;
        let filename_code = locator.slice(filename.range());
        format!("{binding}({filename_code})")
    };
```


---

_@11happy reviewed on 2025-11-12 06:29_

---

_Review comment by @11happy on `crates/ruff_linter/src/rules/refurb/helpers.rs`:283 on 2025-11-12 06:29_

i think filename is redundant in path open cases as we are send `path_obj`

---

_@11happy reviewed on 2025-11-12 06:31_

---

_Review comment by @11happy on `crates/ruff_linter/src/rules/refurb/snapshots/ruff_linter__rules__refurb__tests__FURB103_FURB103_1.py.snap`:131 on 2025-11-12 06:31_

I believe for this case we can have a fix, but would like to know your opinion on this .
Thank you

---

_Comment by @11happy on 2025-11-12 06:33_

> Thanks for rebasing and for your work on this! I pushed a commit fixing a few nits but then noticed a few more minor issues. The implementation feels a bit complicated to me now, I want to keep thinking about it.

I agree with you implementation is complex ,felt the same, would appreciate any pointers to decomplicate/refactor it ?
Thank you : )

---

_@11happy reviewed on 2025-11-12 06:33_

---

_Review comment by @11happy on `crates/ruff_linter/src/rules/refurb/helpers.rs`:283 on 2025-11-12 06:33_

have added the case

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/snapshots/ruff_linter__rules__refurb__tests__FURB103_FURB103_1.py.snap`:131 on 2025-11-19 20:29_

Sorry, yeah, I thought it should have a fix. This looks good now!

---

_@ntBre reviewed on 2025-11-19 20:29_

---

_@ntBre reviewed on 2025-11-19 20:31_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/helpers.rs`:283 on 2025-11-19 20:31_

Thanks, the new test looks good!

---

_Comment by @ntBre on 2025-11-19 21:44_

I just pushed a commit trying out an `OpenArgument` enum since the `path_obj` field and `filename` field were basically mutually exclusive. I think this makes things a little simpler, but I'm curious to get your thoughts on it too. I think the only remaining thing I don't love is the `is_path_open` check on the `Violation` struct. Maybe we could use the whole suggestion we generate for the fix in the error message?

I also fixed some new merge conflicts while I was here.

I also noticed that we haven't updated the code for FURB102, even though it's quite similar to FURB103. Do you want to do that in a follow-up PR?

---

_Comment by @11happy on 2025-11-20 17:22_

> I just pushed a commit trying out an `OpenArgument` enum since the `path_obj` field and `filename` field were basically mutually exclusive. I think this makes things a little simpler, but I'm curious to get your thoughts on it too. I think the only remaining thing I don't love is the `is_path_open` check on the `Violation` struct. Maybe we could use the whole suggestion we generate for the fix in the error message?
> 
> I also fixed some new merge conflicts while I was here.
> 
> I also noticed that we haven't updated the code for FURB102, even though it's quite similar to FURB103. Do you want to do that in a follow-up PR?

Thank you for these updates, will go through the commit in detail & will share my thoughts, I am also interested to work on the follow up PR for FURB102 , I feel a little occupied this week but will update this PR & my other open PRs soon.
Thanks again : )

---

_Comment by @11happy on 2025-11-26 12:41_

> I just pushed a commit trying out an `OpenArgument` enum since the `path_obj` field and `filename` field were basically mutually exclusive. I think this makes things a little simpler, but I'm curious to get your thoughts on it too. I think the only remaining thing I don't love is the `is_path_open` check on the `Violation` struct. Maybe we could use the whole suggestion we generate for the fix in the error message?

Yes, with the new OpenArgument enum code is a lot cleaner, what do you think of using `OpenArgument` directly instead of `is_path_open`  ?



---

_Review requested from @ntBre by @11happy on 2025-12-01 06:34_

---

_@ntBre reviewed on 2025-12-16 20:58_

Thank you! The code for FURB103  looks good to me now, your last commit was a nice improvement :)

Based on the ecosystem report, this is already affecting FURB101 too (I had the wrong rule code when I mentioned FURB102 earlier). Would you mind going ahead and updating the FURB101 code and adding a test for it? I don't think it makes as much sense to save that for a follow-up if we already changed the rule's behavior here.

I think it's mostly just the `message`, `fix_title`, and `filename_code` that need to be updated, most of the logic is already in the shared helpers.

---

_Comment by @11happy on 2025-12-17 11:14_

> Thank you! The code for FURB103 looks good to me now, your last commit was a nice improvement :)
> 
> Based on the ecosystem report, this is already affecting FURB101 too (I had the wrong rule code when I mentioned FURB102 earlier). Would you mind going ahead and updating the FURB101 code and adding a test for it? I don't think it makes as much sense to save that for a follow-up if we already changed the rule's behavior here.
> 
> I think it's mostly just the `message`, `fix_title`, and `filename_code` that need to be updated, most of the logic is already in the shared helpers.

done, to keep it consistent with FURB103 separated the tests for FURB101, also should I add more tests ?
Thank  you : )


---

_@ntBre approved on 2025-12-17 14:17_

Thank you! I think this is enough testing for FURB101 since most of the tricky code is shared with FURB103.

---

_Renamed from "Extend support for Path.Open in FURB103" to "[`refurb`] Extend support for `Path.open` (`FURB101`, `FURB103`)" by @ntBre on 2025-12-17 14:17_

---

_Merged by @ntBre on 2025-12-17 14:18_

---

_Closed by @ntBre on 2025-12-17 14:18_

---
