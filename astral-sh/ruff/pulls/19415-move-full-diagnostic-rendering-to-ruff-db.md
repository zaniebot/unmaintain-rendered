```yaml
number: 19415
title: "Move full diagnostic rendering to `ruff_db`"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
  - diagnostics
assignees: []
merged: true
base: main
head: brent/full
created_at: 2025-07-17T22:19:32Z
updated_at: 2025-08-08T16:58:55Z
url: https://github.com/astral-sh/ruff/pull/19415
synced_at: 2026-01-10T17:52:17Z
```

# Move full diagnostic rendering to `ruff_db`

---

_Pull request opened by @ntBre on 2025-07-17 22:19_

## Summary

This PR switches the `full` output format in Ruff over to use the rendering code
in `ruff_db`. As proposed in the design doc, this involves a lot of
changes to the snapshot output.

I also had to comment out this assertion with a TODO to replace it after https://github.com/astral-sh/ruff/issues/19688 because many of Ruff's "file-level" annotations aren't actually file-level. They just happen to occur at the start of the file, especially in tests with very short snippets.

https://github.com/astral-sh/ruff/blob/529d81daca30e0af93a18d7406bd2c419008e8f4/crates/ruff_annotate_snippets/src/renderer/display_list.rs#L1204-L1208

I broke up the snapshot commits at the end into several blocks, but I don't think it's enough to help with review. The first few (notebooks, syntax errors, and test rules) are small enough to look at, but I couldn't really think of other categories beyond that. I'm happy to break those up or pick out specific examples beyond what I have below, if that would help.

The minimal code changes are in this [range](https://github.com/astral-sh/ruff/pull/19415/files/abd28f1e776e442379186e9f24c9aa6290f64163), with the snapshot commits following. Moving the `FullRenderer` and updating the `EmitterFlags` aren't strictly necessary either. I even dropped the renderer commit this morning but figured it made sense to keep it since we have the `full` module for tests. I don't feel strongly either way.

## Test Plan

I did actually click through all 1700 snapshots individually instead of
accepting them all at once, although I moved through them quickly. There are a
few main categories:

### Lint diagnostics

```diff
-unused.py:8:19: F401 [*] `pathlib` imported but unused
+F401 [*] `pathlib` imported but unused
+  --> unused.py:8:19
    |
  7 | # Unused, _not_ marked as required (due to the alias).
  8 | import pathlib as non_alias
-   |                   ^^^^^^^^^ F401
+   |                   ^^^^^^^^^
  9 |
 10 | # Unused, marked as required.
    |
-   = help: Remove unused import: `pathlib`
+help: Remove unused import: `pathlib`
```

- The filename and line numbers are moved to the second line
- The second noqa code next to the underline is removed

### Syntax errors

These are much like the above.

```diff
-    -:1:16: invalid-syntax: Expected one or more symbol names after import
+    invalid-syntax: Expected one or more symbol names after import
+     --> -:1:16
       |
     1 | from foo import
       |                ^
```

One thing I noticed while reviewing some of these, but I don't think is strictly syntax-error-related, is that some of the new diagnostics have a little less context after the error. I don't think this is a problem, but it's one small discrepancy I hadn't noticed before. Here's a minor example:

```diff
-syntax_errors.py:1:15: invalid-syntax: Expected one or more symbol names after import
+invalid-syntax: Expected one or more symbol names after import
+ --> syntax_errors.py:1:15
   |
 1 | from os import
   |               ^
 2 |
 3 | if call(foo
-4 |     def bar():
   |
```

And one of the biggest examples:

```diff
-E30_syntax_error.py:18:11: invalid-syntax: Expected ')', found newline
+invalid-syntax: Expected ')', found newline
+  --> E30_syntax_error.py:18:11
    |
 16 |         pass
 17 |
 18 | foo = Foo(
    |           ^
-19 |
-20 |
-21 | def top(
    |
```

Similarly, a few of the lint diagnostics showed that the cut indicator calculation for overly long lines is also slightly different, but I think that's okay too.

### Full-file diagnostics

```diff
-comment.py:1:1: I002 [*] Missing required import: `from __future__ import annotations`
+I002 [*] Missing required import: `from __future__ import annotations`
+--> comment.py:1:1
+help: Insert required import: `from __future__ import annotations`
+
```

As noted above, these will be much more rare after #19688 too. This case isn't a true full-file diagnostic and will render a snippet in the future, but you can see that we're now rendering the help message that would have been discarded before. In contrast, this is a true full-file diagnostic and should still look like this after #19688:

```diff
-__init__.py:1:1: A005 Module `logging` shadows a Python standard-library module
+A005 Module `logging` shadows a Python standard-library module
+--> __init__.py:1:1
```

### Jupyter notebooks

There's nothing particularly different about these, just showing off the cell index again.

```diff
-    Jupyter.ipynb:cell 3:1:7: F821 Undefined name `x`
+    F821 Undefined name `x`
+     --> Jupyter.ipynb:cell 3:1:7
       |
     1 | print(x)
-      |       ^ F821
+      |       ^
       |
```

---

_Label `internal` added by @ntBre on 2025-07-17 22:19_

---

_Label `diagnostics` added by @ntBre on 2025-07-17 22:19_

---

_Comment by @github-actions[bot] on 2025-07-17 22:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2025-07-18 07:24_

Thank you for looking into this.

I'm not sure I understand what the diff shown for **Diagnostics with FixAvailability::Never** or **Sometimes** has to do with the fix availability. 

I think it would be more helpful to list the separate differences:

* The annotated message now shows the help text instead of the rule code (makes sense to me, but I haven't reviewed all snapshot changes)
* The line/column and filename moved: This makes sense to me and is a necessity to support multifile spans
* Full file diagnostics: @BurntSushi didn't you add some special casing for it to ruff's diagnostic rendering? 

---

_Comment by @MichaReiser on 2025-07-18 11:06_

> These are the only ones I noticed that I wasn't very happy with. I tried to
address these by making the Range on Ruff's diagnostics None

We can't change the range or that would be breaking where suppression comments are allowed. We also can't remove the range or the diagnostic can no longer be suppressed. 


What I don't understand in your example. Is the rendered code frame just empty? Does it collapse lines or does it render the full file?



---

_Comment by @MichaReiser on 2025-07-18 11:24_

> What I don't understand in your example. Is the rendered code frame just empty? Does it collapse lines or does it render the full file?

Okay, I think I understand now what's happening. The issue is that ruff uses empty ranges for file level diagnostics. I think the ideal output would be to only render the file name if both the range and message are empty. 

We could decide to make `Range` optional but I think we should keep defaulting to an empty range and special case that in the rendering code. My reasoning is that the range is important to know where to place suppressions AND diagnostics like IO errors have truely no range and we don't want to render line column numbers at all. 

>  but it still left a single blank line. 

@BurntSushi might be able to help but you might have to bite the bullet and try to find the source of the empty line yourself.

It's also worth double checking if the new renderer handles these tricky cases

* https://github.com/astral-sh/ruff/blob/77a5c5ac80cfe77def2397e80ed9a7d79e068b34/crates/ruff_linter/src/message/text.rs#L374-L407
* https://github.com/astral-sh/ruff/blob/9aaed4f04b243b092a5c2df7283d4883359476aa/crates/ruff_linter/src/message/text.rs#L287
* https://github.com/astral-sh/ruff/blob/77a5c5ac80cfe77def2397e80ed9a7d79e068b34/crates/ruff_linter/src/message/text.rs#L299

---

_Comment by @ntBre on 2025-07-18 12:02_

> I'm not sure I understand what the diff shown for **Diagnostics with FixAvailability::Never** or **Sometimes** has to do with the fix availability.

Oh sorry, I was using that as a proxy for having a `fix_title` method and thus help text. I'll update those sections with your suggestions.

>> I tried to address these by making the Range on Ruff's diagnostics None

> We can't change the range or that would be breaking where suppression comments are allowed. We also can't remove the range or the diagnostic can no longer be suppressed.

Oops, this was really poorly phrased, but I think you still figured out what I meant in your last comment. Yes, we use `TextRange::default` as a marker for full-file diagnostics:

https://github.com/astral-sh/ruff/blob/6403eba9e907cfa1e5e0d16ad99b8d998a6750ae/crates/ruff_linter/src/message/text.rs#L119-L121

I tried making _those_ `None`, not all Ruff ranges.

> I think the ideal output would be to only render the file name if both the range and message are empty.

I totally agree. I got close to that, modulo the blank line I need to track down in `annotate-snippets`, at least I think it's in there. Concretely, I got output like this:

```diff
-    -:1:1: RUF901 [*] Hey this is a stable test rule with a safe fix.
-    -:1:1: RUF902 Hey this is a stable test rule with an unsafe fix.
+    [RUF901]: [*] Hey this is a stable test rule with a safe fix.
+
+    [RUF902]: Hey this is a stable test rule with an unsafe fix.
+
```

I think there was a filename, but maybe not. I'd need to figure that out too.

I don't think we're even passing anything on stdin in these tests, so the three-line code frame isn't even empty lines in the file, it's entirely synthetic.

Thanks for the tricky cases. They look similar to some of Andrew's other code I've run across in `ruff_db` so I kind of assume they're handled, but I'll double check!

---

_@MichaReiser reviewed on 2025-07-18 13:50_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bugbear/snapshots/ruff_linter__rules__flake8_bugbear__tests__B911_B911.py.snap`:52 on 2025-07-18 13:50_

I feel like the primary message looks very good in many cases but not all. 

For example here, I think the `help` text was better. I don't think this is something we need to change in rendering. Instead, we should change how the diagnostic is constructed in the rule. 

The airflow rules are similar. The inline texts are too long. 

---

_Comment by @BurntSushi on 2025-07-18 17:41_

I looked through my commit history here, but couldn't find anything directly related to file-level diagnostics. There were some changes related to tweaking ranges and even one phantom line terminator (although this came from our code and not `annotate-snippets`). The closest I could find were the following commits:

* 5021f3244909c0513ea434f1d7f29535b2cfca34
* 2ff2a54f5699b00b5b12aedefcdd3774b23b32d1
* c9b99e4bee32c0fce91cf97363e1b208c898d242
* 602a27c4e3861757e9d78257f49c58bb823d43b9
* 2ca2f73ba89498204db3c06fe434934249027045

---

_@MichaReiser reviewed on 2025-07-22 18:50_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bugbear/snapshots/ruff_linter__rules__flake8_bugbear__tests__B911_B911.py.snap`:52 on 2025-07-22 18:50_

I think this is resolved now

---

_Comment by @ntBre on 2025-07-22 23:10_

I think I'm getting pretty close to having the output we want, albeit with a potentially hacky implementation. For this input:

```python
import math

x: str = 1

match 42:
    case _: ...
```

and this shell command:

```shell
git diff <(ruff check --no-cache tr{i,y}.py --target-version py39) \
         <(myruff check --no-cache tr{i,y}.py --target-version py39)
```

I'm currently getting this diff:

```diff
-tri.py:1:1: E902 No such file or directory (os error 2)
-try.py:1:8: F401 [*] `math` imported but unused
+E902 No such file or directory (os error 2)
+
+F401 [*] `math` imported but unused
+ --> try.py:1:8
   |
 1 | import math
-  |        ^^^^ F401
+  |        ^^^^
 2 |
 3 | x: str = 1
   |
-  = help: Remove unused import: `math`
+help: Remove unused import: `math`
 
-try.py:5:1: SyntaxError: Cannot use `match` statement on Python 3.9 (syntax was added in Python 3.10)
+SyntaxError: Cannot use `match` statement on Python 3.9 (syntax was added in Python 3.10)
+ --> try.py:5:1
   |
 3 | x: str = 1
 4 |
```

This all looks good to me except the missing filename for the full-file diagnostic E902. I also tried a bit to get rid of the extra newline without much luck. I think the last two commits Andrew linked are in exactly the right place, but reverting to the version in [602a27c](https://github.com/astral-sh/ruff/commit/602a27c4e3861757e9d78257f49c58bb823d43b9) removes all the lines between diagnostics. I think I've convinced myself that it's actually nice to have the "extra" line for E902 since that's consistent with other diagnostics, but maybe I'm just giving up too easily.

Another issue with the check I added to remove the empty annotations is that some `TextRange::default` ranges are intentional, I think. For example, this snapshot changed:

https://github.com/astral-sh/ruff/blob/dc10ab81bd85b8af49ee2fe8ed4a6f767d37d6e0/crates/ty_ide/src/goto_declaration.rs#L289-L294

when I think it might want to point to the beginning of the file, with a real annotation. I002 is another example of a diagnostic that might actually want a default range and changed in my recent commits:

https://github.com/astral-sh/ruff/blob/dc10ab81bd85b8af49ee2fe8ed4a6f767d37d6e0/crates/ruff_linter/src/rules/isort/snapshots/ruff_linter__rules__isort__tests__required_import_with_alias_off.py.snap#L5-L8

I'll keep looking at these remaining issues tomorrow, but at least I made some progress on the brackets and blank lines (maybe) today.

---

_Comment by @MichaReiser on 2025-07-23 06:26_

> This all looks good to me except the missing filename for the full-file diagnostic E902. 

I think that actually looks fine. Have you tried how `full` renders diagnostics without an annotation? Does it add an empty line between them too (which I think would make sense)? Or is the extra line something that isn't visible in that diff?

Regarding the full range diagnostics. This is obviously something I'd like @BurntSushi's input on before changing but I wonder if the problem here really is that the primary annotation range  does:

1. Set the span for the line/column rendering and where the diagnostic can be suppressed
2. Enable code span rendering, even if the message is empty (very common)

Ideally, the diagnostic model would allow a rule to set a span for 1 but without enabling 2. 

The first idea that comes to mind is to add a span to `Diagnostic` itself. I believe the issue is that the file name rendering is tied to the annotation. Which makes sense, because different annotations could point to different files. That means we would only render the file name if no annotations are present. However, this opens us up to the risk that the `primary_annotation` and the `diagnostic.range `could point to different files, in which case omitting the diagnostic range's file name could be confusing. But that's an invariant that we could enforce with assertions. 

An alternative is to mark a primary annotation to say, nope, only render the file name but no message or annotation. This does seem a bit weird given that that's exactly what an annotation is for but again, something we could enforce with an assertion somewhere.

I think the biggest challenge here might is probably that annotation snippet doesn't allow rendering the file name without a "snippet". We could probably work around this by simply copy pasting the file name rendering. 


What's unclear to me from a UX perspective if the following is confusing:

```
E902 No such file or directory (os error 2)
--> try.py:1:8

F401 [*] `math` imported but unused
--> try.py:1:8
  |
1 | import math
  |        ^^^^
2 |
3 | x: str = 1
  |
help: Remove unused import: `math`
```


---

_Comment by @ntBre on 2025-07-23 13:42_

> Have you tried how `full` renders diagnostics without an annotation? Does it add an empty line between them too (which I think would make sense)? Or is the extra line something that isn't visible in that diff?

What do you mean without an annotation? I think in a literal, but maybe unhelpful, sense all Ruff diagnostics have annotations. Here's the full output of the ruff command above with a couple of extra non-existent files, if that helps:

```
E902 No such file or directory (os error 2)

E902 No such file or directory (os error 2)

E902 No such file or directory (os error 2)

F401 [*] `math` imported but unused
 --> /tmp/tmp.gqRJ0jMRZ9/try.py:1:8
  |
1 | import math
  |        ^^^^
2 |
3 | x: str = 1
  |
help: Remove unused import: `math`

SyntaxError: Cannot use `match` statement on Python 3.9 (syntax was added in Python 3.10)
 --> /tmp/tmp.gqRJ0jMRZ9/try.py:5:1
  |
3 | x: str = 1
4 |
5 | match 42:
  | ^^^^^
6 |     case _: ...
  |

Found 5 errors.
[*] 1 fixable with the `--fix` option.
```

> Regarding the full range diagnostics. This is obviously something I'd like @BurntSushi's input on before changing but I wonder if the problem here really is that the primary annotation range does:
> 
> 1. Set the span for the line/column rendering and where the diagnostic can be suppressed
> 2. Enable code span rendering, even if the message is empty (very common)
> 
> Ideally, the diagnostic model would allow a rule to set a span for 1 but without enabling 2.

I think that's a very good summary of the problem. And thanks for the other ideas, those seem helpful to explore.

> What's unclear to me from a UX perspective if the following is confusing:
> 
> ```
> E902 No such file or directory (os error 2)
> --> try.py:1:8
> 
> F401 [*] `math` imported but unused
> --> try.py:1:8
>   |
> 1 | import math
>   |        ^^^^
> 2 |
> 3 | x: str = 1
>   |
> help: Remove unused import: `math`
> ```

Yeah, I do like Ruff's current rendering here, basically collapsing back to concise rendering for E902:

```
tri.py:1:1: E902 No such file or directory (os error 2)
try.py:1:8: F401 [*] `math` imported but unused
  |
1 | import math
  |        ^^^^ F401
2 |
3 | x: str = 1
  |
  = help: Remove unused import: `math`
```

But that's also consistent with its full output. I'm not sure if mixing the two is confusing too:

```
tri.py:1:1: E902 No such file or directory (os error 2)
F401 [*] `math` imported but unused
 --> try.py:1:8
  |
1 | import math
  |        ^^^^
2 |
3 | x: str = 1
  |
help: Remove unused import: `math`
```

Ah, it looks like rustc just embeds the filename in the error message when there's no annotation:

```
$ rustc fake.rs
error: couldn't read fake.rs: No such file or directory (os error 2)

error: aborting due to 1 previous error
```

But then we'd have to modify the message on `E902` and any other rule like it.

---

_Comment by @BurntSushi on 2025-07-23 14:35_

I think adding a knob on to `Annotation` that lets you suppress code snippets is probably the better choice. This "feels" better to me than trying to put a range on the diagnostic itself, which I think could be quite confusing. (Micha noted this.) While true we could enforce this with assertions, it feels like a more complicated change to the diagnostic model than just letting an annotation be "light" in the sense that there are no code snippets.

I do agree that this makes the name "annotation" somewhat of a misnomer in certain cases. Although, you could recast as, "it's still an annotation, but it applies to contents that are too large to usefully display in an error message."

---

_Comment by @ntBre on 2025-07-23 17:30_

We've got filenames!

```diff
-tri.py:1:1: E902 No such file or directory (os error 2)
-try.py:1:8: F401 [*] `math` imported but unused
+E902 No such file or directory (os error 2)
+--> tri.py:1:1
+
+F401 [*] `math` imported but unused
+ --> try.py:1:8
   |
 1 | import math
-  |        ^^^^ F401
+  |        ^^^^
 2 |
 3 | x: str = 1
   |
-  = help: Remove unused import: `math`
+help: Remove unused import: `math`

-try.py:5:1: SyntaxError: Cannot use `match` statement on Python 3.9 (syntax was added in Python 3.10)
+SyntaxError: Cannot use `match` statement on Python 3.9 (syntax was added in Python 3.10)
+ --> try.py:5:1
   |
 3 | x: str = 1
 4 |
```

Plumbing this setting through the `Annotation`s felt like a nice solution, thank you both for that suggestion! Now we can also set this only in Ruff and avoid the ty snapshot changes I was worried about yesterday. I didn't do this now, but we could also use this on a per-diagnostic basis in Ruff for those rules that happen to point to the start of the file but still want a snippet rendered, unlike E902.

I think the diagnostic output is where I want it now. I'll look over the implementation again and hopefully open it for review later today!

---

_Comment by @ntBre on 2025-07-23 21:43_

I think the implementation looks okay. It's mostly just passing flags (that might need better names!) into annotate-snippets and some touchy formatting changes based on them.

I also looked closely at the tricky cases Micha mentioned above. The cut indicator ellipsis was easy thanks to #19420, I just had to copy that over to the new file. That also reverted the remaining snapshot changes in ty crates.

For the `SourceCode` case, I was able to copy over the code from `ruff_linter` and integrate it pretty smoothly. ~~We can almost delete the version I copied from, getting back down to one copy, but that code is still used by the `grouped` output format. I guess we could at least export `ceil_char_boundary` from `ruff_db` if we want to avoid the duplication. Hopefully it will get stabilized one day and we can delete both copies, though.~~ This duplication was kind of forcefully resolved by it needing to be `pub` for the doctests.

I'm a bit stuck on where to inject the `replace_whitespace_and_unprintable` call. We definitely need it because the snapshot range fixed in e6e610c274 has regressed. The `DiagnosticSource` makes it trickier to access and modify the source text directly, at least in `ResolvedAnnotation::new` where I added the `ceil_char_boundary` stuff. It might make more sense to add both checks in `RenderableSnippet::new`, at least that's the other candidate that jumped out to me. I'll keep thinking about this tonight.

All of the code changes except the line terminator fix in the last commit are in [this range](https://github.com/astral-sh/ruff/pull/19415/files/e787f975c0a93b6036a97783cf16682eca8a0762). I'll update the summary before marking this ready for review.


---

_Renamed from "[prototype] Move full diagnostic rendering to `ruff_db`" to "Move full diagnostic rendering to `ruff_db`" by @ntBre on 2025-07-23 21:58_

---

_Comment by @MichaReiser on 2025-07-24 05:21_

Would it make sense to split this pr into 1) extending the rendering and b) migrating ruff?

Or can you organize the commits in a way that makes reviewing easier

---

_Comment by @ntBre on 2025-07-30 16:26_

This is up to date again, at least until we merge any other rule PRs, and I squashed the commits into a more linear history. I think I can still split more of them off into standalone PRs, though.

---

_Comment by @ntBre on 2025-07-31 21:33_

This is now based on #19653 and contains very few code changes, even fewer if I didn't split out the `FullRenderer`.

I just noticed that we aren't handling Jupyter notebook cell offsets like Ruff's old format or in the new `concise` format. So I guess I'll be splitting off another PR trying to get that information into `annotate-snippets` too. It seems like it could be a pretty invasive change without looking into it much yet.

---

_Comment by @ntBre on 2025-08-06 14:56_

This is very nearly ready for review, but I asked Claude to take a look and it found a few additional discrepancies. As it was glad to point out, this affects a very small number of snapshots (8/12,377 or 0.06%), but it might still be worth trying to fix these.

### EOF handling

It looks like `annotate-snippets` doesn't like our EOF ranges. These used to report a range of 30:1, the first character of the non-existent 30th line in this file), but now fall back to a default range of 1:1:

https://github.com/astral-sh/ruff/blob/4d57fcd5a488e628aa26aaacd0a61728609ff310/crates/ruff_linter/src/rules/flake8_implicit_str_concat/snapshots/ruff_linter__rules__flake8_implicit_str_concat__tests__ISC001_ISC_syntax_error.py.snap#L186-L200

We could probably fix that in our own handling of these syntax errors or maybe in `annotate-snippets`.

### BOM handling

Claude called this an improvement, but I'm a bit skeptical. We now report 1:2 here instead of 1:1:

https://github.com/astral-sh/ruff/blob/4d57fcd5a488e628aa26aaacd0a61728609ff310/crates/ruff_linter/src/rules/isort/snapshots/ruff_linter__rules__isort__tests__bom_unsorted.py.snap#L4-L11

### Carriage return handling

It seems we're no longer counting carriage returns as line breaks. This case is now reported as 1:40 instead of 2:1:

https://github.com/astral-sh/ruff/blob/4d57fcd5a488e628aa26aaacd0a61728609ff310/crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP018_CR.py.snap#L4-L10

This one kind of makes sense to me, but I can see the argument against it as well.

---

_Comment by @MichaReiser on 2025-08-06 16:34_

> It seems we're no longer counting carriage returns as line breaks. This case is now reported as 1:40 instead of 2:1:

It's certainly unfortunate because it's a valid line terminator in Python. But I doubt that it's wildly used and it might be somewhat annoying to fix (unless we patch `\r` to `\n` in the source code normalization before passing it to annotated snippets)

> Claude called this an improvement, but I'm a bit skeptical. We now report 1:2 here instead of 1:1:

Zed does render the BOM character and also reports 1:2 for the `import`. So I guess it's not entirely unreasonable. I'm fine deferring this for now (or stripping the BOM as part of our source code normalization).


> It looks like annotate-snippets doesn't like our EOF ranges. These used to report a range of 30:1, the first character of the non-existent 30th line in this file), but now fall back to a default range of 1:1:

What do we use as range? Is it `TextRange::at(source_text.text_len(), 0)` or is it one beyond? If it's one beyond, thank I think the right thing is to fix the range. 

I think we need to fix this in some way that it doesn't point at the start of the file. It seems like this is only a bug in the code that determines the line number as the carret rendering seems correct.

---

_Comment by @ntBre on 2025-08-06 20:14_

> It's certainly unfortunate because it's a valid line terminator in Python. But I doubt that it's wildly used and it might be somewhat annoying to fix (unless we patch `\r` to `\n` in the source code normalization before passing it to annotated snippets)

I think this normalization is a very good idea. It looks like the snapshots are actually quite misleading in this case. This is how the check renders for me locally in the CLI on 0.12.7:

```
> ruff check --no-cache --select UP018 UP018_CR.py
UP018_CR.py:2:1: UP018 [*] Unnecessary `int` call (rewrite as a literal)
  |
    1)Keep parenthesis around preserved CR
  |                                       ^^^^^^^^^^^ UP018
  |
  = help: Replace with integer literal
```

versus the version after a simple normalization in this PR:

```
> myruff check --no-cache --select UP018 UP018_CR.py
UP018 [*] Unnecessary `int` call (rewrite as a literal)
 --> UP018_CR.py:2:1
  |
1 |   # Keep parenthesis around preserved CR
2 | / int(-
3 | |     1)
  | |______^
4 |   int(+
5 |       1)
  |
help: Replace with integer literal
```

That seems to be the case for the other two snapshots affected by my normalization in too.

> Zed does render the BOM character and also reports 1:2 for the `import`. So I guess it's not entirely unreasonable. I'm fine deferring this for now (or stripping the BOM as part of our source code normalization).

This normalization was also easy to add. It affected one other BOM test snapshot, but not really in a visible way since the BOM is basically unprintable anyway.

> What do we use as range? Is it `TextRange::at(source_text.text_len(), 0)` or is it one beyond? If it's one beyond, thank I think the right thing is to fix the range.
> 
> I think we need to fix this in some way that it doesn't point at the start of the file. It seems like this is only a bug in the code that determines the line number as the carret rendering seems correct.

Yeah, it's `TextRange::at(source_text.text_len(), 0)`. For this test, the `source.text_len()` is 481 in `Lexer::new`, and the resulting range is 481..481. So it sounds like we need to fix this in `annotate-snippets` rather than the lexer. I'll take a look at that next.

---

_Comment by @ntBre on 2025-08-07 01:57_

(I'll split these off into a separate PR again too, this was just the easiest way to test them first)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/text.rs`:93 on 2025-08-08 16:02_

If you haven't done so already. Can you double check if the diff rendering still looks good?

---

_@MichaReiser approved on 2025-08-08 16:06_

I didn't review all snapshot changes but this looks great. Thank you for patiently rebasing this PR and splitting out your fixes into smaller PRs!

---

_Comment by @ntBre on 2025-08-08 16:06_

This should be ready for review. I left the commits towards the end named as if they resolve the carriage return, BOM, and EOF issues, but after https://github.com/astral-sh/ruff/pull/19806 they just accept the snapshots from the actual code changes in the other PR.

I also wrote a much shorter AWK version of Claude's script for checking the offsets in the diff. It has three false positives, but I verified that they are just due to git displaying a larger diff in those areas, not real differences in `row:column` reporting.

<details><summary>Script and output</summary>
<p>

```awk
#!/usr/bin/awk -f

{
    # Old line like:
    # -    -:1:8: F401 [*] `os` imported but unused
    if (match($0, /-.*:([0-9]+):([0-9]+):/, m)) {
        row = m[1]
        col = m[2]
        line = $0
    }
    # New line like:
    # +    F401 [*] `os` imported but unused
    else if (match($0, /+.*-->.*:([0-9]+):([0-9]+)/, m)) {
        new_row = m[1]
        new_col = m[2]
        if (new_row != row || new_col != col) {
            print "MISMATCH:"
            printf "\tOld line: %s\n", line
            printf "\tNew line: %s\n", $0
        }
    }
}
```

These are false positives due to how git reports the diff for these files.

```shell
$ git diff main | ./check_offsets.awk
MISMATCH:
        Old line: -E501_1.py:5:89: E501 Line too long (150 > 88)
        New line: + --> E501_1.py:4:89
MISMATCH:
        Old line: -E501_1.py:6:89: E501 Line too long (149 > 88)
        New line: + --> E501_1.py:5:89
MISMATCH:
        Old line: -all.py:1:5: D103 Missing docstring in public function
        New line: +--> all.py:1:1
```

For example:

```diff
-all.py:1:1: D100 Missing docstring in public module
-all.py:1:5: D103 Missing docstring in public function
+D100 Missing docstring in public module
+--> all.py:1:1
+
+D103 Missing docstring in public function
+ --> all.py:1:5
```

The E501 case is similar. Basically all of the E501 diagnostics for the file are jumbled together in the diff because the long-line truncation changes too, but all of the individual diagnostics are still there and reported with the correct line and column numbers.

</p>
</details> 

---

_Marked ready for review by @ntBre on 2025-08-08 16:06_

---

_Review requested from @carljm by @ntBre on 2025-08-08 16:06_

---

_Review requested from @sharkdp by @ntBre on 2025-08-08 16:06_

---

_Review requested from @dcreager by @ntBre on 2025-08-08 16:06_

---

_Review requested from @AlexWaygood by @ntBre on 2025-08-08 16:06_

---

_Review requested from @BurntSushi by @ntBre on 2025-08-08 16:06_

---

_Review request for @dcreager removed by @ntBre on 2025-08-08 16:07_

---

_Review request for @carljm removed by @ntBre on 2025-08-08 16:07_

---

_Review request for @sharkdp removed by @ntBre on 2025-08-08 16:07_

---

_Review request for @AlexWaygood removed by @ntBre on 2025-08-08 16:07_

---

_Review comment by @ntBre on `crates/ruff_linter/src/message/text.rs`:93 on 2025-08-08 16:13_

This is the diff rendering that only works in the snapshots (https://github.com/astral-sh/ruff/issues/7352). I didn't notice any changes in those sections of the snapshots, as expected, but is there something else you want me to check here?

---

_@ntBre reviewed on 2025-08-08 16:13_

---

_@MichaReiser reviewed on 2025-08-08 16:17_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/text.rs`:93 on 2025-08-08 16:17_

Oh, I thought it's the CLI's --diff mode. Also, is there any existing code that we can delete, now that we moved the diagnostic rendering (e.g. `MessageCodeFrame`)?

---

_@ntBre reviewed on 2025-08-08 16:19_

---

_Review comment by @ntBre on `crates/ruff_linter/src/message/text.rs`:93 on 2025-08-08 16:19_

No the `--diff` flag is separate and looks good:

```shell
> ruff check try.py --diff
--- try.py
+++ try.py
@@ -1 +0,0 @@
-import math

Would fix 1 error.
> myruff check try.py --diff
--- try.py
+++ try.py
@@ -1 +0,0 @@
-import math

Would fix 1 error.
```

We can't quite delete the `TextEmitter` and the `MessageCodeFrame` rendering because it's still used by the `grouped` format. Once we move that over, we can delete a big chunk of code.

---

_@MichaReiser reviewed on 2025-08-08 16:25_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/text.rs`:93 on 2025-08-08 16:25_

It seems that `GroupEmitter::with_show_source` is only called in tests. So we could just delete it? But I also don't mind if you do this in a separate PR

---

_@ntBre reviewed on 2025-08-08 16:28_

---

_Review comment by @ntBre on `crates/ruff_linter/src/message/text.rs`:93 on 2025-08-08 16:28_

Oh you're right, I forgot about that one. I'll check again for unused code and follow up!

---

_@MichaReiser reviewed on 2025-08-08 16:31_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/text.rs`:93 on 2025-08-08 16:31_

Sounds good. I'm fine merging this PR and follow up in a separate PR. 

---

_Merged by @ntBre on 2025-08-08 16:56_

---

_Closed by @ntBre on 2025-08-08 16:56_

---

_Branch deleted on 2025-08-08 16:56_

---

_@BurntSushi approved on 2025-08-08 16:58_

LGTM!

---
