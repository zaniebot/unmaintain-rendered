```yaml
number: 19065
title: "[`ruff`] Offer fixes for `RUF039` in more cases"
type: pull_request
state: merged
author: robsdedude
labels:
  - fixes
  - preview
assignees: []
merged: true
base: main
head: feat/ruf039-better-fixes
created_at: 2025-07-01T07:58:44Z
updated_at: 2025-07-24T18:33:41Z
url: https://github.com/astral-sh/ruff/pull/19065
synced_at: 2026-01-10T17:58:13Z
```

# [`ruff`] Offer fixes for `RUF039` in more cases

---

_Pull request opened by @robsdedude on 2025-07-01 07:58_

## Summary
Expand cases in which ruff can offer a fix for `RUF039` (some of which are unsafe).

While turning `"\n"` (== `\n`) into `r"\n"` (== `\\n`) is not equivalent at run-time, it's still functionally equivalent to do so in the context of [regex patterns](https://docs.python.org/3/library/re.html#regular-expression-syntax) as they themselves interpret the escape sequence. Therefore, an unsafe fix can be offered.

Further, this PR also makes ruff offer fixes for byte string literals, not only strings literals as before.

## Test Plan
Tests for all escape sequences have been added.

## Related
Closes: https://github.com/astral-sh/ruff/issues/16713


---

_Comment by @github-actions[bot] on 2025-07-01 08:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +2 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+0 -0 violations, +2 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pytest-dev/pytest/blob/cfc66062cb298e8b90a2849127e2104cd5070293/testing/python/metafunc.py#L432'>testing/python/metafunc.py:432:45:</a> RUF039 First argument to `re.compile()` is not raw bytes literal
+ <a href='https://github.com/pytest-dev/pytest/blob/cfc66062cb298e8b90a2849127e2104cd5070293/testing/python/metafunc.py#L432'>testing/python/metafunc.py:432:45:</a> RUF039 [*] First argument to `re.compile()` is not raw bytes literal
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF039 | 2 | 0 | 0 | 2 | 0 |

</p>
</details>




---

_Marked ready for review by @robsdedude on 2025-07-01 08:40_

---

_Review requested from @ntBre by @MichaReiser on 2025-07-07 12:57_

---

_Label `fixes` added by @MichaReiser on 2025-07-07 12:57_

---

_Label `preview` added by @ntBre on 2025-07-08 14:02_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unraw_re_pattern.rs`:28 on 2025-07-08 14:07_

Sometimes we also include a `## Fix availability` section. Would that be useful here? "If a fix is available" is kind of raising that question for me.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unraw_re_pattern.rs`:279 on 2025-07-08 14:20_

This is some related code that I wrote for another rule:

https://github.com/astral-sh/ruff/blob/2f8bc9a30bbcb3839fa0a5031e0bbae329210e53/crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs#L191-L210

It looks like we're handling a slightly different set of characters there: `abfnrtv`. `b` is present there but not here and `x` is here but not there, for example. Is that expected, or should the two be the same?

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unraw_re_pattern.rs`:279 on 2025-07-08 14:26_

I kind of like this `str::contains` check more than having to pass a closure. Then we could avoid calling the `checker.target_version()` repeatedly too, not that it's too expensive. For example:

```rust
let allowed = if checker.target_version() >= PythonVersion::PY38 {
    "afnrtuUvxN"
} else {
    "afnrtuUvx"
};
```

and for bytes it would just be `"afnrtvx"`.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unraw_re_pattern.rs`:279 on 2025-07-08 14:31_

There's also this section in the [docs](https://docs.python.org/3/library/re.html#module-contents) (just above the linked node):

> Octal escapes are included in a limited form. If the first digit is a 0, or if there are three octal digits, it is considered an octal escape. Otherwise, it is a group reference. As for string literals, octal escapes are always at most three digits in length.

This might be too niche to worry about, if it's relevant at all. I see there's a test case with an octal escape, so this seems intentional.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unraw_re_pattern.rs`:257 on 2025-07-08 14:42_

```suggestion
                // If the next character is not one of the whitelisted ones, we likely cannot safely turn
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unraw_re_pattern.rs`:191 on 2025-07-08 14:42_

```suggestion
/// Check how safe it is to prepend the `r` prefix to the string.
```

---

_@ntBre reviewed on 2025-07-08 14:44_

Thanks! I just had a couple of questions/suggestions around the allowed escape characters and an idea for the docs.

---

_@robsdedude reviewed on 2025-07-22 17:18_

---

_Review comment by @robsdedude on `crates/ruff_linter/src/rules/ruff/rules/unraw_re_pattern.rs`:279 on 2025-07-22 17:18_

> I kind of like this `str::contains` check more 

If you prefer the `str::contains` approach for readability, I'll follow whatever you prefer as this is your code base to maintain. If you worry about performance: match is often faster. Pulling the `checker.target_version()` out of the closure is still possible and since Rust will monomorphize `raw_applicability` with the closure/anonymous function the compiler can optimize things to its heart's content (as opposed to receiving an opaque `str` and calling `contains` on it (see [godbolt example](https://godbolt.org/#g:!((g:!((g:!((h:codeEditor,i:(filename:'1',fontScale:14,fontUsePx:'0',j:1,lang:rust,selection:(endColumn:74,endLineNumber:11,positionColumn:74,positionLineNumber:11,selectionStartColumn:74,selectionStartLineNumber:11,startColumn:74,startLineNumber:11),source:'use+std::hint::black_box%3B%0A%0A%23%5Binline(never)%5D%0Apub+fn+f1(c:+char)+-%3E+bool+%7B%0A++++let+matches+%3D+black_box(%22afnrtuUvxN%22)%3B%0A++++matches.contains(c)%0A%7D%0A%0A%23%5Binline(never)%5D%0Apub+fn+f2(c:+char)+-%3E+bool+%7B%0A++++matches!!(c,+!'a!'+%7C+!'f!'+%7C+!'n!'+%7C+!'r!'+%7C+!'t!'+%7C+!'u!'+%7C+!'U!'+%7C+!'v!'+%7C+!'x!'+%7C+!'N!')%0A%7D%0A%0Apub+fn+main()+%7B+%0A++++let+input+%3D+black_box(!'c!')%3B%0A++++black_box(f1(input))%3B%0A++++black_box(f2(input))%3B%0A+%7D%0A'),l:'5',n:'0',o:'Rust+source+%231',t:'0')),k:50,l:'4',n:'0',o:'',s:0,t:'0'),(g:!((h:compiler,i:(compiler:r1860,filters:(b:'0',binary:'1',binaryObject:'1',commentOnly:'0',debugCalls:'1',demangle:'0',directives:'0',execute:'1',intel:'0',libraryCode:'0',trim:'1',verboseDemangling:'0'),flagsViewOpen:'1',fontScale:14,fontUsePx:'0',j:1,lang:rust,libs:!(),options:'-O',overrides:!(),selection:(endColumn:12,endLineNumber:78,positionColumn:12,positionLineNumber:78,selectionStartColumn:10,selectionStartLineNumber:78,startColumn:10,startLineNumber:78),source:1),l:'5',n:'0',o:'+rustc+1.86.0+(Editor+%231)',t:'0')),k:50,l:'4',n:'0',o:'',s:0,t:'0')),l:'2',n:'0',o:'',t:'0')),version:4)).

---

_@robsdedude reviewed on 2025-07-22 17:23_

---

_Review comment by @robsdedude on `crates/ruff_linter/src/rules/ruff/rules/unraw_re_pattern.rs`:279 on 2025-07-22 17:23_

> Is that expected, or should the two be the same?

I'm unsure. The linked code sure is different. If I'm interpreting it right, it's trying to go from `re.sub` to `str.replace`. While in this PR we're trying to go from `re.some_func("some str")` to `re.some_func(r"some str")`. So here we're always staying in the regex world. It's just a question of how many `\`es the string will end up having. My gut feeling is that those cases are not the same. But as said, unsure.

---

_Review comment by @robsdedude on `crates/ruff_linter/src/rules/ruff/rules/unraw_re_pattern.rs`:279 on 2025-07-22 17:29_

> This might be too niche to worry about

Yeah... I didn't want to go there :sweat_smile: I mean it wouldn't be hard to implement, but probably slightly less efficient because the search window would grow from 2 to 4 characters.

I'll leave it up to you to decide if you want me to go for it or not.

---

_@robsdedude reviewed on 2025-07-22 17:29_

---

_@ntBre reviewed on 2025-07-24 15:42_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unraw_re_pattern.rs`:279 on 2025-07-24 15:42_

You've convinced me :) let's stick with this version.

---

_@ntBre approved on 2025-07-24 15:42_

Thanks!

---

_Merged by @ntBre on 2025-07-24 15:45_

---

_Closed by @ntBre on 2025-07-24 15:45_

---

_Branch deleted on 2025-07-24 18:33_

---
