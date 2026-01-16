```yaml
number: 22234
title: "[`refurb`] Do not add `abc.ABC` if already present (`FURB180`)"
type: pull_request
state: open
author: akawd
labels:
  - fixes
assignees: []
base: main
head: issue/17162/do-not-add-abc-if-already-added
created_at: 2025-12-28T11:38:46Z
updated_at: 2026-01-16T21:17:30Z
url: https://github.com/astral-sh/ruff/pull/22234
synced_at: 2026-01-16T22:15:00Z
```

# [`refurb`] Do not add `abc.ABC` if already present (`FURB180`)

---

_@akawd_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

According to the `FURB180` rule, `abc.ABC` should be used instead of `metaclass=abc.ABCMeta`. This issue can be fixed automatically, but if the class being checked already contains `abc.ABC`, it would be added a second time.

Fixes: #17162

## Test Plan

I ran `cargo test refurb`.  
For checking the "fix" I used this python file:
<details>

<summary>Python file (input)</summary>

```python
import abc
from abc import abstractmethod, ABCMeta, ABC
from abc import ABC as ABCAnotherName

class ParentClass:
    ...

from abc import ABC, ABCMeta

class Foo(metaclass=ABCMeta):
    ...
    
class Foo2(abc.ABC, ParentClass, metaclass=ABCMeta):
    ...
    
class Foo3(ParentClass, ABC, metaclass=ABCMeta):
    ...
    
class Foo4(ABCAnotherName, metaclass=ABCMeta):
    ...
```

</details>

The output after applying the fix is so:
<details>
<summary>Python file (output)</summary>

```python
import abc
from abc import abstractmethod, ABCMeta, ABC
from abc import ABC as ABCAnotherName

class ParentClass:
    ...

from abc import ABC, ABCMeta

class Foo(ABC):
    ...
    
class Foo2(abc.ABC, ParentClass, ):
    ...
    
class Foo3(ParentClass, ABC, ):
    ...
    
class Foo4(ABCAnotherName, ):
    ...
```

</details>

Note: Trailing commas may remain after the automatic fix. It is unclear whether this is an issue.


---

_Review requested from @ntBre by @ntBre on 2025-12-29 14:18_

---

_Label `fixes` added by @ntBre on 2026-01-01 14:24_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/metaclass_abcmeta.rs`:136 on 2026-01-01 14:26_

nit: is there an `Edit::range_deletion`? It looks like we could use that with `keyword.range()` here.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/metaclass_abcmeta.rs`:100 on 2026-01-01 14:27_

We have an [`any_qualified_base_class`](https://github.com/astral-sh/ruff/blob/b2b9d91859f2abc017e35d22f01696f141f6f5b1/crates/ruff_python_semantic/src/analyze/class.rs#L16) helper function that I think we could use here.

`is_has_abc` also reads a bit strangely to me, maybe just `has_abc` would be better.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/metaclass_abcmeta.rs`:120 on 2026-01-01 14:29_

I think we should be able to use a slice here to avoid allocating vecs:


```suggestion
            let rest = if is_has_abc {
                &[]
            } else {
                &[
                    Edit::insertion(format!("{binding}, "), class_def.keywords()[0].start()),
                    import_edit,
                ]
            };
```

You might have to annotate `rest` as `&[Edit]`, though.

---

_@ntBre requested changes on 2026-01-01 14:34_

Thanks! This looks good to me overall, I just had a couple of small suggestions. We should also add your manual tests from the PR summary as regression tests. At the bottom of this file would be a good place for them:

https://github.com/astral-sh/ruff/blob/b2b9d91859f2abc017e35d22f01696f141f6f5b1/crates/ruff_linter/resources/test/fixtures/refurb/FURB180.py#L58

---

_Renamed from "[refurb] Do not add abc.ABC if already present" to "[`refurb`] Do not add `abc.ABC` if already present (`FURB180`)" by @ntBre on 2026-01-01 14:35_

---

_Comment by @astral-sh-bot[bot] on 2026-01-01 14:35_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -7 violations, +0 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/core/query.py#L53'>src/bokeh/core/query.py:53:5:</a> RUF068 [*] `__all__` contains duplicate entries: `find` duplicated here
</pre>

</p>
</details>
<details><summary><a href="https://github.com/langchain-ai/langchain">langchain-ai/langchain</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/langchain-ai/langchain/blob/a84722e2d768a3029a632d0ab3515b897ebba63b/libs/langchain/langchain_classic/document_loaders/__init__.py#L373'>libs/langchain/langchain_classic/document_loaders/__init__.py:373:5:</a> RUF068 [*] `__all__` contains duplicate entries: `AcreomLoader` duplicated here
- <a href='https://github.com/langchain-ai/langchain/blob/a84722e2d768a3029a632d0ab3515b897ebba63b/libs/langchain/langchain_classic/document_loaders/__init__.py#L391'>libs/langchain/langchain_classic/document_loaders/__init__.py:391:5:</a> RUF068 [*] `__all__` contains duplicate entries: `AsyncHtmlLoader` duplicated here
</pre>

</p>
</details>
<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
- <a href='https://github.com/python/typeshed/blob/b94c9e8e48f552b2f2b979b69937b0e4848675cf/stubs/convertdate/convertdate/__init__.pyi#L45'>stubs/convertdate/convertdate/__init__.pyi:45:5:</a> RUF068 [*] `__all__` contains duplicate entries: `mayan` duplicated here
- <a href='https://github.com/python/typeshed/blob/b94c9e8e48f552b2f2b979b69937b0e4848675cf/stubs/reportlab/reportlab/lib/rltempfile.pyi#L4'>stubs/reportlab/reportlab/lib/rltempfile.pyi:4:30:</a> RUF068 [*] `__all__` contains duplicate entries: `get_rl_tempdir` duplicated here
- <a href='https://github.com/python/typeshed/blob/b94c9e8e48f552b2f2b979b69937b0e4848675cf/stubs/workalendar/workalendar/europe/__init__.pyi#L252'>stubs/workalendar/workalendar/europe/__init__.pyi:252:5:</a> RUF068 [*] `__all__` contains duplicate entries: `Switzerland` duplicated here
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/astropy/astropy/blob/b5754a667932b7c39b5f425abb2de77a7962fa9c/astropy/table/__init__.py#L32'>astropy/table/__init__.py:32:5:</a> RUF068 [*] `__all__` contains duplicate entries: `conf` duplicated here
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF068 | 7 | 0 | 7 | 0 | 0 |

</p>
</details>





---

_@akawd reviewed on 2026-01-02 08:51_

---

_Review comment by @akawd on `crates/ruff_linter/src/rules/refurb/rules/metaclass_abcmeta.rs`:136 on 2026-01-02 08:51_

Changed to `Edit::range_deletion`.

---

_@akawd reviewed on 2026-01-02 08:52_

---

_Review comment by @akawd on `crates/ruff_linter/src/rules/refurb/rules/metaclass_abcmeta.rs`:100 on 2026-01-02 08:52_

Switched to using [any_qualified_base_class](https://github.com/astral-sh/ruff/blob/b2b9d91859f2abc017e35d22f01696f141f6f5b1/crates/ruff_python_semantic/src/analyze/class.rs#L16).

---

_@akawd reviewed on 2026-01-02 08:54_

---

_Review comment by @akawd on `crates/ruff_linter/src/rules/refurb/rules/metaclass_abcmeta.rs`:120 on 2026-01-02 08:54_

Thank you for this advice, done.
P.S.: I had to use `iter().cloned()` because of https://github.com/akawd/ruff/blob/issue/17162/do-not-add-abc-if-already-added/crates/ruff_diagnostics/src/fix.rs#L130

---

_Comment by @akawd on 2026-01-02 09:00_

According to @"... add your manual tests from the PR summary as regression tests." - I added the following changes to `FURB180.py`:

```python

from abc import abstractmethod, ABCMeta, ABC
from abc import ABC as ABCAlternativeName

...
class A7(ABC):
    @abstractmethod
    def foo(self): pass

class A8(ABCAlternativeName):
    @abstractmethod
    def foo(self): pass  
...
```

I hope this is what was expected.

---

_Review requested from @ntBre by @akawd on 2026-01-02 09:06_

---

_@ntBre reviewed on 2026-01-02 14:57_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/metaclass_abcmeta.rs`:120 on 2026-01-02 14:57_

Ah right, my mistake. Let's just go back to the vecs. I forgot the slices would change the type of the iterator.

---

_Comment by @ntBre on 2026-01-02 14:58_

Thank you! That was close to what I had in mind, but I think your tests in the summary were a bit more helpful than the ones added to FURB180.py. I just pushed two commits updating the tests. A couple of things I tried to fix:
- I avoided changing any lines earlier in the file, this is nice for review because it doesn't shift the line numbers in the existing snapshots
- I added `metaclass=ABCMeta` to the new test cases so that FURB180 triggers
- I added `class A11` showing off the parent class traversal that `any_qualified_base_class` gets us

After making these changes, it showed that there's an issue with the fix:

```diff
- class A11(MyMetaClass, metaclass=ABCMeta):  # FURB180
+ class A11(MyMetaClass, ):  # FURB180
```

We're leaving a trailing comma after removing the metaclass argument. I think we might want to try the [`remove_argument`](https://github.com/astral-sh/ruff/blob/3cbe1e613c2dfc4b536130e635cb714bc7c0b375/crates/ruff_linter/src/fix/edits.rs#L206) helper function here. Sorry, I should have recommended that sooner!

---

_@akawd reviewed on 2026-01-02 18:54_

---

_Review comment by @akawd on `crates/ruff_linter/src/rules/refurb/rules/metaclass_abcmeta.rs`:120 on 2026-01-02 18:54_

Anyway it was an interesting idea (but not for this particular case, looks like). Reverted.
https://github.com/astral-sh/ruff/pull/22234/commits/aa524c82a18e12d890726554031b65745019679b
P.S.: And sorry for the mess with tests, I did not think the line numbers are important.

---

_Converted to draft by @akawd on 2026-01-02 19:24_

---

_Comment by @akawd on 2026-01-02 19:26_

According to: "We're leaving a trailing comma after removing the metaclass argument".
Marked as draft to cover this issue.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/metaclass_abcmeta.rs`:120 on 2026-01-02 20:02_

Thank you! And no worries, the line numbers aren't a huge deal, just a little nicer for review :) Otherwise the formatting you had would be much nicer in normal circumstances.

---

_@ntBre reviewed on 2026-01-02 20:02_

---

_Comment by @akawd on 2026-01-03 07:23_

> We're leaving a trailing comma after removing the metaclass argument. I think we might want to try the [`remove_argument`](https://github.com/astral-sh/ruff/blob/3cbe1e613c2dfc4b536130e635cb714bc7c0b375/crates/ruff_linter/src/fix/edits.rs#L206) helper function here. 

Fixed the issue above here: https://github.com/astral-sh/ruff/pull/22234/commits/740da9e2808d8fda3616f8467dd706cca42ff7c0

Example.

<details>

<summary>Before fix:</summary>

```python
import abc
from abc import abstractmethod, ABCMeta, ABC
from abc import ABC as ABCAnotherName

class ParentClass:
    ...

class Foo(metaclass=ABCMeta):
    ...
    
class Foo2(abc.ABC, ParentClass, metaclass=ABCMeta):
    ...
    
class Foo3(ParentClass, ABC, metaclass=ABCMeta):
    ...
    
class Foo4(ABCAnotherName, metaclass=ABCMeta):
    ...
```

</details>

<details>

<summary>After fix:</summary>

```python
import abc
from abc import abstractmethod, ABCMeta, ABC
from abc import ABC as ABCAnotherName

class ParentClass:
    ...

class Foo(ABCAnotherName):
    ...
    
class Foo2(abc.ABC, ParentClass):
    ...
    
class Foo3(ParentClass, ABC):
    ...
    
class Foo4(ABCAnotherName):
    ...
```

</details>

---

_Marked ready for review by @akawd on 2026-01-03 07:24_

---

_Comment by @akawd on 2026-01-03 07:27_

And since there are several commits here, do I need to squash them?

---

_Comment by @akawd on 2026-01-13 18:43_

Hello, @ntBre . Just a friendly reminder and request to take a look at this if possible.

---

_Comment by @ntBre on 2026-01-13 20:08_

Thank you! I was on PTO last week, but I'm back now :) I'll take a look soon.

> And since there are several commits here, do I need to squash them?

Don't worry about squashing, we'll squash-merge at the end, so it's easier to keep all the commits for review in the meantime.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/metaclass_abcmeta.rs`:68 on 2026-01-13 20:12_

```suggestion
    // Determine whether the class definition contains at least one argument.
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/metaclass_abcmeta.rs`:153 on 2026-01-13 20:25_

What do you think about something like this:

```rust
        let delete_keyword = remove_argument(
            keyword,
            arguments,
            Parentheses::Remove,
            checker.source(),
            checker.tokens(),
        )?;

        Ok(if position > 0 {
            // When the `abc.ABCMeta` is not the first keyword and `abc.ABC` is not
            // in base classes put `abc.ABC` before the first keyword argument.
            if has_abc {
                Fix::applicable_edit(delete_keyword, applicability)
            } else {
                Fix::applicable_edits(
                    delete_keyword,
                    [
                        Edit::insertion(format!("{binding}, "), class_def.keywords()[0].start()),
                        import_edit,
                    ],
                    applicability,
                )
            }
        } else {
            let edit_action = if has_abc {
                // Class already inherits the `abc.ABC`, delete the `metaclass` keyword only.
                delete_keyword
            } else {
                // Replace `metaclass` keyword by `abc.ABC`.
                Edit::range_replacement(binding, keyword.range)
            };

            Fix::applicable_edits(edit_action, [import_edit], applicability)
        })
```

I factored out the `delete_keyword` edit and got rid of the `Vec`s by using two different `Fix` constructors. I don't feel too strongly about the second part, but I think factoring out the shared deletion edit definitely makes sense.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/metaclass_abcmeta.rs`:92 on 2026-01-13 20:32_

I think this is a pre-existing issue, but I'm pretty sure the fix should also be unsafe if there are comments in the range of the deletion. For example,

```py
import abc


class C(
    other_kwarg=1,
    # comment
    metaclass=abc.ABCMeta,
):
    pass
```

This comment gets [deleted](https://play.ruff.rs/f64dfdc1-811d-466f-8e49-0f400e1859d0) but wouldn't make the fix unsafe on main as far as I can tell.

Feel free to fix that here if you want, or I can open an issue for follow-up. We usually use a pattern like this:

https://github.com/astral-sh/ruff/blob/55a335165ab126d50edb89e775c0c5c340cb8ae3/crates/ruff_linter/src/rules/flake8_return/rules/function.rs#L406-L410

We'd also need to update the docs.

---

_@ntBre reviewed on 2026-01-13 20:34_

Thank you! This looks great, I just had a couple of minor suggestions. I also noticed a potential issue with the fix safety, but I can turn that into a separate issue if you'd rather not update it here.

---

_Review comment by @akawd on `crates/ruff_linter/src/rules/refurb/rules/metaclass_abcmeta.rs`:153 on 2026-01-16 19:59_

In my humble opinion, avoiding the heap is worth a few extra lines of code.

---

_@akawd reviewed on 2026-01-16 19:59_

---

_@akawd reviewed on 2026-01-16 20:07_

---

_Review comment by @akawd on `crates/ruff_linter/src/rules/refurb/rules/metaclass_abcmeta.rs`:92 on 2026-01-16 20:07_

As far as I can see, the root cause of the comment deletion is (somewhere) here:
https://github.com/akawd/ruff/blob/d08e41417971f1d05b9daa75f794536a1dd4bedf/crates/ruff_linter/src/fix/edits.rs#L248

If this is a real issue, it may be better to be addressed in a separate issue?
Here, I simply added the unsafe marker to the class containing the comment:
https://github.com/astral-sh/ruff/pull/22234/changes/a2091751f553191f7af42777def94d5e522c423b

---

_Comment by @akawd on 2026-01-16 20:14_

@ntBre , sorry for delay with the answer. 
Thank you for your suggestions, I added them.

Could you please clarify what docs you meant here?

> We'd also need to update the docs.

---

_Review requested from @ntBre by @akawd on 2026-01-16 20:14_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/metaclass_abcmeta.rs`:92 on 2026-01-16 20:49_

Sorry, I think my comment here may have been a bit confusing! I'll open a separate issue for this and try to explain the problem better.

Edit: https://github.com/astral-sh/ruff/issues/22631

---

_@ntBre reviewed on 2026-01-16 20:49_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/metaclass_abcmeta.rs`:92 on 2026-01-16 20:57_

Oh oops, you took care of this, nice work! I should have looked at the code first. We just need to update the fix safety documentation on line 42 of this file to say that the fix can also be unsafe if it would remove comments in the base class list.

---

_@ntBre reviewed on 2026-01-16 20:57_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/metaclass_abcmeta.rs`:95 on 2026-01-16 20:59_

I think we could narrow this to `keyword.range` instead of the whole `arguments.range()`, but I'm not totally sure:

```suggestion
    } else if checker.comment_ranges().intersects(keyword.range()) {
```

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/refurb/FURB180.py`:92 on 2026-01-16 21:02_

Could you add a test case for the unsafe comment deletion? I think something like this example would work well:

```py
class C(
    other_kwarg=1,
    # comment
    metaclass=abc.ABCMeta,
):
    pass
```

---

_@ntBre reviewed on 2026-01-16 21:04_

Thank you! I had one suggestion about narrowing the comment range check and a test for that. Then we just need to update the `## Fix safety` section of the rule docs.

---
