```yaml
number: 2100
title: "feat: report line and column on requirements parser errors"
type: pull_request
state: merged
author: samypr100
labels:
  - error messages
assignees: []
merged: true
base: main
head: requirements-parsing-line-column
created_at: 2024-03-01T03:32:35Z
updated_at: 2024-03-11T22:21:56Z
url: https://github.com/astral-sh/uv/pull/2100
synced_at: 2026-01-12T16:04:52Z
```

# feat: report line and column on requirements parser errors

---

_@samypr100_

## Summary

Closes #2012

This changes `RequirementsTxtParserError::Parser` to take a line, column instead of cursor location to improve reporting of parser errors. A new function was added to compute the line and column based on the content and cursor location when a parser error occurs for simplicity.

Given `uv pip compile .\requirements.txt` of below
```
numpy>=1,<2
  --borken
tqdm
```

Before:

``` 
error: Unexpected '-', expected '-c', '-e', '-r' or the start of a requirement in `.\requirements.txt` at position 14
```

After:

```
error: Unexpected '-', expected '-c', '-e', '-r' or the start of a requirement in `.\requirements.txt` at position 2:3
```

Open Question: Do we want to support `line:column` for other types of errors? I didn't look dig other potential error types where this might be desired.

## Test Plan

New test was added to `requirements-txt` crate with this example.

---

_Marked ready for review by @samypr100 on 2024-03-01 03:38_

---

_@charliermarsh reviewed on 2024-03-01 03:43_

---

_Review comment by @charliermarsh on `crates/requirements-txt/src/lib.rs`:330 on 2024-03-01 03:43_

So here, if we see `\r\n`, I think we technically want to avoid incrementing `column` after we see the `\r`. I can't see how this would matter in practice for _this_ use-case, but e.g., you can see an example of how that's done here in Ruff: https://github.com/astral-sh/ruff/blob/c9931a548ff07c031130571f3664343bea224026/crates/ruff_source_file/src/line_index.rs#L29

---

_@samypr100 reviewed on 2024-03-01 03:48_

---

_Review comment by @samypr100 on `crates/requirements-txt/src/lib.rs`:330 on 2024-03-01 03:48_

>  I think we technically want to avoid incrementing column after we see the \r

Agreed, I left this as-is on purpose but I really debated to either keep this for simplicity or track the prev_char reference for these types of checks. Luckily it can be changed easily whichever route we want to go.

---

_@charliermarsh reviewed on 2024-03-01 03:51_

---

_Review comment by @charliermarsh on `crates/requirements-txt/src/lib.rs`:330 on 2024-03-01 03:51_

I'd prefer to change it just for completeness, in case this gets reused elsewhere. Are you ok to modify it?

---

_@samypr100 reviewed on 2024-03-01 03:56_

---

_Review comment by @samypr100 on `crates/requirements-txt/src/lib.rs`:330 on 2024-03-01 03:56_

Let me know if this is what you had in mind d0f7161f1c4b2e903f73256b51368d32dbb34cc5

---

_@charliermarsh reviewed on 2024-03-01 05:40_

---

_Review comment by @charliermarsh on `crates/requirements-txt/src/lib.rs`:330 on 2024-03-01 05:40_

I was aiming for something more like: if we see `\r`, check if the next character is `\n`; if so, skip it. (That would correctly handle `\r`, `\n`, and `\r\n`.

---

_Review comment by @konstin on `crates/requirements-txt/src/lib.rs`:955 on 2024-03-01 10:43_

Could you make that format `at <REQUIREMENTS_TXT>:<line>:<col>`? This format is support by many IDEs

---

_Review comment by @konstin on `crates/requirements-txt/src/lib.rs`:325 on 2024-03-01 10:46_

I think we want byte indices rather than char indices here? CC @BurntSushi 

---

_@konstin approved on 2024-03-01 10:57_

---

_Review comment by @BurntSushi on `crates/requirements-txt/src/lib.rs`:325 on 2024-03-01 11:32_

`char_indices` does actually return byte offsets. The `index` is referring to the index in `content` of where `char` starts. For example, this code:

```rust
let content = "a💩b";
for (i, ch) in content.char_indices() {
    eprintln!("{}:{}", i, ch);
}
```

Has this output:

```
0:a
1:💩
5:b
```

Since `💩` has a UTF-8 encoding that uses 4 bytes.

---

_Review comment by @BurntSushi on `crates/requirements-txt/src/lib.rs`:334 on 2024-03-01 11:38_

I would suggest using the [`unicode-width`](https://docs.rs/unicode-width/latest/unicode_width/) crate to compute the visual width of the codepoint via [`UnicodeWidthChar::width`](https://docs.rs/unicode-width/latest/unicode_width/trait.UnicodeWidthChar.html#tymethod.width).

---

_Review comment by @BurntSushi on `crates/requirements-txt/src/lib.rs`:320 on 2024-03-01 11:39_

Assuming you use `unicode-width` per my suggestion below, can you just add a quick note here documenting that we define column in this context as the, "offset according to the visual width of each codepoint."

---

_Review comment by @BurntSushi on `crates/requirements-txt/src/lib.rs`:1377 on 2024-03-01 11:40_

Could you add a couple of tests that include non-ASCII codepoints. More concretely, one test with a non-ASCII codepoint like, say, `💩`. And then another test with grapheme cluster made up of more than one codepoint like `à̖` (that's `a\u{0300}\u{0316}` as a string literal in Rust).

---

_@BurntSushi reviewed on 2024-03-01 11:40_

---

_@MichaReiser reviewed on 2024-03-01 11:43_

---

_Review comment by @MichaReiser on `crates/requirements-txt/src/lib.rs`:334 on 2024-03-01 11:43_

I don't think we should be using `width` here. I would need to check again but what I remember is that editors use character offsets for column indices and not their width. Ruff also uses character offsets and not the width for diagnostics. What `uv` output should match editors or shortcuts like *go to position* won't work.

---

_@BurntSushi reviewed on 2024-03-01 11:53_

---

_Review comment by @BurntSushi on `crates/requirements-txt/src/lib.rs`:334 on 2024-03-01 11:53_

Yeah I agree that we should do whatever editors are likely to use here.

---

_@BurntSushi reviewed on 2024-03-01 11:53_

---

_Review comment by @BurntSushi on `crates/requirements-txt/src/lib.rs`:320 on 2024-03-01 11:53_

If `unicode-width` is not the right thing to use, then just adding, "offset according to the number of Unicode codepoints."

---

_@charliermarsh reviewed on 2024-03-01 14:38_

---

_Review comment by @charliermarsh on `crates/requirements-txt/src/lib.rs`:334 on 2024-03-01 14:38_

Yeah, I think char_indices is okay here, but I would like to tweak the newline handling slightly in line with the Ruff reference above.

---

_@samypr100 reviewed on 2024-03-01 18:45_

---

_Review comment by @samypr100 on `crates/requirements-txt/src/lib.rs`:330 on 2024-03-01 18:45_

Sorry for the delay (work 😆), hopefully this is closer 223fd99ac1b44e0557227d1c461a20bf1a9d85f2

---

_@samypr100 reviewed on 2024-03-01 18:45_

---

_Review comment by @samypr100 on `crates/requirements-txt/src/lib.rs`:955 on 2024-03-01 18:45_

Changed in 223fd99ac1b44e0557227d1c461a20bf1a9d85f2

---

_@samypr100 reviewed on 2024-03-01 18:46_

---

_Review comment by @samypr100 on `crates/requirements-txt/src/lib.rs`:320 on 2024-03-01 18:46_

I left it as a comment in 223fd99ac1b44e0557227d1c461a20bf1a9d85f2

---

_@samypr100 reviewed on 2024-03-01 18:46_

---

_Review comment by @samypr100 on `crates/requirements-txt/src/lib.rs`:1377 on 2024-03-01 18:46_

Done in 223fd99ac1b44e0557227d1c461a20bf1a9d85f2, included one with two codepoints and your example one with three.

---

_@samypr100 reviewed on 2024-03-01 19:10_

---

_Review comment by @samypr100 on `crates/requirements-txt/src/lib.rs`:955 on 2024-03-01 19:10_

Note, mileage may vary, I noticed that for full-paths it does highlight on my IDE, but not relative paths like `./requirements.txt`. We could try to canonicalize always?

---

_@konstin reviewed on 2024-03-01 19:29_

---

_Review comment by @konstin on `crates/requirements-txt/src/lib.rs`:955 on 2024-03-01 19:29_

I think using `current_dir()` and joining the path on top of it is the best way, canonicalize could be a path outside the apparent project root (we used to canonicalize everything but rolled that back since it caused problems where software expected the "virtual" name of the link).

---

_@charliermarsh approved on 2024-03-01 21:40_

Awesome, thanks!

---

_@charliermarsh reviewed on 2024-03-01 21:40_

---

_Review comment by @charliermarsh on `crates/requirements-txt/src/lib.rs`:988 on 2024-03-01 21:40_

Tweaked this a little because `\r` on its own _should_ be considered a newline (it's not used on (m)any modern platforms, but older macOS did use it).

---

_Label `error messages` added by @charliermarsh on 2024-03-01 21:41_

---

_Merged by @charliermarsh on 2024-03-01 21:50_

---

_Closed by @charliermarsh on 2024-03-01 21:50_

---

_@samypr100 reviewed on 2024-03-01 22:13_

---

_Review comment by @samypr100 on `crates/requirements-txt/src/lib.rs`:988 on 2024-03-01 22:13_

Np, I went a bit back and forth on that. Glad it's settled

---

_@charliermarsh reviewed on 2024-03-01 22:53_

---

_Review comment by @charliermarsh on `crates/requirements-txt/src/lib.rs`:988 on 2024-03-01 22:53_

Sorry for all the back-and-forth, I just have scars from Ruff where we didn't handle all the newline kinds and eventually hit panics in certain projects.

---

_@samypr100 reviewed on 2024-03-01 23:43_

---

_Review comment by @samypr100 on `crates/requirements-txt/src/lib.rs`:988 on 2024-03-01 23:43_

no worries, makes sense, and feedback is always welcomed 💯 

---

_Branch deleted on 2024-03-11 22:21_

---
