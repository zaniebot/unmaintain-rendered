```yaml
number: 13530
title: "[`refurb`] implement `hardcoded-string-charset` (FURB156)"
type: pull_request
state: merged
author: alex-700
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: latyshev/furb-156
created_at: 2024-09-26T18:14:27Z
updated_at: 2025-06-10T20:46:23Z
url: https://github.com/astral-sh/ruff/pull/13530
synced_at: 2026-01-12T15:55:44Z
```

# [`refurb`] implement `hardcoded-string-charset` (FURB156)

---

_@alex-700_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Implement `hardcoded-string-charset` (FURB156)
See:
- https://github.com/astral-sh/ruff/issues/1348
- [original lint](https://github.com/dosisod/refurb/blob/master/refurb/checks/string/charsets.py)

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
cargo test
<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2024-09-26 18:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+3 -0 violations, +0 -0 fixes in 2 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/e46006b25b025eee2feb5aedcb3bb01069a4b730/dev/breeze/src/airflow_breeze/utils/packages.py#L433'>dev/breeze/src/airflow_breeze/utils/packages.py:433:48:</a> FURB156 [*] Use of hardcoded string charset
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/examples/reference/models/Text.py#L9'>examples/reference/models/Text.py:9:5:</a> FURB156 [*] Use of hardcoded string charset
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/tests/unit/bokeh/util/test_token.py#L69'>tests/unit/bokeh/util/test_token.py:69:20:</a> FURB156 [*] Use of hardcoded string charset
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| FURB156 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>




---

_Assigned to @zanieb by @zanieb on 2024-09-26 18:58_

---

_@MichaReiser reviewed on 2024-09-26 19:03_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/hardcoded_string_charset.rs`:51 on 2024-09-26 19:03_

This is clever! But also somewhat complicated. Have you done a performance comparison with https://docs.rs/aho-corasick/latest/aho_corasick/ 

CC: @BurntSushi maybe you have a sense for whether the bitset approach or corasick is better here.

---

_@alex-700 reviewed on 2024-09-26 19:36_

---

_Review comment by @alex-700 on `crates/ruff_linter/src/rules/refurb/rules/hardcoded_string_charset.rs`:51 on 2024-09-26 19:36_

> Have you done a performance comparison with https://docs.rs/aho-corasick/latest/aho_corasick/?

No.

There are two parts here:
- `exact`: there are 9 `&[u8]` comparisons, which are usually fail by `.len()` comparison. Do not believe that aho-corasick could help a lot (if we do not lift the scope higher than `fn string_like`).
- `as_set`: 
  - in the current implementation works only in `... [not] in <str literal>` case (looks not so often to care)
  - if we would like to save the semantic `set(...) == set(string.<charset>)`, AFAIK, aho-corasick does not help us. We still need some `set`-ish solution (e.g. like this inlined bitset). The one idea we can add is "early return if the byte is not `printable`", but looks like premature optimization for me. 

---

_Label `rule` added by @MichaReiser on 2024-09-27 06:21_

---

_Label `preview` added by @MichaReiser on 2024-09-27 06:21_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/hardcoded_string_charset.rs`:69 on 2024-09-27 06:23_

Let's assert that a `Charset` is ascii only

```suggestion
						assert!(bytes[i].is_ascii());
            bitset |= 1 << bytes[i];
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/hardcoded_string_charset.rs`:102 on 2024-09-27 06:24_

I suggest that we extract a `AsciiCharSet` struct that wraps the bitset to avoid repeating the bitset calculation. It also offers an opportunity to write some documentation.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/hardcoded_string_charset.rs`:174 on 2024-09-27 06:32_

The only charset that could be an exact match here should be the charset found by `check_charset_as_set`. Searching over all charsets seems unnecessary.

```suggestion
        if charset.is_same(&value.to_str()) {
        return;
    }
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/hardcoded_string_charset.rs`:184 on 2024-09-27 06:36_

You can move the invocation of this rule to the string-literal phase if it only needs to run on string literals.
```suggestion
pub(crate) fn hardcoded_string_charset_literal(checker: &mut Checker, string_literal: &ExprStringLiteral) {
    let StringLike::String(expr) = string_like else {
        return;
    };
    let Some(charset) = check_charset_exact(expr.value.to_str().as_bytes()) else {
        return;
    };
    push_diagnostic(checker, string_like.range(), charset);
}
```

 to https://github.com/astral-sh/ruff/blob/40149a17f5cf061664b849f7d6c0d03e42616038/crates/ruff_linter/src/checkers/ast/analyze/expression.rs#L1100-L1101 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/hardcoded_string_charset.rs`:166 on 2024-09-27 06:41_

Nit: We prefer to pass the entire expression rather than picking individual fields
```suggestion
pub(crate) fn hardcoded_string_charset_comparison(
    checker: &mut Checker,
    ExprCompare {
        ops, comparators, ..
    }: &ExprCompare,
) {
    let ([CmpOp::In | CmpOp::NotIn], [Expr::StringLiteral(ExprStringLiteral { value, .. })]) =
        (&**ops, &**comparators)
    else {
        return;
    };
```

---

_@MichaReiser requested changes on 2024-09-27 06:47_

Thanks. This looks great. 

I left a few smaller comments. It would also be great to add some documentation to `Charset` and `bitset`, considering that the representations are non-trivial and make some assumptions about how they're used.

---

_@alex-700 reviewed on 2024-09-27 10:42_

---

_Review comment by @alex-700 on `crates/ruff_linter/src/rules/refurb/rules/hardcoded_string_charset.rs`:102 on 2024-09-27 10:42_

Added.

---

_@alex-700 reviewed on 2024-09-27 10:42_

---

_Review comment by @alex-700 on `crates/ruff_linter/src/rules/refurb/rules/hardcoded_string_charset.rs`:174 on 2024-09-27 10:42_

Thanks! Nice catch! Fixed.

---

_@alex-700 reviewed on 2024-09-27 10:43_

---

_Review comment by @alex-700 on `crates/ruff_linter/src/rules/refurb/rules/hardcoded_string_charset.rs`:184 on 2024-09-27 10:43_

IIUC, the right place was https://github.com/astral-sh/ruff/blob/a4a585da2a98f94407e602fc2328d5e4207185f3/crates/ruff_linter/src/checkers/ast/analyze/expression.rs#L1370
Moved.

---

_@alex-700 reviewed on 2024-09-27 10:45_

---

_Review comment by @alex-700 on `crates/ruff_linter/src/rules/refurb/rules/hardcoded_string_charset.rs`:166 on 2024-09-27 10:45_

Was copy-pasted from the neighbor:
https://github.com/astral-sh/ruff/blob/a4a585da2a98f94407e602fc2328d5e4207185f3/crates/ruff_linter/src/checkers/ast/analyze/expression.rs#L1356

Fixed now.

---

_@alex-700 reviewed on 2024-09-27 10:46_

---

_Review comment by @alex-700 on `crates/ruff_linter/src/rules/refurb/rules/hardcoded_string_charset.rs`:69 on 2024-09-27 10:46_

Asserted with `unwrap()`

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/refurb/FURB156.py`:14 on 2024-09-27 11:52_

Could you add a test for a case where the string is parenthesized

```suggestion
_ = (
	'0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ!"#$%&\'()*+,-./:;<=>?@[\\]^_`{|}~ \t\n\r\x0b\x0c'
)
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/hardcoded_string_charset.rs`:65 on 2024-09-27 11:54_

What's the motivation for having both `from_bytes` and `from_bytes_const`? Aren't both returning `None` if the string contains any non ascii character? 

---

_@MichaReiser approved on 2024-09-27 11:57_

Nice, thanks for following up. Let's add a test for when the expression is parenthesized to verify that the fix is correct (if not, take a look at `parenthesized_range`. 

I also think that we may be able to remove the factory methods on `AsciiCharSet` and reduce them to just one.

---

_@BurntSushi reviewed on 2024-09-27 12:12_

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/refurb/rules/hardcoded_string_charset.rs`:51 on 2024-09-27 12:12_

I don't have a good sense here. 9 memcmp's could be very quick. This is the sort of thing where, if you want to go as fast as possible, you probably need a micro-benchmark to try and optimize it. But I think the approach here looks very reasonable to me.

---

_Unassigned @zanieb by @MichaReiser on 2024-09-27 13:46_

---

_@alex-700 reviewed on 2024-09-29 16:14_

---

_Review comment by @alex-700 on `crates/ruff_linter/resources/test/fixtures/refurb/FURB156.py`:14 on 2024-09-29 16:14_

https://github.com/astral-sh/ruff/pull/13530/commits/066e7006e4b5fad341fa8c84a85ec1f5cf5d3602
I added the test and "took a look" on `parenthesized_range`, but its usage was really not intuitive for me in that special case. Could I somehow simplify "getting the parent" here? 

---

_@alex-700 reviewed on 2024-09-29 16:29_

---

_Review comment by @alex-700 on `crates/ruff_linter/src/rules/refurb/rules/hardcoded_string_charset.rs`:65 on 2024-09-29 16:29_

I would like to stick to using a `const` here (another option, I would like to avoid, is runtime evaluation under "OnceLock"):
https://github.com/astral-sh/ruff/blob/066e7006e4b5fad341fa8c84a85ec1f5cf5d3602/crates/ruff_linter/src/rules/refurb/rules/hardcoded_string_charset.rs#L108
So I need `from_bytes` to be a `const fn`, but unfortunately currently "in stable" it cannot have a clean implementation like this:
https://github.com/astral-sh/ruff/blob/066e7006e4b5fad341fa8c84a85ec1f5cf5d3602/crates/ruff_linter/src/rules/refurb/rules/hardcoded_string_charset.rs#L62-L65

So I added the dirty implementation (which can be `const fn`) under another name `from_bytes_const`:
https://github.com/astral-sh/ruff/blob/066e7006e4b5fad341fa8c84a85ec1f5cf5d3602/crates/ruff_linter/src/rules/refurb/rules/hardcoded_string_charset.rs#L70-L85 

And there is a TODO to remove it, when stable supports `const fn` like usual `from_bytes`.

The third factory is about the usage of `Option::unwrap` in `const fn`. Currently is not possible (wait for https://github.com/rust-lang/rust/issues/67441). When it will be shipped, we can write
```rust
            ascii_char_set: AsciiCharSet::from_bytes_const(bytes).unwrap(),
```
instead of
https://github.com/astral-sh/ruff/blob/066e7006e4b5fad341fa8c84a85ec1f5cf5d3602/crates/ruff_linter/src/rules/refurb/rules/hardcoded_string_charset.rs#L103

---

_@MichaReiser reviewed on 2024-09-29 17:45_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/hardcoded_string_charset.rs`:65 on 2024-09-29 17:45_

I do like the const version but do we need  the non const version?

---

_@alex-700 reviewed on 2024-09-29 22:45_

---

_Review comment by @alex-700 on `crates/ruff_linter/src/rules/refurb/rules/hardcoded_string_charset.rs`:65 on 2024-09-29 22:45_

The const version is non-idiomatic and will surprise a reader.

Having this non-idiomatic version in runtime could have some interesting behavior of optimization part of compiler. 
While there is a difference in generated code ( see https://godbolt.org/z/53T3z86f8 ), I assume it is a good default decision to have two implemenation untill the `const fn` supports idiomatic implementation.

---

_@BurntSushi reviewed on 2024-09-30 11:53_

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/refurb/rules/hardcoded_string_charset.rs`:65 on 2024-09-30 11:53_

I'd be fine with keeping the code simpler and using `OnceLock` if that works personally.

Otherwise, if the non-const version isn't being used, I'd drop it. If you're worried about surprising the reader, then you could write a short comment. (Although I don't think it's that surprising. If you've ever tried to write a const fn before, you'll know that you're pretty limited in the tools you have.)

---

_@alex-700 reviewed on 2024-09-30 12:27_

---

_Review comment by @alex-700 on `crates/ruff_linter/src/rules/refurb/rules/hardcoded_string_charset.rs`:65 on 2024-09-30 12:27_

https://github.com/astral-sh/ruff/pull/13530/commits/b9b3b64937f000c66a850b541df17b4899bb3ed5
Ok. `AsciiCharSet` interface was simplified to the direction of only `const` implementation. 
Also: 
- `from_bytes_const_unwrap` was inlined to localize the story of `Option::unwrap` in `const` context.
- slightly change the `TODO` comments.

---

_@MichaReiser reviewed on 2024-10-07 07:24_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/refurb/FURB156.py`:14 on 2024-10-07 07:24_

I think it's fine to preserve the parentheses in this case. That also guarantees that we preserve (most) comments. I'm sorry that I set you off on the wrong trail.

---

_@MichaReiser approved on 2024-10-07 07:25_

Nice thanks. This looks great!

---

_Merged by @MichaReiser on 2024-10-07 07:35_

---

_Closed by @MichaReiser on 2024-10-07 07:35_

---

_Branch deleted on 2025-06-10 20:46_

---
