```yaml
number: 8822
title: "Add \"preserve\" quote-style to mimic Black's skip-string-normalization"
type: pull_request
state: merged
author: sciyoshi
labels:
  - formatter
assignees: []
merged: true
base: main
head: quote-style-preserve
created_at: 2023-11-22T20:32:46Z
updated_at: 2024-01-10T23:48:24Z
url: https://github.com/astral-sh/ruff/pull/8822
synced_at: 2026-01-12T15:55:27Z
```

# Add "preserve" quote-style to mimic Black's skip-string-normalization

---

_@sciyoshi_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Adds a new option to the `quote-style` formatter configuration called "preserve":

* normalizes the prefixes of all strings
* enforces double quotes for triple-quoted strings (docstrings and multiline strings)
* normalizes unnecessary escapes

In other words, using `preserve` leaves the quotes of inline-strings as is, but performs all other normalisation, the same as for `single` or `double`. 

Fixes #7525

## Test Plan

Added tests.  Tested that `quote-style="preserve` without `--preview` fails with an error.

Edited by @MichaReiser: I expanded the summary to be explicit about what `preserve` does and updated the test plan.


---

_Comment by @github-actions[bot] on 2023-11-22 20:48_

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

_@hussein-awala reviewed on 2023-11-26 18:15_

Nice one! +1

---

_Label `formatter` added by @MichaReiser on 2023-11-27 02:29_

---

_Label `needs-decision` added by @MichaReiser on 2023-11-27 02:29_

---

_Comment by @MichaReiser on 2023-11-27 07:33_

Thanks for implementing this new option. It is a good signal on how important this functionality is for the community. 

I haven't looked at the code yet and have some more catch-up work to do before I'll find the time for the review. I'm overall okay with adding this new option but I want to make sure that it removes the need for `--skip-string-normalization` because I want to avoid eventually having to support both.

---

_Comment by @charliermarsh on 2023-11-27 18:21_

Awesome, thanks @sciyoshi, great to see you back on the repo :) I have basically the same reaction: I support adding this, though it'd be nice if doing so could definitively preclude us from adding `--skip-string-normalization` in the future.

---

_Comment by @sciyoshi on 2023-11-27 19:12_

Thanks :) I think there's three cases to think about for the `preserve` option and how it compares to `skip-string-normalization`:

1. Whether it should keep existing string prefixes or normalize them 
2. Whether it should format docstrings to use double quotes
3. Whether it should normalize unnecessary escapes, e.g. `"\'"` ➡️`"'"`

Black's behavior with `skip-string-normalization` is to not touch any string prefixes or docstrings, and also to skip changing any character escapes. Personally, I think that having ruff normalize string prefixes and escapes (1. and 3.) in `preserve` mode is OK, although docstrings probably should be kept as-is. As it stands, this PR does all three (but for 2., only if the docstring is triple-quoted).

---

_Comment by @MichaReiser on 2023-11-28 02:14_

> Black's behavior with skip-string-normalization is to not touch any string prefixes or docstrings, and also to skip changing any character escapes. Personally, I think that having ruff normalize string prefixes and escapes (1. and 3.) in preserve mode is OK, although docstrings probably should be kept as-is. As it stands, this PR does all three (but for 2., only if the docstring is triple-quoted).

To make sure I understand you correctly. This PR

* normalizes the prefixes of all strings
* enforces double quotes for triple-quoted strings (docstrings and multiline strings)
* normalizes unnecessary escapes

In other words, using `preserve` leaves the quotes of inline-strings as is, but performs all other normalisation as with `single` or `double`. 



---

_Comment by @sciyoshi on 2023-11-28 02:26_

Correct, that's the current behavior of this PR:

```diff
--- example.py
+++ example.py
@@ -6,12 +6,12 @@

 R"double"

-B'single'
+b'single'

-B"double"
+b"double"

-"\' escape"
+"' escape"

-'''single triple'''
+"""single triple"""

 """double triple"""
```

---

_Comment by @edreamleo on 2023-11-29 09:49_

@MichaReiser

The "preserve" quote style looks like a step in the right direction. 

What would this PR do about this?
```python
s = '''
"""
Example docstring
"""
'''
```
Or this?
```python
return('''
"""
Example docstring
"""
'''
)
```
Are you *sure* you know what an "unnecessary" escape is? What about?
```python
pattern = re.compile('''
whatever
''')
```
Or this?
```python
pattern = re.compile(fr"""
whatever
""")
```
Finally, what about backslash-newlines? 
```python
s = textwrap.dedent("""\
unit test data
""")
```
Removing the trailing backslash would break some of Leo's unit tests.

---

_Comment by @zanieb on 2023-11-29 16:08_

@edreamleo the result for your examples would be

```python
import re
import textwrap

s = '''"""
Example docstring
"""
'''

def foo():
    return('''
    """
    Example docstring
    """
    '''
    )

pattern = re.compile('''
whatever
''')

pattern = re.compile(fr"""
whatever
""")

s = textwrap.dedent("""\
unit test data
""")
```

```diff
--- example.py
+++ example.py
@@ -6,22 +6,29 @@
 """
 '''
 
+
 def foo():
-    return('''
+    return '''
     """
     Example docstring
     """
     '''
-    )
 
-pattern = re.compile('''
+
+pattern = re.compile(
+    """
 whatever
-''')
+"""
+)
 
-pattern = re.compile(fr"""
+pattern = re.compile(
+    rf"""
 whatever
-""")
+"""
+)
 
-s = textwrap.dedent("""\
+s = textwrap.dedent(
+    """\
 unit test data
-""")
+"""
+)
```

This happens to be identical to the formatting _without_ the `preserve` option.

---

_Comment by @edreamleo on 2023-11-29 16:15_

@zanieb Thanks for your comment!

My only remaining concern is the backslash-newline question. By "whatever" I meant various cases containing escapes that might be considered "unnecessary". Perhaps this PR has no effect on that question.

---

_Comment by @sciyoshi on 2023-11-30 04:48_

I've added an argument to `ruff format` which will allow setting `--quote-style` via the CLI.

Backslash-newlines will be preserved, as will other escape sequences. From my testing I think it is only unnecessarily-escaped quotes that are changed.

If @MichaReiser and @charliermarsh are OK with it, I think this could probably be merged as-is. Nothing precludes us from changing the behavior of `preserve` to be weaker if there are still concerns with the reformatting of triple quotes & string prefixes - making it stronger on the other hand would be a more of a breaking change. If on the other hand we want `preserve` to match `skip-string-normalization` exactly, I can take a look at changing that behavior as well. We probably do also want a test or two for this option.

---

_Comment by @MichaReiser on 2023-11-30 04:58_

> I've added an argument to ruff format which will allow setting --quote-style via the CLI.

We've intentionally omitted any other options than `--line-length` for now. Let's not mix this PR with other changes.

> Nothing precludes us from changing the behavior of preserve to be weaker if there are still concerns with the reformatting of triple quotes & string prefixes - making it stronger on the other hand would be a more of a breaking change

Any other semantic than what we've defined in this PR would be confusing in my view, because the scope of the `quote-style` setting than depends on the value which makes it harder to predict what changing the value does. 

> If on the other hand we want preserve to match skip-string-normalization exactly, I can take a look at changing that behavior as well. 

In that case, I would prefer having a dedicated `--skip-string-normalization` option instead to ensure all `quote-style` settings have the same semantics (they only change the outcome, but they configure the same functionality).

> We probably do also want a test or two for this option.

That would be great. You can probably even use an existing test and add an `<test-name>.options.json` file and add a test case for `quote-style: "preserve"`. For example see `docstring.options.json`.




---

_Comment by @sciyoshi on 2023-12-03 02:05_

> We've intentionally omitted any other options than `--line-length` for now. Let's not mix this PR with other changes.

No problem, I've now removed those.

> Any other semantic than what we've defined in this PR would be confusing in my view, because the scope of the `quote-style` setting than depends on the value which makes it harder to predict what changing the value does.

> In that case, I would prefer having a dedicated `--skip-string-normalization` option instead to ensure all `quote-style` settings have the same semantics (they only change the outcome, but they configure the same functionality).

I don't quite follow, are you saying that the issue is with the name `quote-style` since the option might change things other than just the string quotes?

I don't think Ruff will need to add a separate `skip-string-normalization` option regardless. This option would be mutually exclusive with any value of `quote-style`, so it would be confusing (you could only set one or the other). Having `quote-style=preserve` as another value is equivalent and removes that ambiguity. The reason Black added it under that name is probably because it doesn't support configuring string quoting otherwise.

> That would be great. You can probably even use an existing test and add an `<test-name>.options.json` file and add a test case for `quote-style: "preserve"`. For example see `docstring.options.json`.

Thanks for the tip - I added some simple tests to this PR. Let me know if there's other cases we should be checking.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/options.rs`:210 on 2023-12-05 05:04_

We need to update the documentation of the `FormatOption::quote_style` field to make the change reflect on our website:

https://github.com/astral-sh/ruff/blob/578ddf1bb1e8edef50cda9981744ba5fe825867d/crates/ruff_workspace/src/options.rs#L2804-L2829

---

_@MichaReiser reviewed on 2023-12-05 05:09_

I made a few changes to the code and introduced a `QuoteChar` to avoid having to handle `Preserve` in situations where `Preserve` isn't a valid variant (and there's no good way of e.g. implementing `invert`). 

I also extended your tests by a few docstring examples and noticed that single-quoted docstrings remain unchanged (see failing tests). This isn't specific to `preserve`, the docstring quotes also remain unchanged when using `quote-style: single`. 

@charliermarsh I belive you changed the formatter to enforce double quotes for all docstrings and triple quoted strings. Was it an intentional decision to handle single quoted docstrings the same as inline strings or do you think this should be changed? 

Implementation note: The string formatting should know that it is a docstring by check its layout.

---

_Label `needs-decision` removed by @MichaReiser on 2023-12-05 05:09_

---

_Review requested from @BurntSushi by @MichaReiser on 2023-12-07 05:47_

---

_Review requested from @charliermarsh by @MichaReiser on 2023-12-07 05:47_

---

_Comment by @MichaReiser on 2023-12-07 05:51_

I rebased the changes, updated the documentation, and gated the new option behind the `--preview` flag to get more feedback. 

Something I noticed when updating the documentation is that `preserve` is special in the sense that it won't pick the quote that requires the few escapes, at least not the way it's implemented now. This is a slight deviation from `single` and `double` but I think it's subtle enough that adding `preserve` to quote-style is fine.

The reason for gating this behind preview is that we had some internal discussions and @charliermarsh brought up this valid concern:

> I think more users are likely to be confused that preserve changes docstrings than they are happy that preserve changes docstrings.

We gate this behind preview because we want to ship the functionality that allows users to preserve their quote style, but we need to gather more feedback if configuring this using `quote-style` is the right way to go. 


@charliermarsh or @BurntSushi would appreciate if you could review the changes that I made to this PR.


---

_@MichaReiser approved on 2023-12-07 05:51_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/options.rs`:218 on 2023-12-07 14:03_

Maybe a dumb question, but what does "not used" mean here? Would it make sense to have this return an `Option<char>` instead or is it not worth it?

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/options.rs`:218 on 2023-12-07 14:06_

Oh nice, you fixed this in a subsequent commit!

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/tests/snapshots/format@quote_style.py.snap`:266 on 2023-12-07 14:13_

Just to clarify here (given what @charliermarsh said), we are starting with normalizing docstring using single quotes to double quotes (even with `preserve=true`)?

---

_@BurntSushi approved on 2023-12-07 14:14_

Nice!

One other thought here is whether it makes sense to document the nooks and crannies of the `preserve` option. That is, its impact (or not) on docstring quoting and how it interacts with escaping and what not.

---

_@sciyoshi reviewed on 2023-12-07 14:23_

---

_Review comment by @sciyoshi on `crates/ruff_python_formatter/tests/snapshots/format@quote_style.py.snap`:266 on 2023-12-07 14:23_

Correct, that's the current behavior of this PR. Like @charliermarsh this is probably unexpected behavior, but can be changed later if we hear feedback about it.

---

_@charliermarsh approved on 2023-12-07 14:26_

---

_Comment by @sciyoshi on 2023-12-07 14:26_

> I think more users are likely to be confused that preserve changes docstrings than they are happy that preserve changes docstrings.

I agree with this, although like I mentioned this can be relaxed in a followup without breaking compatibility.

> We need to gather more feedback if configuring this using quote-style is the right way to go.

I think adding a separate option for this would be a mistake - `preserve` is an aid to adopting ruff, and `single`/`double` are the alternative configurations. If the confusion is that this changes things other than quotes, then maybe the option should be renamed to `string-style` or just `strings`.

---

_Comment by @charliermarsh on 2023-12-07 14:29_

> I agree with this, although like I mentioned this can be relaxed in a followup without breaking compatibility.

This is my feeling as well.

> I think adding a separate option for this would be a mistake - preserve is an aid to adopting ruff, and single/double are the alternative configurations. If the confusion is that this changes things other than quotes, then maybe the option should be renamed to string-style or just strings.

(Just to clarify, I was referring here to the confusion that `preserve` changes quotes, not that it may change things _other_ than quotes.)


---

_@BurntSushi reviewed on 2023-12-07 14:34_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/tests/snapshots/format@quote_style.py.snap`:266 on 2023-12-07 14:34_

SGTM!

---

_Comment by @MichaReiser on 2023-12-07 23:59_

> I agree with this, although like I mentioned this can be relaxed in a followup without breaking compatibility.

I continue to feel conflicted about this. The documentation for `quote-style` already become more complicated because some parts no longer apply for `preserve`. Potentially preserving all quotes styles and having this configured under the same setting will make this worse. 

The other challenge is that we wouldn't be able to support enforcing double quotes for triple quoted strings and docstrings but preserve the other quotes. 

Anyway, I think this is something we can re-visit when we get more feedback. This looks good to me. Thanks for pushing us to revisit adding this option by creating the PR. 

---

_Merged by @MichaReiser on 2023-12-07 23:59_

---

_Closed by @MichaReiser on 2023-12-07 23:59_

---

_Branch deleted on 2023-12-14 04:02_

---

_Comment by @genericmoniker on 2023-12-15 23:03_

I was trying out the "preserve" quote-style on a code base formatted with black using "skip-string-normalization" and was surprised to see non-docstring multiline strings using `'''` have their quotes replaced. I came looking for a bug on that, but discovered it was intentional.

> enforces double quotes for triple-quoted strings (docstrings and multiline strings)

Just a little feedback.

---

_Comment by @MichaReiser on 2023-12-16 02:24_

Thanks for the feedback. Does this behavior prevent you from adopting ruff or are okay with the changes that it introduces?

---

_Comment by @genericmoniker on 2023-12-16 15:47_

I wouldn't say the behavior prevents adoption, but it does add a little friction. If there were only a small number of changes between black and ruff on our code base, I'd just open a pull request to switch to ruff and expect my team to be happy that formatting is faster. Without "preserve" there are hundreds of files changed. Since "preserve" is only currently available with the preview setting and that changes other things, using it also results in many files reformatted. 

I'm personally fine normalizing all quotes, and I imagine my teammates probably are too, but it becomes a point of discussion when lots of files change and cause merge conflicts with their outstanding pull requests.

**Edit**: I do notice that PEP 8 says:

> In Python, single-quoted strings and double-quoted strings are the same. This PEP does not make a recommendation for this....
> For triple-quoted strings, always use double quote characters...

So the behavior seems good.


---

_Comment by @lucas-labs on 2024-01-10 23:48_

Just like @genericmoniker I also expected `preserve` to completely skip quotes normalization (just like black's `--skip-string-normalization` which is what we've been using on our code base). I understand this is by-design but I guess it would at least delay adopting ruff until we have fewer ongoing pull requests to avoid conflicts (I mean, to adopt it as a formatter, obviously; we are already using ruff as a linter and its awesome).




---
