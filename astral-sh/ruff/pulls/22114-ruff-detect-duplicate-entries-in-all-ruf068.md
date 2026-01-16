```yaml
number: 22114
title: "[`ruff`] Detect duplicate entries in `__all__` (`RUF068`)"
type: pull_request
state: merged
author: leandrobbraga
labels:
  - rule
  - preview
  - great writeup
assignees: []
merged: true
base: main
head: ruf069
created_at: 2025-12-20T15:36:26Z
updated_at: 2026-01-16T20:14:58Z
url: https://github.com/astral-sh/ruff/pull/22114
synced_at: 2026-01-16T21:04:11Z
```

# [`ruff`] Detect duplicate entries in `__all__` (`RUF068`)

---

_@leandrobbraga_

Hello,

This MR adds a new rule and its fix, `RUF069`, `DuplicateEntryInDunderAll`. I'm using `RUF069` because we already have [RUF068](https://github.com/astral-sh/ruff/pull/20585) and [RUF069](https://github.com/astral-sh/ruff/pull/21079#issuecomment-3493839453) in the works.

The rule job is to prevent users from accidentally adding duplicate entries to `__all__`, which, for example, can result from copy-paste mistakes.

It deals with the following syntaxes:

```python
__all__: list[str] = ["a", "a"]
__all__: typing.Any = ("a", "a")
__all__.extend(["a", "a"])
__all__ += ["a", "a"]
```

But it does not keep track of `__all__` contents, meaning the following code snippet is a false negative:
```python
class A: ...

__all__ = ["A"]
__all__.extend(["A"])
```

## Violation Example

```console
RUF069 `__all__` contains duplicate entries
 --> RUF069.py:2:17
  |
1 | __all__ = ["A", "A", "B"]
  |                 ^^^
help: Remove duplicate entries from `__all__`
1 | __all__ = ["A", "B"]
  - __all__ = ["A", "A", "B"]
```

## Ecosystem Report

The `ruff-ecosystem` results contain seven violations in four projects, all of them seem like true positives, with one instance appearing to be an actual bug.

This [code snippet](https://github.com/python/typeshed/blob/90d855985be5aae9bc76e77b0f3d4b6738c38347/stubs/reportlab/reportlab/lib/rltempfile.pyi#L4) from `reportlab` contains the same entry twice instead of exporting both functions.

```python
def get_rl_tempdir(*subdirs: str) -> str: ...
def get_rl_tempfile(fn: str | None = None) -> str: ...

__all__ = ("get_rl_tempdir", "get_rl_tempdir")
```

Closes [#21945](https://github.com/astral-sh/ruff/issues/21945)

---

_Comment by @astral-sh-bot[bot] on 2025-12-20 15:47_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+7 -0 violations, +0 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/query.py#L53'>src/bokeh/core/query.py:53:5:</a> RUF068 [*] `__all__` contains duplicate entries: `find` duplicated here
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/langchain-ai/langchain/blob/a84722e2d768a3029a632d0ab3515b897ebba63b/libs/langchain/langchain_classic/document_loaders/__init__.py#L373'>libs/langchain/langchain_classic/document_loaders/__init__.py:373:5:</a> RUF068 [*] `__all__` contains duplicate entries: `AcreomLoader` duplicated here
+ <a href='https://github.com/langchain-ai/langchain/blob/a84722e2d768a3029a632d0ab3515b897ebba63b/libs/langchain/langchain_classic/document_loaders/__init__.py#L391'>libs/langchain/langchain_classic/document_loaders/__init__.py:391:5:</a> RUF068 [*] `__all__` contains duplicate entries: `AsyncHtmlLoader` duplicated here
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/b94c9e8e48f552b2f2b979b69937b0e4848675cf/stubs/convertdate/convertdate/__init__.pyi#L45'>stubs/convertdate/convertdate/__init__.pyi:45:5:</a> RUF068 [*] `__all__` contains duplicate entries: `mayan` duplicated here
+ <a href='https://github.com/python/typeshed/blob/b94c9e8e48f552b2f2b979b69937b0e4848675cf/stubs/reportlab/reportlab/lib/rltempfile.pyi#L4'>stubs/reportlab/reportlab/lib/rltempfile.pyi:4:30:</a> RUF068 [*] `__all__` contains duplicate entries: `get_rl_tempdir` duplicated here
+ <a href='https://github.com/python/typeshed/blob/b94c9e8e48f552b2f2b979b69937b0e4848675cf/stubs/workalendar/workalendar/europe/__init__.pyi#L252'>stubs/workalendar/workalendar/europe/__init__.pyi:252:5:</a> RUF068 [*] `__all__` contains duplicate entries: `Switzerland` duplicated here
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/b5754a667932b7c39b5f425abb2de77a7962fa9c/astropy/table/__init__.py#L32'>astropy/table/__init__.py:32:5:</a> RUF068 [*] `__all__` contains duplicate entries: `conf` duplicated here
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF068 | 7 | 7 | 0 | 0 | 0 |

</p>
</details>





---

_Comment by @leandrobbraga on 2025-12-20 16:29_

It seems that those are all true positives.

This one might even be a bug.
https://github.com/python/typeshed/blob/06ecffccc72423711137c07be80c90522c084bbe/stubs/reportlab/reportlab/lib/rltempfile.pyi#L1-L4 

---

_Comment by @chirizxc on 2025-12-21 09:31_

What about these cases? 
```py
import typing


class A: ...


class B: ...

__all__ = "A" + "B"
__all__: list[str] = ["A", "B"]
__all__: typing.Any = ("A", "B")
```

---

_@chirizxc reviewed on 2025-12-21 09:35_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/ruff/rules/duplicate_all_entry.rs`:1 on 2025-12-21 09:35_

maybe `use rustc_hash::FxHashSet;`?

---

_Comment by @leandrobbraga on 2025-12-21 09:44_

> What about these cases? 
> ```py
> import typing
> 
> 
> class A: ...
> 
> 
> class B: ...
> 
> __all__ = "A" + "B"
> __all__: list[str] = ["A", "B"]
> __all__: typing.Any = ("A", "B")
> ```

Do you want to test whether those situations produce false positives or whether it still catches duplicates when using tuples, concatenation and/or type annotations?

---

_Comment by @chirizxc on 2025-12-21 09:46_

> > What about these cases?
> > ```python
> > import typing
> > 
> > 
> > class A: ...
> > 
> > 
> > class B: ...
> > 
> > __all__ = "A" + "B"
> > __all__: list[str] = ["A", "B"]
> > __all__: typing.Any = ("A", "B")
> > ```
> 
> Do you want to test whether those situations produce false positives or whether it still catches duplicates when using tuples, concatenation and/or type annotations?

I think it makes sense to add them to the test cases

---

_@leandrobbraga reviewed on 2025-12-21 13:55_

---

_Review comment by @leandrobbraga on `crates/ruff_linter/src/rules/ruff/rules/duplicate_all_entry.rs`:1 on 2025-12-21 13:55_

Done as suggested

---

_Comment by @leandrobbraga on 2025-12-21 13:56_

> > > What about these cases?
> > > ```python
> > > import typing
> > > 
> > > 
> > > class A: ...
> > > 
> > > 
> > > class B: ...
> > > 
> > > __all__ = "A" + "B"
> > > __all__: list[str] = ["A", "B"]
> > > __all__: typing.Any = ("A", "B")
> > > ```
> > 
> > 
> > Do you want to test whether those situations produce false positives or whether it still catches duplicates when using tuples, concatenation and/or type annotations?
> 
> I think it makes sense to add them to the test cases

Added these cases as suggested

---

_Comment by @leandrobbraga on 2025-12-21 14:08_

I added some extra cases, based on RUF022:
```python
__all__: list[str] = ["a", "a"]
__all__: typing.Any = ("a", "a")
__all__.extend(["a", "a"])
__all__ += ["a", "a"]
```

It still does not track mutable `__all__`, example:
```python
__all__ = ["a"]
__all__ += ["a"]
```

This will be a false negative.

---

_Review requested from @chirizxc by @leandrobbraga on 2025-12-21 17:14_

---

_Comment by @chirizxc on 2025-12-21 17:33_

LGTM.

There may be other cases, but again, we cannot check dynamically created `__all__`, and we can only look at one file at a time, so if someone decides to add a duplicate in another file, we will not be able to detect it. 

I also think it's worth adding `RUF069.pyi`.

---

_Comment by @leandrobbraga on 2025-12-21 19:47_

> LGTM.
> 
> There may be other cases, but again, we cannot check dynamically created `__all__`, and we can only look at one file at a time, so if someone decides to add a duplicate in another file, we will not be able to detect it.
> 
> I also think it's worth adding `RUF069.pyi`.

Done

---

_Review request for @chirizxc removed by @ntBre on 2025-12-23 14:30_

---

_Review requested from @ntBre by @ntBre on 2025-12-23 14:30_

---

_Label `rule` added by @ntBre on 2026-01-01 15:55_

---

_Label `preview` added by @ntBre on 2026-01-01 15:55_

---

_Label `great writeup` added by @ntBre on 2026-01-01 15:58_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/duplicate_all_entry.rs`:89 on 2026-01-01 16:15_

I think if you use `&**func` like in `args` and `keywords` above you can avoid the `ref`s here, but either way is fine.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/duplicate_all_entry.rs`:157 on 2026-01-01 16:28_

I think we should try to inspect the tokens instead of doing this text manipulation. I think we could use something like `SimpleTokenizer::starts_at(range.end(), checker.source()).skip_trivia().next()` to find the trailing comma. That's similar to what we do in this rule:

https://github.com/astral-sh/ruff/blob/b2b9d91859f2abc017e35d22f01696f141f6f5b1/crates/ruff_linter/src/rules/flake8_bugbear/rules/duplicate_value.rs#L82-L91

Actually that whole function would almost work here. It might be worth making that a helper function that just takes a `&[Expr]` since it really only uses `set.elts` (even `set.len()` defers to the length of `elts`).

The helper could go in this file maybe:

https://github.com/astral-sh/ruff/blob/b2b9d91859f2abc017e35d22f01696f141f6f5b1/crates/ruff_linter/src/fix/edits.rs#L272

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/duplicate_all_entry.rs`:167 on 2026-01-01 16:29_

Small nit, but the usual pattern we use here is something like:


```suggestion
            let applicability = if checker.comment_ranges().intersects(fix_range) {
                Applicability::Unsafe
            } else {
                Applicability::Safe
            };
            
            diagnostic.set_fix(Fix::applicable_edit(edit, applicability));
```

or you could inline the applicability too.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/duplicate_entry_in_dunder_all.rs`:154 on 2026-01-01 16:33_

Do we need to check this before iterating over the elements and starting to emit diagnostics? Based on the comment, it sounds like we should, but it also seems like we could actually just `continue` here. It should be okay to emit diagnostics in this case right?

```py
__all__ = [
	foo,
	"bar",
	"bar",  # RUF069
]
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__RUF069_RUF069.py.snap`:196 on 2026-01-01 16:35_

We shouldn't append this comma to the comment. Hopefully the token approach will help with this.

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/ruff/RUF069.pyi`:1 on 2026-01-01 16:36_

Just curious, what's the benefit of including the `.pyi` version? I don't think the rule has any stub-specific behavior right? Maybe it was just to check that it runs on stubs?

cc @chirizxc 

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/ruff/RUF068.py`:23 on 2026-01-01 16:38_

Let's throw in one multi-line case without comments.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/duplicate_entry_in_dunder_all.rs`:1 on 2026-01-01 16:39_

Small nit, and this might just be me, but I often expect the file name to match the struct name exactly, so could we rename the file to `duplicate_entry_in_dunder_all.rs`?

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/duplicate_entry_in_dunder_all.rs`:49 on 2026-01-01 16:48_

We should add a `## Fix safety` section explaining the unsafety around comments. We may also want to double-check the comment range check. I don't think the existing test case actually deletes comments. I think the only way we could delete a comment is in a highly contrived case like:

```py
__all__ = [
	"a",
	"a"
	# comment
	,
	"b"
]
```

since we need to delete both the second `"a"` and its trailing comma.

---

_@ntBre requested changes on 2026-01-01 16:51_

Thank you! This looks great overall, and the summary was really helpful.

Thanks @chirizxc for the rule suggestion and the reviews, as well.

My main concern was with the fix, but all of my other comments are very minor.

---

_@chirizxc reviewed on 2026-01-01 17:23_

---

_Review comment by @chirizxc on `crates/ruff_linter/resources/test/fixtures/ruff/RUF069.pyi`:1 on 2026-01-01 17:23_

Yes, there will be nothing like that in `.pyi` if the duplicate remains, but I think it's useful to check this, since duplicates were most likely added as a result of IDE autocompletion, or in large files where `__all__` can be 100-200 lines or more. In any case, it's something like a typo check.

---

_@ntBre reviewed on 2026-01-01 17:28_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/ruff/RUF069.pyi`:1 on 2026-01-01 17:28_

Oh sorry, I didn't phrase my question very well. I think most of our rules apply to both `py` and `pyi` files, so I was thinking it might be okay to remove the duplicate tests in this `pyi` file (remove this file) since there's no stub-specific functionality being tested.

I agree that the rule should apply to stubs, just wondering if we can reduce the number of identical tests.

---

_@chirizxc reviewed on 2026-01-01 17:31_

---

_Review comment by @chirizxc on `crates/ruff_linter/resources/test/fixtures/ruff/RUF069.pyi`:1 on 2026-01-01 17:31_

Yes, I think we can remove the tests for `.pyi`.

---

_Comment by @ntBre on 2026-01-01 17:32_

> I also think it's worth adding a check for `__all__.append()`. If we look at the [code search on GitHub](https://github.com/search?q=__all__.append&type=code), there are 21,000 matches. 🤔
> 
> @ntBre what do you think about this?

@chirizxc I think it's tricky to check `.append` because we'd have to keep internal state on `__all__` values we've seen so far. There's not really a good way to do that with the current structure of the rule, which only sees one statement/expression at a time. I'm okay with leaving it out of the rule, especially in a first version.

---

_Review comment by @leandrobbraga on `crates/ruff_linter/src/rules/ruff/rules/duplicate_all_entry.rs`:157 on 2026-01-11 14:34_

The `remove_member` implementation does not account for comments, removing them together with the element, which means that B033 also deletes them.

This changed the snapshot from:

```
RUF069 [*] `__all__` contains duplicate entries
  --> RUF069.py:33:5
   |
31 |     "B",
32 |     # Comment
33 |     "B",
   |     ^^^
34 | ]
   |
help: Remove duplicate entries from `__all__`
29 |     "A",
30 |     "A",
31 |     "B",
   -     # Comment
   -     "B",
32 +     # Comment,
33 | ]
note: This is an unsafe fix and may change runtime behavior
```

to

```
RUF069 [*] `__all__` contains duplicate entries
  --> RUF069.py:33:5
   |
31 |     "B",
32 |     # Comment
33 |     "B",
   |     ^^^
34 | ]
   |
help: Remove duplicate entries from `__all__`
29 |     "A",
30 |     "A",
31 |     "B",
   -     # Comment
   -     "B",
32 | ]
note: This is an unsafe fix and may change runtime behavior
```

Are you okay with that or should we strive to keep the comment, changing the behavior for B033 and RUF069?

---

_@leandrobbraga reviewed on 2026-01-11 14:34_

---

_@leandrobbraga reviewed on 2026-01-11 14:36_

---

_Review comment by @leandrobbraga on `crates/ruff_linter/src/rules/ruff/rules/duplicate_all_entry.rs`:157 on 2026-01-11 14:36_

**Out of scope**: I've noticed a few other instances of text manipulation for deletion, from where I took initial inspiration:
https://github.com/astral-sh/ruff/blob/b2b9d91859f2abc017e35d22f01696f141f6f5b1/crates/ty_python_semantic/src/suppression/unused.rs#L135-L165

---

_Comment by @leandrobbraga on 2026-01-11 15:04_

I’ve addressed all the remarks, improved the documentation and bumped the version.

---

_Review requested from @ntBre by @leandrobbraga on 2026-01-11 15:04_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/duplicate_entry_in_dunder_all.rs`:170 on 2026-01-12 21:22_

I think we should be able to use [`try_set_fix`](https://github.com/astral-sh/ruff/blob/e4ba29392bb768965ba16ba86aba7d482e4f2ea8/crates/ruff_linter/src/checkers/ast/mod.rs#L3538) here, which will automatically log this error if it fails.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/duplicate_entry_in_dunder_all.rs`:13 on 2026-01-12 21:24_

I think this was actually okay before the latest revision since the rule may be used without `--fix`.

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/ruff/RUF069.py`:32 on 2026-01-12 21:31_

Can we add an end-of-line comment too? And maybe one after the deleted element, just to check which comments get deleted.


```suggestion
    "B",  # 2
    # 3
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/duplicate_entry_in_dunder_all.rs`:10 on 2026-01-12 21:32_

tiny nit:


```suggestion
use rustc_hash::{FxBuildHasher, FxHashSet};

use ruff_diagnostics::{Applicability, Fix};
use ruff_macros::{ViolationMetadata, derive_message_formats};
use ruff_python_ast as ast;
use ruff_text_size::Ranged;

use crate::checkers::ast::Checker;
use crate::fix::edits;
use crate::{FixAvailability, Violation};
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/duplicate_entry_in_dunder_all.rs`:154 on 2026-01-12 21:36_

Let's add a test for a case like this.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/duplicate_entry_in_dunder_all.rs`:158 on 2026-01-12 21:38_

tiny nit:


```suggestion
            let mut diagnostic = checker.report_diagnostic(DuplicateEntryInDunderAll, expr);
```

`expr` implements `Ranged`, which should make it a valid argument on its own, as long as I'm not missing another reference to `range`.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/duplicate_all_entry.rs`:157 on 2026-01-12 21:40_

Yes, removing that comment is okay. That's what makes the fix unsafe, though.

---

_@ntBre reviewed on 2026-01-12 21:45_

Thank you! This is looking great. I just had a few more very small nits, and a couple of suggestions about additional tests (but I think they'll be handled well already).

I also think it might be worth manually testing the CLI to make sure cases with multiple deletions in the same assignment are fixed correctly. I think they should be, but we have a mechanism to [`isolate`](https://github.com/astral-sh/ruff/blob/e4ba29392bb768965ba16ba86aba7d482e4f2ea8/crates/ruff_diagnostics/src/fix.rs#L168) fixes if we need it. I think just testing something like:

```shell
$ cargo run -p ruff -- check --preview --select RUF069 --fix - <<<'__all__ = ["A", "A", "B", "B"]'
```

should work. If it _doesn't_ work, we should add an actual CLI test, but I think it's okay as long as the manual test works as expected.

---

_@MichaReiser reviewed on 2026-01-13 08:31_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__RUF069_RUF069.py.snap`:9 on 2026-01-13 08:31_

It would be nice if we highlighted all definitions here, so that users don't need to scan the entire list to find the first definition, similar to what we do in ty


```
error[duplicate-base]: Duplicate base class `int`
 --> simple.py:1:7
  |
1 | class Test(int, str, int): ...
  |       ^^^^^^^^^^^^^^^^^^^
  |
info: The definition of class `Test` will raise `TypeError` at runtime
 --> simple.py:1:12
  |
1 | class Test(int, str, int): ...
  |            ---       ^^^ Class `int` later repeated here
  |            |
  |            Class `int` first included in bases list here
  |
info: rule `duplicate-base` is enabled by default
```

(except that I wouldn't highlight the entire definition)

You can accomplish this by adding a secondary diagnostic annotation

---

_@ntBre reviewed on 2026-01-13 20:48_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__RUF069_RUF069.py.snap`:9 on 2026-01-13 20:48_

Link to the secondary annotation helper for reference:

https://github.com/astral-sh/ruff/blob/2245355931f66def488809a6b9b784f7c294bd49/crates/ruff_linter/src/checkers/ast/mod.rs#L3309-L3311

---

_Comment by @leandrobbraga on 2026-01-14 23:43_

> Thank you! This is looking great. I just had a few more very small nits, and a couple of suggestions about additional tests (but I think they'll be handled well already).
> 
> I also think it might be worth manually testing the CLI to make sure cases with multiple deletions in the same assignment are fixed correctly. I think they should be, but we have a mechanism to [`isolate`](https://github.com/astral-sh/ruff/blob/e4ba29392bb768965ba16ba86aba7d482e4f2ea8/crates/ruff_diagnostics/src/fix.rs#L168) fixes if we need it. I think just testing something like:
> 
> ```shell
> $ cargo run -p ruff -- check --preview --select RUF069 --fix - <<<'__all__ = ["A", "A", "B", "B"]'
> ```
> 
> should work. If it _doesn't_ work, we should add an actual CLI test, but I think it's okay as long as the manual test works as expected.

It does work:
```console
$ cargo run -p ruff -- check --preview --select RUF069 --fix - <<<'__all__ = ["A", "A", "B", "B"]'
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 8.98s
     Running `target/debug/ruff check --preview --select RUF069 --fix -`
warning: Detected debug build without --no-cache.
__all__ = ["A", "B"]
Found 2 errors (2 fixed, 0 remaining).
```
I also have tested it in the [crates/ruff_linter/resources/test/fixtures/ruff/RUF069.py](https://github.com/astral-sh/ruff/pull/22114/changes#diff-c23d55609dc272d6a87875a04cb84bfcc9ee07964e1bbb101b2506c6318e48cf) file and the fix works flawless.

---

_Review comment by @leandrobbraga on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__RUF069_RUF069.py.snap`:9 on 2026-01-15 00:28_

What do you think about this?

```console
$ cargo run -p ruff -- check --preview --select RUF069 - <<<'__all__ = ["A", "A", "B", "B"]'
RUF069 [*] `__all__` contains duplicate entries
 --> -:1:12
  |
1 | __all__ = ["A", "A", "B", "B"]
  |            ---  ^^^ `A` duplicated here
  |            |
  |            previous occurrence of `A` here
  |
help: Remove duplicate entries from `__all__`
  - __all__ = ["A", "A", "B", "B"]
1 + __all__ = ["A", "B", "B"]

RUF069 [*] `__all__` contains duplicate entries
 --> -:1:22
  |
1 | __all__ = ["A", "A", "B", "B"]
  |                      ---  ^^^ `B` duplicated here
  |                      |
  |                      previous occurrence of `B` here
  |
help: Remove duplicate entries from `__all__`
  - __all__ = ["A", "A", "B", "B"]
1 + __all__ = ["A", "A", "B"]

Found 2 errors.
[*] 2 fixable with the `--fix` option.
```

---

_@leandrobbraga reviewed on 2026-01-15 00:28_

---

_Review requested from @MichaReiser by @leandrobbraga on 2026-01-15 00:37_

---

_Review requested from @ntBre by @leandrobbraga on 2026-01-15 00:37_

---

_Comment by @leandrobbraga on 2026-01-15 00:38_

I should have addressed all concerns now, sorry for the delay

---

_Review requested from @chirizxc by @leandrobbraga on 2026-01-15 00:38_

---

_Comment by @chirizxc on 2026-01-15 14:10_

In general, it seems that we also have [code](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs) that repeats in this rule. Should we move some functions to helpers and reuse them?

Do something like this:
```rust
pub(crate) fn all_assignment<F>(
    checker: &Checker,
    target: &ast::Expr,
    value: &ast::Expr,
    call: F,
) where
    F: FnOnce(&Checker, &[ast::Expr]),
{

    let ast::Expr::Name(ast::ExprName { id, .. }) = target else {
        return;
    };

    if id != "__all__" {
        return;
    }

    if !checker.semantic().current_scope().kind.is_module() {
        return
    }

    let elts = match value {
        ast::Expr::List(ast::ExprList { elts, .. }) => elts,
        ast::Expr::Tuple(ast::ExprTuple { elts, .. }) => elts,
        _ => return,
    };

    call(checker, elts);
}
```


---

_Comment by @chirizxc on 2026-01-15 14:14_

It is possible to leave it as it is. However, if we decide to add more checks to the rule in the future, for example, to track `.append()`, we will have to change the code, and moving it to helpers may not be that useful.

---

_Comment by @leandrobbraga on 2026-01-15 22:18_

> In general, it seems that we also have [code](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/ruff/rules/sort_dunder_all.rs) that repeats in this rule. Should we move some functions to helpers and reuse them?
> 
> Do something like this:
> ```rust
> pub(crate) fn all_assignment<F>(
>     checker: &Checker,
>     target: &ast::Expr,
>     value: &ast::Expr,
>     call: F,
> ) where
>     F: FnOnce(&Checker, &[ast::Expr]),
> {
> 
>     let ast::Expr::Name(ast::ExprName { id, .. }) = target else {
>         return;
>     };
> 
>     if id != "__all__" {
>         return;
>     }
> 
>     if !checker.semantic().current_scope().kind.is_module() {
>         return
>     }
> 
>     let elts = match value {
>         ast::Expr::List(ast::ExprList { elts, .. }) => elts,
>         ast::Expr::Tuple(ast::ExprTuple { elts, .. }) => elts,
>         _ => return,
>     };
> 
>     call(checker, elts);
> }
> ```
> 

I guess we can defer this to later and have an specific MR for refactors. 

---

_Comment by @ntBre on 2026-01-15 23:04_

Yeah I think this is probably okay. I'll try to take another look tomorrow!

---

_@MichaReiser reviewed on 2026-01-16 08:53_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__RUF069_RUF069.py.snap`:9 on 2026-01-16 08:53_

This looks nice

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/ruff/RUF069.py`:15 on 2026-01-16 19:01_

I think this is actually what I meant in https://github.com/astral-sh/ruff/pull/22114#discussion_r2656477043:


```suggestion
__all__ = [A, "B", "B"]
```

Sorry for being quite pedantic, I just want to have one test case for the `continue` vs `return` choice.

---

_@ntBre approved on 2026-01-16 19:49_

Very nice, thank you!

I pushed a couple of commits applying my last test suggestion and re-coding the rule to avoid a gap at `RUF068`, but this is great.

---

_Renamed from "[ruff] Add RUF069 to detect duplicate entries in __all__" to "[`ruff`] Detect duplicate entries in `__all__` (`RUF068`)" by @ntBre on 2026-01-16 19:50_

---

_@ntBre reviewed on 2026-01-16 19:54_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_bugbear/rules/duplicate_value.rs`:74 on 2026-01-16 19:54_

I'll open a follow-up issue for this since it's not related to your PR, but this shouldn't always be a safe fix either:

```diff
❯ cat <<EOF | ruff check --diff --select B033 -
∙ {1, 2, 3,
# comment
1}
∙ EOF
@@ -1,3 +1 @@
-{1, 2, 3,
-# comment
-1}
+{1, 2, 3}

Would fix 1 error.
```

---

_Merged by @ntBre on 2026-01-16 19:58_

---

_Closed by @ntBre on 2026-01-16 19:58_

---

_Branch deleted on 2026-01-16 20:14_

---
