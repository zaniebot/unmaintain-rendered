```yaml
number: 15359
title: "[`ruff`] update and vendor annotate-snippets"
type: pull_request
state: merged
author: BurntSushi
labels:
  - internal
  - diagnostics
assignees: []
merged: true
base: main
head: ag/upgrade-annotate-snippets
created_at: 2025-01-08T20:23:15Z
updated_at: 2025-01-15T18:37:55Z
url: https://github.com/astral-sh/ruff/pull/15359
synced_at: 2026-01-10T20:34:00Z
```

# [`ruff`] update and vendor annotate-snippets

---

_Pull request opened by @BurntSushi on 2025-01-08 20:23_

This PR upgrades to the latest version of
[`annotate-snippets`](https://docs.rs/annotate-snippets)
and vendors it. The motivating reason for vendoring the
crate is [because it makes a non-elidable change to the
diagnostic header][annotate-snippets-header]. (Note that
I've [submitted a PR](https://github.com/rust-lang/annotate-snippets-rs/pull/171)
to help us work-around that issue, but it might not be
a good fit for upstream.) Moreover,
while `annotate-snippets` is dense, it's not very big. And
I think it makes sense for us to maintain our own copy,
at least for now, so that we can be masters of our own
destiny over a critical component of Ruff.

For example, our vendored copy now contains two bug fixes
to `annotate-snippets` that I sent to upstream, but we
can benefit from the fixes now. See https://github.com/rust-lang/annotate-snippets-rs/pull/169 
and https://github.com/rust-lang/annotate-snippets-rs/pull/170.

Reviewers should look at this PR commit-by-commit. The
unusually large number of commits is a result of dividing
the snapshot changes (over 1,000 of them) into different
categories. I did this so that I could more easily review
the changes and ensure they are correct and/or desirable.
In some cases, they are bug fixes. In other cases, they
are just stylistic improvements. In yet other cases, they
flagged bugs in `annotate-snippets` themselves. While the
number of commits is large, I actually believe this will
make review much easier and quicker.

Closes #10832, Ref #11618

[annotate-snippets-header]: https://github.com/rust-lang/annotate-snippets-rs/issues/167


---

_Review requested from @AlexWaygood by @BurntSushi on 2025-01-08 20:23_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-01-08 20:23_

---

_Review requested from @dhruvmanila by @BurntSushi on 2025-01-08 20:23_

---

_Comment by @BurntSushi on 2025-01-08 20:25_

Bah, a bunch of Clippy lints are firing on the vendored copy of `annotate-snippets`. I'm considering just squashing all of them in order to reduce the diff with upstream, but that might be a non-goal.

EDIT: This is what I ended up doing.

---

_Comment by @github-actions[bot] on 2025-01-08 20:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

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

_Review comment by @Gankra on `crates/ruff_linter/src/message/text.rs`:343 on 2025-01-08 21:19_

This expect is very funny to me (because it doesn't trip on 5, it trips on like... 5 billion)

---

_Review comment by @Gankra on `crates/ruff_linter/src/message/text.rs`:313 on 2025-01-08 21:25_

This function could use a comment explaining what it does or a more descriptive name. (I have been staring at it for a while trying to make sense of it and I can't quite, and I'm not sure if that's because I see a bug or misunderstand what it's doing)

---

_Review comment by @Gankra on `crates/ruff_linter/src/message/text.rs`:306 on 2025-01-08 21:27_

is this actually commented out code that got missed, or just implying a theoretical macro to gesture to what this function is doing (if the latter, seems like too much detail)

---

_Review comment by @Gankra on `crates/ruff_linter/src/message/text.rs`:323 on 2025-01-08 21:29_

typo "sisze"

---

_@Gankra reviewed on 2025-01-08 21:31_

---

_Comment by @Gankra on 2025-01-08 21:36_

partial review status: i've "approved" all the commits that aren't `test:`

---

_Label `do-not-merge` added by @MichaReiser on 2025-01-09 09:12_

---

_Comment by @MichaReiser on 2025-01-09 09:12_

Adding "do-not-merge" until the 0.9 release is out

---

_@dhruvmanila reviewed on 2025-01-09 09:28_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@del_incomplete_target.py.snap`:125 on 2025-01-09 09:28_

Yes! Thank you! This has been annoying for a while, happy to see this fixed.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/fixtures.rs`:215 on 2025-01-09 11:02_

Nice, you removed the useless colored check!

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/text.rs`:343 on 2025-01-09 12:01_

You can avoid the `u32::try` from here by using `char::text_len` (char implements the `TextLen` trait). It also better encodes that `len` is a byte offset rather than any `u32`. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/snapshots/ruff_linter__rules__pycodestyle__tests__preview__W391_W391_2.py.snap`:12 on 2025-01-09 12:26_

This looks somewhat funny because we don't render new lines. I don't think it's something we have to address as part of this PR but might be good to be aware of

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_comprehensions/snapshots/ruff_linter__rules__flake8_comprehensions__tests__C419_C419.py.snap`:105 on 2025-01-09 12:29_

I find the new style less distracting but it's slightly less clear where the range starts. But I'd leave it as is because I don't consider it a big deal.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/snapshots/ruff_linter__rules__pycodestyle__tests__E112_E11.py.snap`:8 on 2025-01-09 12:36_

Hmm, not sure if I like this change. It's not incorrect but it's probably a bit misleading because the indent needs to be inserted at the next line: Adding a tab at the end of this line is useless. The same applies for the other E* rules. 

It seems we already special case indents. Maybe this is no longer necessary?

https://github.com/astral-sh/ruff/blob/9f3a38d408f473df7a6b3574c5d1389bef303dd5/crates/ruff_linter/src/checkers/logical_lines.rs#L161-L165

---

_Renamed from "update and vendor annotate-snippets" to "[`ruff`] update and vendor annotate-snippets" by @BurntSushi on 2025-01-09 12:37_

---

_@BurntSushi reviewed on 2025-01-09 12:40_

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/message/text.rs`:306 on 2025-01-09 12:40_

Nah just a snippet I copied from https://github.com/astral-sh/ruff/blob/beb8e2dfe07b71de5507fbff151e510fb284747a/crates/ruff_linter/src/text_helpers.rs

(That was what was originally being used to remove unprintable chars. Other code is still using it.)

I meant to remove this. Nice catch.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydocstyle/snapshots/ruff_linter__rules__pydocstyle__tests__D207_D.py.snap`:8 on 2025-01-09 12:40_

Hmm, this also seems off but it's possible that it only used to work out of poor luck? I think the idea here is that the violation is at the begining of `Description` because whitespace only lines should be ignored. 

The relevant line seems to be 

https://github.com/astral-sh/ruff/blob/5bc9d6d3aa694ab13f38dd5cf91b713fd3844380/crates/ruff_linter/src/rules/pydocstyle/rules/indent.rs#L226-L227

Adding the `line.start()` seems correct. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydocstyle/snapshots/ruff_linter__rules__pydocstyle__tests__D208_D.py.snap`:8 on 2025-01-09 12:41_

Same here. I think the diagnostic should be at the beginning of the `Description` line

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/snapshots/ruff_linter__rules__pycodestyle__tests__E112_E11.py.snap`:1 on 2025-01-09 12:43_

Looking at this commit. I think that all changes here are undesired other than for I001. We should double check what the ranges are but most of the rules try to render a marker right at the start of a line with an empty range. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/isort/snapshots/ruff_linter__rules__isort__tests__lines_after_imports.pyi.snap`:18 on 2025-01-09 12:44_

I agree that this is better but it's far from nice :D 

Would you mind opening an issue to look into the I001 ranges. I think we should trim them to the imports range. 

---

_@BurntSushi reviewed on 2025-01-09 12:45_

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/message/text.rs`:313 on 2025-01-09 12:45_

Yes! I added this:

```rust
    // Updates the range given by the caller whenever a single byte (at
    // `index` in `source`) is replaced with `len` bytes.
    //
    // When the index occurs before the start of the range, the range is
    // offset by `len`. When the range occurs after or at the start but before
    // the end, then the end of the range only is offset by `len`.
    let mut update_range = |index, len| {
        if index < usize::from(annotation_range.start()) {
            range += TextSize::new(len - 1);
        } else if index < usize::from(annotation_range.end()) {
            range = range.add_end(TextSize::new(len - 1));
        }
    };
```

(The actual logic here is unchanged. I just moved it to a helper function so I could reuse it.)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/isort/snapshots/ruff_linter__rules__isort__tests__skip.py.snap`:56 on 2025-01-09 12:47_

Agree, this seems correct and is more a problem with `I001` then the rendering (unless `I001` generates the correct range

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/snapshots/ruff_linter__rules__pycodestyle__tests__E204_E204.py.snap`:48 on 2025-01-09 12:47_

This is great!

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/snapshots/ruff_linter__rules__flake8_pyi__tests__PYI003_PYI003.pyi.snap`:7 on 2025-01-09 12:50_

Hmm, using `...` is slightly problematic because it's a valid syntax element 😆 Any chance we can customize the trimming character, although I'm not sure what character to recommend: Is there a unicode equivalent of `...`?

---

_@MichaReiser reviewed on 2025-01-09 12:53_

This is great. Thank you and thanks a ton for splitting the commits as you did. It allowed me to skip over a large chunk of the snapshot changes because I knew they're all whitespace (and github's UI is pain because it requires clicking load changes for many of them). 

I think there are a few cases where the snapshots now are incorrect and may require changes to the rule or rendering. The truncation itself is nice, but I think we can't use `...` because it's a valid syntax element in Python.

---

_@BurntSushi reviewed on 2025-01-09 13:13_

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/flake8_pyi/snapshots/ruff_linter__rules__flake8_pyi__tests__PYI003_PYI003.pyi.snap`:7 on 2025-01-09 13:13_

Yeah, `…` is `U+2026 HORIZONTAL ELLIPSIS`. Are you fine with that even though it renders very similarly to `...` (three separate periods)? Or do you want something entirely different?

But yes, we can change it. I'll do it in a way where I add a new API to `annotate-snippets` so that I can push the patch upstream too. (The `...` is currently hard-coded into `annotate-snippets`, so the simplest would be to just change what is hard-coded.)

---

_@BurntSushi reviewed on 2025-01-09 13:17_

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/pycodestyle/snapshots/ruff_linter__rules__pycodestyle__tests__E112_E11.py.snap`:8 on 2025-01-09 13:17_

Hmmmm yes I see. I'll look into this. I could see this being tricky because I think part of the `annotate-snippets` fixes was rendering a span immediately _after_ the line terminator to be at the end of the line it terminates instead of at the start of the next line. And it seems like this is what led to at least some portion of the fixes here. But I think this ends up being wrong for cases where we actually want the annotation pointing to the beginning of a line.

Like, doesn't my description of how `annotate-snippets` works with a line terminator imply that you can _never_ point to the beginning of a line? If so, then in theory the right fix there is to change `annotate-snippets` to render a span immediately after a line terminator to point to the start of the following line, and then any lints that want to point to the end of a line need to put the span just _before_ the line terminator. If so, that's probably a pretty gnarly fix, but let me dig into it and see if my description here is actually accurate.

---

_@Gankra reviewed on 2025-01-09 13:38_

---

_Review comment by @Gankra on `crates/ruff_linter/src/message/text.rs`:313 on 2025-01-09 13:38_

Ohh ok that's why this was so baffling. In spite of the clear text of the match I couldn't get it out of my head that "obviously" we're replacing "weird unicodey characters" with "plain ascii characters" and therefore this code should be replacing `len` bytes with 1 byte (which it clearly isn't doing).

---

_Comment by @sharkdp on 2025-01-09 13:48_

> and github's UI is pain because it requires clicking load changes for many of them

@MichaReiser I have a "Load diffs" bookmark in my browsers toolbar which does this automatically :smile:
```js
javascript:document.querySelectorAll('.load-diff-button').forEach(node => node.click())
```

---

_@MichaReiser reviewed on 2025-01-09 13:49_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/snapshots/ruff_linter__rules__flake8_pyi__tests__PYI003_PYI003.pyi.snap`:7 on 2025-01-09 13:49_

I think I'm fine with that. It's different enough to avoid confusion. It might still be slightly confusing if it happens to be right at the end of a clause header. We can also iterate on this based on user feedback.

---

_Label `do-not-merge` removed by @AlexWaygood on 2025-01-09 14:47_

---

_Comment by @BurntSushi on 2025-01-09 18:53_

I've updated the PR with some rewritten history and addressed a good portion of the feedback so far. What's remaining is some of the thornier problems with some of the new rendering. My plan is to investigate those, figure out their root causes and figure out how to fix them. I don't think a re-review is helpful yet. I'll ping y'all when a re-review makes more sense.

---

_@BurntSushi reviewed on 2025-01-14 19:04_

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/pycodestyle/snapshots/ruff_linter__rules__pycodestyle__tests__E112_E11.py.snap`:1 on 2025-01-14 19:04_

These are unfortunately turning out to be extremely difficult to fix. I was able to fix the E112 regressions by tweaking the spans generated by that specific lint, but some of the other non-I001 changes in this commit are harder to fix. For example, the syntax errors seem slightly off. The issue is that the spans are generated by the Python parser, and I'm not sure I want to dig into tweaking those. So if I change `annotate-snippets` to render the position after the line terminator to be at the start of the following line instead of the end of the preceding line, it ends up regressing a bunch of the other bug fixes in this PR.

I haven't looked at the D207 errors yet, but I suspect I'll be able to fix that by tweaking the spans like I did for E112.

So I think that overall leaves a few of the syntax error diagnostics (it doesn't appear to be many) that might wind up a little more confusing than the status quo, but still not outright wrong I think. Thoughts on me filing an issue for that but otherwise moving on?

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/pydocstyle/snapshots/ruff_linter__rules__pydocstyle__tests__D207_D.py.snap`:8 on 2025-01-14 19:15_

The offsets reported by the lint here do indeed appear correct (note the assert):

```rust
use annotate_snippets::{Level, Renderer, Snippet};

fn main() {
    let source = "    \"\"\"Summary.\n\nDescription.\n\n    \"\"\"\n";
    assert_eq!(&source[17..], "Description.\n\n    \"\"\"\n");
    let src_annotation = Level::Error.span(17..17).label("D207");
    let snippet =
        Snippet::source(source).line_start(230).annotation(src_annotation);
    let message = Level::Error.title("").snippet(snippet);

    println!("{}", Renderer::plain().render(message));
}
```

But the output of the above is:

```
error
    |
230 |     """Summary.
231 |
    | ^ D207
232 | Description.
233 |
234 |     """
    |
```

So this looks like a legit bug in `annotate-snippets`.

---

_@BurntSushi reviewed on 2025-01-14 19:15_

---

_@BurntSushi reviewed on 2025-01-14 19:19_

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/pydocstyle/snapshots/ruff_linter__rules__pydocstyle__tests__D207_D.py.snap`:8 on 2025-01-14 19:19_

Filed upstream: https://github.com/rust-lang/annotate-snippets-rs/issues/177

I'll take a look at fixing this myself.

---

_Label `internal` added by @dhruvmanila on 2025-01-15 05:16_

---

_Label `diagnostics` added by @dhruvmanila on 2025-01-15 05:16_

---

_@BurntSushi reviewed on 2025-01-15 15:20_

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/pydocstyle/snapshots/ruff_linter__rules__pydocstyle__tests__D207_D.py.snap`:8 on 2025-01-15 15:20_

OK, upstream bug was an illusion. Lol. I've fixed this by tweaking the spans produced by the D207 and D208 lints.

---

_@BurntSushi reviewed on 2025-01-15 15:21_

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/pycodestyle/snapshots/ruff_linter__rules__pycodestyle__tests__E112_E11.py.snap`:8 on 2025-01-15 15:21_

I fixed this by tweaking the spans produced by the E112 lint.

---

_@BurntSushi reviewed on 2025-01-15 15:35_

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/isort/snapshots/ruff_linter__rules__isort__tests__lines_after_imports.pyi.snap`:18 on 2025-01-15 15:35_

Done! I did a bit of a detailed write up and tagged it as "help wanted" since I think it's a good candidate for an outside contribution. https://github.com/astral-sh/ruff/issues/15504

---

_Comment by @BurntSushi on 2025-01-15 15:40_

OK, I think this should be ready for re-review!

---

_Review requested from @MichaReiser by @BurntSushi on 2025-01-15 15:40_

---

_Review comment by @MichaReiser on `crates/ruff_annotate_snippets/src/renderer/display_list.rs`:371 on 2025-01-15 16:03_

This looked fishy because the `offset` naming suggests that we operate on byte offsets here but I think this is correct (I also agree that we shouldn't do any renames just to keep the diff with upstream small).

---

_Review comment by @MichaReiser on `crates/ruff_annotate_snippets/src/renderer/display_list.rs`:1120 on 2025-01-15 16:04_

Huh, surprising that `&'static` impacts the `'_` lifetime.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/snapshots/ruff_linter__rules__flake8_pyi__tests__PYI003_PYI003.pyi.snap`:7 on 2025-01-15 16:05_

Thanks, this is much better!

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@try_stmt_misspelled_except.py.snap`:200 on 2025-01-15 16:14_

Hmm. I guess this is another case where the offset used to be at the start of the line and the caret now ends up at the end of the previous line. Unfortunately, this will be harder to fix, and I don't think we have tests for every position where we expect a node. 



---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydocstyle/snapshots/ruff_linter__rules__pydocstyle__tests__D208_D.py.snap`:112 on 2025-01-15 16:16_

This range seems nonsensical. It should probably highlight the over-indented part but that's an improvement for another day.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@statements__if_extra_closing_parentheses.py.snap`:79 on 2025-01-15 16:19_

I only noticed those now. They should probably point at the start of the next line. 

---

_@MichaReiser approved on 2025-01-15 16:28_

This is great. It's unfortunate that we have to adjust the ranges for some lint rules, but I think it's the right trade-off. Would you mind opening an issue to track the "shortcoming" so that we don't forget about it? 

A few changes to parser diagnostics might be due to the same bug. Although I'm not 100% certain, it's definitely possible that the parser emits the wrong ranges here. 

The relevant code (just for reference) is:

https://github.com/astral-sh/ruff/blob/96da136e6af9791e574219099412393485f6ceca/crates/ruff_python_parser/src/parser/mod.rs#L489-L492

I don't suggest you change the emitted range. I'd be okay accepting this regression for now. But we may want to double check if it's the parser's or or rendering fault. 

One thing I want to quickly test is if the changed ranges impact the VS code extension in anyway because it uses the same ranges as the generated diagnostics.

---

_@BurntSushi reviewed on 2025-01-15 16:34_

---

_Review comment by @BurntSushi on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@statements__if_extra_closing_parentheses.py.snap`:79 on 2025-01-15 16:34_

Yeah this was what I mentioned in one of my other comments. I tried fixing this, but it's hard. I would either need to change the error offsets produced by the parser (which I'm not sure I really want to touch) or I'd need to fix `annotate-snippets` to render empty spans immediately after a line terminator to point at the beginning of the  following line instead of the end of the preceding line.

---

_@BurntSushi reviewed on 2025-01-15 16:34_

---

_Review comment by @BurntSushi on `crates/ruff_annotate_snippets/src/renderer/display_list.rs`:1120 on 2025-01-15 16:34_

Yeah I was a little surprised too.

---

_Review comment by @BurntSushi on `crates/ruff_annotate_snippets/src/renderer/display_list.rs`:371 on 2025-01-15 16:35_

Yes. Agreed. Extremely fishy.

---

_@BurntSushi reviewed on 2025-01-15 16:35_

---

_Comment by @MichaReiser on 2025-01-15 16:37_

Okay, somewhat "bad" news. The new diagnostic ranges do regress the LSP experience. It's not in a significant way that I think is worth blocking this PR but it is reason enough that we should change annotation-snippet to support empty ranges pointing at the start of a line. This isn't something we have to follow up on immediately but I consider it "serious" enough that it's something we should act on in the coming months.

I tested it with `E115` using

```py
if False:  
print()
```

**Before**

The quick fix was only shown when the cursor is at the start of the line:

```text
if False:  
print()
^-- cursor has to be here
```

**Now**

The quick fix is now shown if the cursor is at the start of the line or between `p` and `r`. This is incorrect because the action doesn't make sense at that position:

```text
if False:  
print()
^ ---- cursor can be here
 ^ ----- or here
```

---

_@BurntSushi reviewed on 2025-01-15 16:57_

---

_Review comment by @BurntSushi on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@try_stmt_misspelled_except.py.snap`:200 on 2025-01-15 16:57_

Aye, I filed an issue about this: https://github.com/astral-sh/ruff/issues/15510

---

_@BurntSushi reviewed on 2025-01-15 16:59_

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/pydocstyle/snapshots/ruff_linter__rules__pydocstyle__tests__D208_D.py.snap`:112 on 2025-01-15 16:59_

Yeah I believe this lint just always points to the start of the corresponding line.

---

_Comment by @BurntSushi on 2025-01-15 16:59_

> Would you mind opening an issue to track the "shortcoming" so that we don't forget about it?

Done here: https://github.com/astral-sh/ruff/issues/15509

> I don't suggest you change the emitted range. I'd be okay accepting this regression for now. But we may want to double check if it's the parser's or or rendering fault.

Yeah I did look into this earlier. And I think our spans are correct. I thought I left a comment about it, but it probably got lost in the see of collapsed messages above haha. In any case, I filed an issue about this: https://github.com/astral-sh/ruff/issues/15510

---

_Comment by @BurntSushi on 2025-01-15 17:08_

@MichaReiser had an idea that maybe we can just "fix up" the ranges generated with the diagnostics in the renderer. I think that's worth a try, so I'm going to give that a go before merging here.

---

_Comment by @BurntSushi on 2025-01-15 18:10_

> @MichaReiser had an idea that maybe we can just "fix up" the ranges generated with the diagnostics in the renderer. I think that's worth a try, so I'm going to give that a go before merging here.

This ended up working. I think this will fix the LSP regression?

---

_Merged by @BurntSushi on 2025-01-15 18:37_

---

_Closed by @BurntSushi on 2025-01-15 18:37_

---

_Branch deleted on 2025-01-15 18:37_

---
