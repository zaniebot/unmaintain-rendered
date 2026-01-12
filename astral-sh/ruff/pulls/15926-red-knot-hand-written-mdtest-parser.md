```yaml
number: 15926
title: "[red-knot] Hand-written MDTest parser"
type: pull_request
state: merged
author: sharkdp
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: david/mdtest-parser-rewrite
created_at: 2025-02-04T11:23:25Z
updated_at: 2025-02-04T13:57:04Z
url: https://github.com/astral-sh/ruff/pull/15926
synced_at: 2026-01-12T15:55:53Z
```

# [red-knot] Hand-written MDTest parser

---

_@sharkdp_

## Summary

Replaces our existing Markdown test parser with a fully hand-written parser. I tried to fix this bug using the old approach and kept running into problems. Eventually this seemed like the easier way. It's more code (+50 lines, excluding the new test), but I hope it's relatively straightforward to understand, compared to the complex interplay between the byte-stream-manipulation and regex-parsing that we had before.

I did not really focus on performance, as the parsing time does not dominate the test execution time, but this seems to be slightly faster than what we had before (executing all MD tests; debug):

| Command | Mean [s] | Min [s] | Max [s] | Relative |
|:---|---:|---:|---:|---:|
| this branch | 2.775 ± 0.072 | 2.690 | 2.877 | 1.00 |
| `main` | 2.921 ± 0.034 | 2.865 | 2.967 | 1.05 ± 0.03 |

closes #15923

## Test Plan

One new regression test.


---

_Label `testing` added by @sharkdp on 2025-02-04 11:23_

---

_Label `red-knot` added by @sharkdp on 2025-02-04 11:23_

---

_Review requested from @carljm by @sharkdp on 2025-02-04 11:23_

---

_Review requested from @MichaReiser by @sharkdp on 2025-02-04 11:23_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-02-04 11:23_

---

_Review comment by @sharkdp on `crates/red_knot_test/src/parser.rs`:344 on 2025-02-04 11:24_

This is something that we could remove. It seems strange that we trim trailing newlines of code blocks. But I didn't want to change the behavior in this PR.

---

_@sharkdp reviewed on 2025-02-04 11:24_

---

_@sharkdp reviewed on 2025-02-04 11:26_

---

_Review comment by @sharkdp on `crates/red_knot_test/src/parser.rs`:318 on 2025-02-04 11:26_

This message could be improved/adjusted according to the logic in the `if` condition, but I didn't want to introduce and behavioral changes here.

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/parser.rs`:286 on 2025-02-04 11:35_

Are `#` considered header even if they're not at the start of a line? E.g. I think the following are just regular hash signs.

```md
    ## not a title
   # title
  # title
 # title
# title
```

Huh, at least github's markdown seems odd about what it considers a title. The same probably applies to code blocks.

I'd be okay to disallow code blocks and headers that aren't at the start of the line.

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/parser.rs`:286 on 2025-02-04 11:39_

Nit: I've a slight preference to use `self.cursor.bump()` over `self.cursor.is_eof` & `self.cursor.first` because:

* It guarantees that the lexer/parser progresses in each iteration
* It's easy to forget a `bump` call with `first`. 


```rust
while let Some(first) = self.cursor.bump() {
	match first {
		'#' => {
			...
		}
		c => if c.is_whitespace() {
			self.skip_whitespace()
		}
		c => {
			self.skip_to_end_of_line()
		}
	}
}
```


But I admit it's somewhat more awkward with the `skip_whitespace`

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/parser.rs`:319 on 2025-02-04 11:41_

Nit: I'd use a `eat_char` here OR `eat_char2`

```rust
if self.cursor.eat_char2('`', '`') {
	// Handle triple quotes
} else if self.preceding_blank_lines > 0 {
	... 
} else {
}
```

I generally prefer `eat` over `first` + `bump` because `first` is easy to get wrong if at `EOF` and it removes the guessing around which token `bump` "bumps"

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/parser.rs`:334 on 2025-02-04 11:43_

Calling `skip_to_beginning_of_next_line` seems unnecessary here because it will always eat the `\n`. Should this just be `if !self.cursor.eat('\n') { bail!(...) }`

---

_Review comment by @MichaReiser on `crates/red_knot_test/src/parser.rs`:343 on 2025-02-04 11:44_

Nit: I find using `char::len_utf8` improves readability of code handling byte offsets because it makes it explicit what character we're offseting

```suggestion
                                    code = &code[..code.len() - '\n'.len_utf8()];
```

---

_@MichaReiser approved on 2025-02-04 11:45_

This is great. No more regex :) 

---

_@sharkdp reviewed on 2025-02-04 12:38_

---

_Review comment by @sharkdp on `crates/red_knot_test/src/parser.rs`:334 on 2025-02-04 12:38_

Yes!

---

_@sharkdp reviewed on 2025-02-04 12:45_

---

_Review comment by @sharkdp on `crates/red_knot_test/src/parser.rs`:286 on 2025-02-04 12:45_

Good catch. This was just wrong. I added two new tests.

---

_@sharkdp reviewed on 2025-02-04 12:46_

---

_Review comment by @sharkdp on `crates/red_knot_test/src/parser.rs`:286 on 2025-02-04 12:46_

Thanks Micha. This is much nicer now, actually (the skip_whitespace was wrong anyway). I eliminated all `.first()` occurrences except for one in `consume_until`.

---

_Comment by @github-actions[bot] on 2025-02-04 12:51_

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

_Merged by @sharkdp on 2025-02-04 13:01_

---

_Closed by @sharkdp on 2025-02-04 13:01_

---

_Branch deleted on 2025-02-04 13:01_

---

_@MichaReiser reviewed on 2025-02-04 13:03_

---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/cursor.rs`:97 on 2025-02-04 13:03_

Oh, I wasn't aware that `eat_char2` is only available on the ruff_python_parser `Cursor`. 

---

_@sharkdp reviewed on 2025-02-04 13:54_

---

_Review comment by @sharkdp on `crates/ruff_python_trivia/src/cursor.rs`:97 on 2025-02-04 13:54_

Oh. And I didn't notice that `eat_char2` was available *somewhere*. My implementation is slightly different:

```rs
pub fn eat_char2(&mut self, c1: char, c2: char) -> bool {
        if self.first() == c1 && self.second() == c2 {
            self.bump();
            self.bump();
            true
        } else {
            false
        }
    }
```

vs the existing:

```rs
pub(super) fn eat_char2(&mut self, c1: char, c2: char) -> bool {
        let mut chars = self.chars.clone();
        if chars.next() == Some(c1) && chars.next() == Some(c2) {
            self.bump();
            self.bump();
            true
        } else {
            false
        }
    }
```

Should I do anything about this?

---

_@MichaReiser reviewed on 2025-02-04 13:57_

---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/cursor.rs`:97 on 2025-02-04 13:57_

I think it's fine. The second might be slightly faster but then, it's used rarely. 

---
