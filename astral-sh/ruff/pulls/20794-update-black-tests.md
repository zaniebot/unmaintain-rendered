```yaml
number: 20794
title: Update Black tests
type: pull_request
state: merged
author: ntBre
labels:
  - formatter
  - testing
assignees: []
merged: true
base: main
head: brent/import-black-tests
created_at: 2025-10-09T21:54:55Z
updated_at: 2025-10-14T14:15:02Z
url: https://github.com/astral-sh/ruff/pull/20794
synced_at: 2026-01-12T15:57:10Z
```

# Update Black tests

---

_@ntBre_

Summary
--

```shell
git clone git@github.com:psf/black.git ../other/black
crates/ruff_python_formatter/resources/test/fixtures/import_black_tests.py ../other/black
```

Then ran our tests and accepted the snapshots

I had to make a small fix to our tuple normalization logic for `del` statements
in the second commit, otherwise the tests were panicking at a changed AST. I
think the new implementation is closer to the intention described in the nearby
comment anyway, though.

The first commit adds the new Python, settings, and `.expect` files, the next three commits make some small
fixes to help get the tests running, and then the fifth commit accepts all but one of the new snapshots. The last commit includes the new unsupported syntax error for one f-string example, tracked in #20774.

Test Plan
--

Newly imported tests. I went through all of the new snapshots and added review comments below. I think they're all expected, except a few cases I wasn't 100% sure about.


---

_Label `internal` added by @ntBre on 2025-10-09 21:55_

---

_Label `formatter` added by @ntBre on 2025-10-09 21:55_

---

_Comment by @github-actions[bot] on 2025-10-09 22:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__remove_lone_list_item_parens.py.snap`:90 on 2025-10-13 16:58_

These look like known deviations since we don't implement the `remove_lone_list_item_parens` style  yet.

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__type_param_defaults.py.snap`:28 on 2025-10-13 17:03_

This is the only new code in this file, I think we've gotten closer to black here, if I'm interpreting the diff correctly.

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__cantfit.py.snap`:68 on 2025-10-13 17:07_

These all look like improvements to me, but I'm slightly confused about this case and the one above without comments. Maybe we have a slightly different line-length metric from black, I'm not sure why they wrap these.

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__fmtskip10.py.snap`:26 on 2025-10-13 17:12_

These are related to https://github.com/astral-sh/ruff/issues/11216 and https://github.com/astral-sh/ruff/pull/20633, Dylan's PR will probably clean them up!

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__fmtskip10.py.snap`:43 on 2025-10-13 17:13_

I'm less sure about this case and the `j =     1` case above. It looks like we're adding a space before the `# fmt: skip`. It seems like we should probably skip that too.

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__fstring.py.snap`:78 on 2025-10-13 17:14_

Bug tracked in https://github.com/astral-sh/ruff/issues/20774.

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__fstring_quotations.py.snap`:54 on 2025-10-13 17:16_

Seems intentional, we merge implicitly concatenated strings.

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__generics_wrapping.py.snap`:212 on 2025-10-13 17:17_

I think our formatting looks better for most of these, but this one looks a bit weird splitting the `Z:` and the `int` annotation. That's what the input looks like though, so we've preserved more of the original formatting.

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__long_strings__type_annotations.py.snap`:51 on 2025-10-13 17:20_

The implicit concatenation here seems nice, maybe a bit weird that we preserve the parens though.

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__pep604_union_types_line_breaks.py.snap`:1 on 2025-10-13 17:22_

Should we revert this? We had a commit to fix the typos, but they get restored by our script. Maybe we should fix the typos upstream?

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__pep_701.py.snap`:85 on 2025-10-13 17:24_

This test case was changed in #18708, should we revert this too?

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__prefer_rhs_split_reformatted.py.snap`:48 on 2025-10-13 17:33_

As noted in the comment above, this test case was added as part of a fix for https://github.com/psf/black/issues/1187. Before that, black had the same style as ruff. I can't quite tell if this is a known/expected deviation.

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__preview_comments7.py.snap`:164 on 2025-10-13 17:45_

I'm confused about this one. The .py file didn't change for this, and it looks like our diff output just changed a bit, or we're adding a blank line for some reason.

If I actually run the formatter on this file, we're not adding a blank line, so this seems like some kind of diff artifact.

And diffing 0.13.3 (my system ruff) against this PR's version shows nothing too:

```shell
> diff \
<(cargo run -p ruff -- format --no-cache --check --preview  crates/ruff_python_formatter/resources/test/fixtures/black/cases/preview_comments7.py)  \
<(ruff format --no-cache --check --preview  crates/ruff_python_formatter/resources/test/fixtures/black/cases/preview_comments7.py)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.15s
     Running `target/debug/ruff format --no-cache --check --preview crates/ruff_python_formatter/resources/test/fixtures/black/cases/preview_comments7.py`
```

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__preview_import_line_collapse.py.snap`:1 on 2025-10-13 17:50_

These are related to the `always_one_newline_after_import` preview style tracked in the style guide issue.

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__preview_long_dict_values.py.snap`:1 on 2025-10-13 17:56_

I _think_ these are expected. The quoting changes are definitely expected, the .py file was modified upstream in https://github.com/psf/black/pull/4377, and this file is a test for `wrap_long_dict_values_in_parens`, which we intentionally don't support yet.

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__preview_long_strings.py.snap`:1 on 2025-10-13 17:59_

These also seem expected to me, I think the changes are just a shift from the extra "very" added to one of the strings.

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__preview_long_strings__regression.py.snap`:1184 on 2025-10-13 18:06_

I think this is expected and in line with the `string_processing` preview style.

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__preview_multiline_strings.py.snap`:1 on 2025-10-13 18:07_

These should be expected in line with `multiline_string_handling` being a non-goal.

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__preview_remove_multiline_lone_list_item_parens.py.snap`:1 on 2025-10-13 18:08_

These seem expected, more `lone_list_item` changes.

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__preview_wrap_comprehension_in.py.snap`:1 on 2025-10-13 18:08_

Expected, `wrap_comprehension_in` feature that we plan to implement.

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__remove_except_types_parens.py.snap`:1 on 2025-10-13 18:09_

These should be fixed in https://github.com/astral-sh/ruff/pull/20768.

---

_@ntBre reviewed on 2025-10-13 18:10_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__cantfit.py.snap`:68 on 2025-10-13 18:11_

After looking around a bit more, I think this is related to https://github.com/astral-sh/ruff/issues/11820.

---

_@ntBre reviewed on 2025-10-13 18:11_

---

_Marked ready for review by @ntBre on 2025-10-13 19:24_

---

_Review requested from @MichaReiser by @ntBre on 2025-10-13 19:24_

---

_Label `internal` removed by @MichaReiser on 2025-10-14 06:07_

---

_Label `testing` added by @MichaReiser on 2025-10-14 06:07_

---

_@MichaReiser reviewed on 2025-10-14 06:10_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__fmtskip10.py.snap`:43 on 2025-10-14 06:10_

Yes, we format the `fmt: skip` comment. I don't think this is something that we need to change.

---

_@MichaReiser reviewed on 2025-10-14 06:12_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__generics_wrapping.py.snap`:212 on 2025-10-14 06:12_

I think it's okay. It's not like Ruff makes it worse. It's just that the user put the comment there.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__long_strings__type_annotations.py.snap`:51 on 2025-10-14 06:13_

Removing parens would set a new precedence but we could

---

_@MichaReiser reviewed on 2025-10-14 06:13_

---

_@MichaReiser reviewed on 2025-10-14 06:14_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__pep_701.py.snap`:85 on 2025-10-14 06:14_

We can change it or leave it as is if the test framework can handle it just fine.

---

_@MichaReiser reviewed on 2025-10-14 06:15_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__prefer_rhs_split_reformatted.py.snap`:48 on 2025-10-14 06:15_

I don't think this is intentional but I'm not sure it's worth fixing

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__preview_comments7.py.snap`:164 on 2025-10-14 06:18_

The `.expect` file changed. I expect this to be because black changed the number of empty lines after imports.

We'll have to be cautious about that change because I suspect it breaks isort compatibility.

---

_@MichaReiser reviewed on 2025-10-14 06:18_

---

_@MichaReiser reviewed on 2025-10-14 06:21_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__type_param_defaults.py.snap`:56 on 2025-10-14 06:21_

Huh, we ignore the trailing comma 

---

_@MichaReiser approved on 2025-10-14 06:24_

Thanks for the detailed analysis. 

---

_@ntBre reviewed on 2025-10-14 14:06_

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__type_param_defaults.py.snap`:56 on 2025-10-14 14:06_

Oh, this might actually be okay. This is the input:

```py
def trailing_comma1[T=int,](a: str):
    pass
```

So we respect the trailing comma in the type parameters, and there's no trailing comma in the function parameters.

---

_Merged by @ntBre on 2025-10-14 14:14_

---

_Closed by @ntBre on 2025-10-14 14:14_

---

_Branch deleted on 2025-10-14 14:15_

---
