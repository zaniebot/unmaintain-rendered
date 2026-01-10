```yaml
number: 14635
title: "[`ruff`] Detect redirected-noqa in file-level comments (`RUF101`)"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
assignees: []
merged: true
base: main
head: brent/fix-14531
created_at: 2024-11-27T14:35:03Z
updated_at: 2024-11-27T22:39:33Z
url: https://github.com/astral-sh/ruff/pull/14635
synced_at: 2026-01-10T20:50:57Z
```

# [`ruff`] Detect redirected-noqa in file-level comments (`RUF101`)

---

_Pull request opened by @ntBre on 2024-11-27 14:35_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes #14531, where ruff was not reporting diagnostics on  file-level `# ruff: noqa` comments with redirected rule codes.

The main change here is making `ParsedFileExemption` closer in structure to `Directive`, with a sequence of `Codes` with `TextRange`s instead of just `&str` for diagnostic reporting.

## Test Plan

<!-- How was it tested? -->
The changes were tested against the example code from #14531 to make sure the diagnostic is now emitted.

---

_Comment by @github-actions[bot] on 2024-11-27 14:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/redirected_noqa.rs`:97 on 2024-11-27 14:50_

Hmm, it's annoying that `FileNoqaDirectives` and `NoqaDirectives` have such a different structure. But maybe there's an opportunity to reuse some of the code in this loop? 

---

_@MichaReiser reviewed on 2024-11-27 14:50_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/ruff/RUF101_1.py`:5 on 2024-11-27 14:51_

Would you mind adding an example where it's a non-direct-match redirect. E.g. `TCH` or `TCH0`

---

_@MichaReiser reviewed on 2024-11-27 14:51_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:438 on 2024-11-27 14:54_

Nit: The current signature is ambigious in the sense that it's unclear whether `offset` is an offset relative to `line` (that's what I would assume) or if it's an absolute index into the source text. 

I suggest changingt the signature to `try_extract(comment_range: TextRange, source: &'a str)` to make this clear (we commonly use `source` to refer to the entire source text).


---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:444 on 2024-11-27 14:55_

Nit: I suggest using `TextLen` methods here to avoid the conversion further down
```suggestion
        let init_line_len = line.text_len();
        let line = Self::lex_whitespace(line);
        let Some(line) = Self::lex_char(line, '#') else {
            return Ok(None);
        };
        let comment_start = init_line_len - line.text_len() - '#'.text_len();
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:480 on 2024-11-27 14:56_


```suggestion
                let codes_end = init_line_len - line.text_len();
                codes.push(Code {
                    code,
                    range: TextRange::at(codes_end, code.text_len()).add(offset),
                });
```

---

_@MichaReiser approved on 2024-11-27 14:57_

Neat! 

---

_Label `rule` added by @MichaReiser on 2024-11-27 14:57_

---

_@ntBre reviewed on 2024-11-27 15:21_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/redirected_noqa.rs`:97 on 2024-11-27 15:21_

Oh yes, great idea! I factored this out into a `build_diagnostics` function.

---

_@ntBre reviewed on 2024-11-27 15:43_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/ruff/RUF101_1.py`:5 on 2024-11-27 15:43_

I tried `# ruff: noqa: TCH`, and it looks like it's not even being detected as a valid code (`ParsedFileExemption::try_extract` returns `Err(MissingCodes)`). Is that what you expected, or am I misunderstanding/broke something? I rolled back to before my changes, and I'm still seeing the `Err(MissingCodes)`, so I'm probably misunderstanding.

---

_@ntBre reviewed on 2024-11-27 15:46_

---

_Review comment by @ntBre on `crates/ruff_linter/src/noqa.rs`:438 on 2024-11-27 15:46_

Ah that makes sense. I was just trying to copy the signature of [Directive::try_extract](https://github.com/astral-sh/ruff/blob/c40b37aa36135283db0b4f16d4582d6879e43cf5/crates/ruff_linter/src/noqa.rs#L56), but this makes more sense here.

---

_@MichaReiser reviewed on 2024-11-27 16:05_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/ruff/RUF101_1.py`:5 on 2024-11-27 16:05_

It's probably me. It's very likely that Ruff doesn't support file-level suppressions for selectors? If that's the case, then we're all good here

---

_@MichaReiser reviewed on 2024-11-27 16:09_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:438 on 2024-11-27 16:09_

Sorry that I haven't been clear about what signature I'd expect. I think this signature still has the same "flaw" in that it's unclear if `comment_range` is an offset relative to `source` or the entire source text of the file. 

To avoid this, I suggest changing the signature to `try_extract(comment_range: TextRange, source; &'a str)` and then call it like this: `ParsedFileExemption::try_extract(range, locator.contents())`; It's then the `try_extract`'s responsible to slice the comment text out of the source text. 



---

_Review comment by @ntBre on `crates/ruff_linter/src/noqa.rs`:438 on 2024-11-27 16:15_

Oh sorry about that. I missed that you suggested `TextRange` instead of `TextSize` :facepalm: This makes much more sense to me now.

---

_@ntBre reviewed on 2024-11-27 16:15_

---

_Comment by @MichaReiser on 2024-11-27 16:40_

Perfect. Let me know when it's ready to merge and I hit the button

---

_@charliermarsh reviewed on 2024-11-27 16:44_

---

_Review comment by @charliermarsh on `crates/ruff_linter/resources/test/fixtures/ruff/RUF101_1.py`:5 on 2024-11-27 16:44_

Yeah I think it has to be an exact code or a catch-all (so `TCH` wouldn't be allowed). 

---

_Comment by @ntBre on 2024-11-27 16:50_

Awesome, thanks for the reviews! I just noticed that the snapshot metadata is not correct anymore after the signature change (the `expression` lines should look like `expression: "ParsedFileExemption::try_extract(TextRange::up_to(source.text_len()), source,)"` now). I ran `cargo insta test --force-update-snapshots`, but it changed the metadata in a ton of other snapshots too. Should I just update the ones I actually changed here, or ignore this for now?

Otherwise ready to merge!

---

_Comment by @MichaReiser on 2024-11-27 16:51_

> Should I just update the ones I actually changed here, or ignore this for now?

Ideally yes. You can also try to update `cargo-insta` to a newer version

---

_Comment by @ntBre on 2024-11-27 17:08_

I think the newest version of insta might be the problem (mitsuhiko/insta#676; adding `snapshot_kind: text` to the metadata now), but I just added the real changes I caused.

---

_Comment by @MichaReiser on 2024-11-27 17:25_

> I think the newest version of insta might be the problem ([mitsuhiko/insta#676](https://github.com/mitsuhiko/insta/issues/676); adding `snapshot_kind: text` to the metadata now), but I just added the real changes I caused.

Oh interesting: I thought I updated all snapshots to use the new format but quiet possible that I missed some.

---

_Merged by @MichaReiser on 2024-11-27 17:25_

---

_Closed by @MichaReiser on 2024-11-27 17:25_

---

_Comment by @AlexWaygood on 2024-11-27 17:41_

> Oh interesting: I thought I updated all snapshots to use the new format but quiet possible that I missed some.

I think some of them have been changed back (and some new ones with the old format have been added) because not all contributors have the latest version of `cargo-insta` installed

---

_Branch deleted on 2024-11-27 17:52_

---
