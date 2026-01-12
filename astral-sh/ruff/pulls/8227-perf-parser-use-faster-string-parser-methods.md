```yaml
number: 8227
title: "perf(parser): use faster string parser methods"
type: pull_request
state: merged
author: sno2
labels:
  - performance
  - parser
assignees: []
merged: true
base: main
head: perf/parser-string
created_at: 2023-10-25T20:41:51Z
updated_at: 2023-10-28T23:01:24Z
url: https://github.com/astral-sh/ruff/pull/8227
synced_at: 2026-01-12T15:55:25Z
```

# perf(parser): use faster string parser methods

---

_@sno2_

## Summary

This makes use of memchr and other methods to parse the strings (hopefully) faster. It might also be worth converting the `parse_fstring_middle` helper to use similar techniques, but I did not implement it in this PR.

## Test Plan

This was tested using the existing tests and passed all of them.

---

_Comment by @codspeed-hq[bot] on 2023-10-25 20:55_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/sno2:perf/parser-string)

### Merging #8227 will **improve performances by 22.78%**

<sub>Comparing <code>sno2:perf/parser-string</code> (28b823f) with <code>main</code> (9792b15)</sub>



### Summary

`⚡ 5` improvements
`✅ 20` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `sno2:perf/parser-string` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-rules[numpy/globals.py]` | 4.1 ms | 3.9 ms | +6.14% |
| ⚡ | `parser[unicode/pypinyin.py]` | 4.1 ms | 3.9 ms | +4.83% |
| ⚡ | `parser[numpy/ctypeslib.py]` | 12 ms | 11.2 ms | +7.36% |
| ⚡ | `parser[numpy/globals.py]` | 1.3 ms | 1.1 ms | +22.78% |
| ⚡ | `linter/default-rules[numpy/globals.py]` | 2 ms | 1.7 ms | +15.48% |


---

_Marked ready for review by @sno2 on 2023-10-25 21:10_

---

_Comment by @github-actions[bot] on 2023-10-25 21:14_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Comment by @MichaReiser on 2023-10-26 00:19_

Wow, that's amazing. We had it on our bucket list to rewrite the String parsing to use our `Cursor` implementation that is also used by the Lexer and should be easier to optimize by the compiler. 

I hope to find some time soon to review this PR. 

---

_Comment by @charliermarsh on 2023-10-26 00:35_

This is really cool, thank you for putting this together.

---

_Comment by @sno2 on 2023-10-26 00:55_

Thank you, I also noticed another panic while looking through the code so hopefully there won't be any more panics in here :)

---

_Label `parser` added by @MichaReiser on 2023-10-27 01:25_

---

_Review requested from @dhruvmanila by @MichaReiser on 2023-10-27 01:25_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string.rs`:162 on 2023-10-27 01:32_

Could we use `find` and create another PR that replaces the search methods with `memchr`? It would allow us to better assess whether using `memchr` is worth it or not.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string.rs`:149 on 2023-10-27 01:33_

Could you explain why this check is no longer necessary? Is it because the optimisation (never was) is no longer necessary because the operation above is so fast and `unicode_names2::character` handles it for us?

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string.rs`:177 on 2023-10-27 01:35_

For another PR: It would be nice if we wouldn't need to allocate a `String` when no character gets replaced (e.g. `\n` remains unchanged).

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string.rs`:210 on 2023-10-27 01:36_

Nit: maybe move this after the error case handling

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string.rs`:305 on 2023-10-27 01:37_

Future (for another PR): It would be nice if we avoid allocating the string if input and output is the same. 

---

_@MichaReiser approved on 2023-10-27 01:40_

Excellent work! And it's good to see how much potential there still is to improve our parser. 

I would prefer if we could split the `memchr` usage out of this PR and submit it as its own PR to better assess whether replacing `find` with `memchr` is worth it.

It would be nice if we could explore using `Cursor` for `StringParser` as part of another PR. `Cursor` is what we use in the `Lexer` and other places where we need to parse text. Using `Cursor` everywhere has the benefit that maintainers are familiar with it, simplifying code reviews and code maintenance.

---

_Review comment by @sno2 on `crates/ruff_python_parser/src/string.rs`:177 on 2023-10-27 02:31_

Yeah, agree. I planned to implement such a feature in a subsequent PR by trying to use `Cow<_>` and considering other methods (e.g. `smol_str`/`compact_str`).

---

_@sno2 reviewed on 2023-10-27 02:31_

---

_@sno2 reviewed on 2023-10-27 02:35_

---

_Review comment by @sno2 on `crates/ruff_python_parser/src/string.rs`:149 on 2023-10-27 02:35_

It seemed like a code smell to me- I did not understand why we should optimize for a fail state as obscure as a unicode escape name > 80 characters.

---

_@sno2 reviewed on 2023-10-27 02:36_

---

_Review comment by @sno2 on `crates/ruff_python_parser/src/string.rs`:210 on 2023-10-27 02:36_

Yup, will do.

---

_@sno2 reviewed on 2023-10-27 02:36_

---

_Review comment by @sno2 on `crates/ruff_python_parser/src/string.rs`:305 on 2023-10-27 02:36_

Agree, and provided input in the other comment.

---

_Comment by @sno2 on 2023-10-27 02:41_

> It would be nice if we could explore using Cursor for StringParser as part of another PR. Cursor is what we use in the Lexer and other places where we need to parse text. Using Cursor everywhere has the benefit that maintainers are familiar with it, simplifying code reviews and code maintenance.

Agree, I believe we could use this technique fairly cleanly in both the lexer and parser with something like `Cursor::eat_until_byte{1,2}` which returns an `Option<&str>`.

---

_@MichaReiser reviewed on 2023-10-27 02:44_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string.rs`:177 on 2023-10-27 02:44_

Cow feels nice but is a bit painful because it also requires adding lifetimes to all AST nodes. I'm not against it, just be prepared to deal with 1000's of lifetime issues 

---

_@sno2 reviewed on 2023-10-27 02:56_

---

_Review comment by @sno2 on `crates/ruff_python_parser/src/string.rs`:177 on 2023-10-27 02:56_

Yeah, the diff would be an eyesore. However, I am wary of the possible lack of change in performance from using an inline string crate. We do enough reading of the strings in the linting/formatting stage to make the control flow required enough to mess up our performance from my previous attempt to introduce stack strings.

Using `Cow<_>` might produce more gains from no copying required which could be greater than the slowdowns caused by added control flow from variant checking. Of course, these changes could be implemented and benched against each other.

---

_@sno2 reviewed on 2023-10-27 03:04_

---

_Review comment by @sno2 on `crates/ruff_python_parser/src/string.rs`:162 on 2023-10-27 03:04_

Wow, only a 0.16% drop without using memchr. Thanks for considering that, I totally expected most of the speedups to be from memchr's vectorization.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/string.rs`:92 on 2023-10-27 09:06_

nit: for consistent styling
```suggestion
    /// # Panics
    ///
    /// When the next byte is a part of a multi-byte character.
```

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/string.rs`:136 on 2023-10-27 09:11_

nit: for me this feels more like an index variable rather than a length. I would prefer `idx` / `index` but if you think `len` is more suitable, it's totally fine.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/string.rs`:149 on 2023-10-27 09:19_

For reference, the relevant issue: https://github.com/RustPython/RustPython/issues/3798

The constant value is now publicly available: https://github.com/progval/unicode_names2/blob/22759d0e725a4c253e401dd8a5edf6d200008299/generator/src/lib.rs#L340, so the following should work.

```rust
use unicode_names2::MAX_NAME_LENGTH;
```

---

_@dhruvmanila approved on 2023-10-27 09:34_

Wow, this is pretty neat. Thanks for doing this!

---

_Label `performance` added by @dhruvmanila on 2023-10-27 09:34_

---

_@sno2 reviewed on 2023-10-27 13:41_

---

_Review comment by @sno2 on `crates/ruff_python_parser/src/string.rs`:136 on 2023-10-27 13:41_

Yeah, I initially named it `idx`; however, the variable could be `3` if all of the octets are set in the escape. `3` would not be a valid index, so I named it `len`.

---

_Comment by @sno2 on 2023-10-27 13:47_

@dhruvmanila The `MAX_NAME_LENGTH` is public in the generated file. But, the file that uses the constants does not mark them as public:

https://github.com/progval/unicode_names2/blob/22759d0e725a4c253e401dd8a5edf6d200008299/src/lib.rs#L70-L72

Would you like for me to re-copy the constant into our source?

(The reply box is not underneath your response for some reason.)

---

_Review comment by @charliermarsh on `crates/ruff_python_parser/src/string.rs`:149 on 2023-10-28 01:35_

I'd suggest we re-add -- costs us very little (nothing?) and gives us an error rather than a panic, if I understand this conversation correctly.

---

_@charliermarsh reviewed on 2023-10-28 01:35_

---

_@sno2 reviewed on 2023-10-28 04:59_

---

_Review comment by @sno2 on `crates/ruff_python_parser/src/string.rs`:149 on 2023-10-28 04:59_

I have validated that the error does not exist by testing the previous reproduction. The issue was fixed in the crate here https://github.com/progval/unicode_names2/commit/9404fb6e48f93ef0b5b9dfb5d7aaff1f0c64168c (Note that it is included in the `1.2.0` tag that we are using)

I did not realize that the motivation was to fix a previous panic in the crate and not a performance trick. Therefore, should we be fine not adding in the magic constants again?

---

_@charliermarsh reviewed on 2023-10-28 22:50_

---

_Review comment by @charliermarsh on `crates/ruff_python_parser/src/string.rs`:149 on 2023-10-28 22:50_

Excellent -- thank you for testing this.

---

_Merged by @charliermarsh on 2023-10-28 22:50_

---

_Closed by @charliermarsh on 2023-10-28 22:50_

---

_Comment by @charliermarsh on 2023-10-28 22:51_

Thanks @sno2, great to have you contributing!

---

_Branch deleted on 2023-10-28 23:01_

---
