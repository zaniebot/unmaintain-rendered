```yaml
number: 9229
title: Use CompactStr
type: pull_request
state: closed
author: MichaReiser
labels:
  - performance
  - parser
assignees: []
draft: true
base: main
head: compact_str
created_at: 2023-12-21T10:34:28Z
updated_at: 2024-08-12T07:53:43Z
url: https://github.com/astral-sh/ruff/pull/9229
synced_at: 2026-01-10T21:38:31Z
```

# Use CompactStr

---

_Pull request opened by @MichaReiser on 2023-12-21 10:34_

## Summary
To get a comparison to `SmolStr` which seems to optimize for 

* `O(1)` clone
* Storing strings longer than 23 that soley consist of Whitespace (not a use case for us)

I found the API a bit clumsy but maybe it's just me being confused by `Deref`.

## Test Plan

`cargo test`


---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:847 on 2023-12-21 10:35_

I find this annoying. Not sure why `&CompactStr == &'static str` doesn't work.

@BurntSushi probably knows why

---

_@MichaReiser reviewed on 2023-12-21 10:35_

---

_Comment by @codspeed-hq[bot] on 2023-12-21 10:45_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/compact_str)

### Merging #9229 will **improve performances by 4.81%**

<sub>Comparing <code>compact_str</code> (b60ac75) with <code>main</code> (c6d8076)</sub>



### Summary

`⚡ 2` improvements
`✅ 28` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `compact_str` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `lexer[pydantic/types.py]` | 3.7 ms | 3.6 ms | +4.07% |
| ⚡ | `lexer[large/dataset.py]` | 8.4 ms | 8 ms | +4.81% |


---

_Label `performance` added by @MichaReiser on 2023-12-21 10:50_

---

_Label `parser` added by @MichaReiser on 2023-12-21 10:50_

---

_Comment by @github-actions[bot] on 2023-12-21 10:54_

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

_@BurntSushi reviewed on 2023-12-21 12:59_

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/checkers/ast/mod.rs`:847 on 2023-12-21 12:59_

Not sure off the top of my head, so let's run through it. `x == y` is syntactic sugar for [`std::cmp::PartialEq::eq(&x, &y)`](https://doc.rust-lang.org/reference/expressions/operator-expr.html#comparison-operators). If `id` is a `&CompactString` and `y` is a `&'static str`, then `&x` is actually an `&&CompactString` and `y` is actually a `&&str`. So I believe this means we need to look for a `PartialEq` impl for `&&CompactString` (notice the double `&` there).

The only `PartialEq` impl for `CompactString` is `impl<T: AsRef<str>> PartialEq<T> for CompactString`, which on its own does not fit here because we're looking for `&&CompactString`.

At least, I think that's why. It's possible the `compact-str` crate could add more impls to alleviate this. But maybe not.

Auto-deref also comes into the picture here. See:

* https://stackoverflow.com/questions/72909938/why-operator-requires-dereference-while-eq-does-not
* https://github.com/rust-lang/rust/issues/44762

---

_Comment by @BurntSushi on 2023-12-21 13:00_

I'd also be interested in seeing how `hipstr` fairs. It [appears to have some useful properties](https://github.com/polazarus/hipstr#-similar-crates).

(Sorry, I know there are a billion "small string" crates...)

---

_Comment by @LaBatata101 on 2023-12-21 13:19_

> I'd also be interested in seeing how `hipstr` fairs. It [appears to have some useful properties](https://github.com/polazarus/hipstr#-similar-crates).
> 
> (Sorry, I know there are a billion "small string" crates...)

I can try it later today

---

_Comment by @charliermarsh on 2023-12-21 13:25_

Interesting, the benchmarks look better. (We almost never clone strings so perhaps O(1) clone is not an important tradeoff right now.)

---

_Comment by @MichaReiser on 2023-12-21 15:56_

> I'd also be interested in seeing how `hipstr` fairs. It [appears to have some useful properties](https://github.com/polazarus/hipstr#-similar-crates).
> 
> 
> 
> (Sorry, I know there are a billion "small string" crates...)

Hipstr has interesting properties but it might not work for us without dropping wasm support (playground):

> This crate is only supported on platforms where:
> * pointers have the same memory size has usize

[source](https://docs.rs/hipstr/latest/hipstr/#platform-support)

Wasm is 32 bit if I remember it correctly.

---

_Comment by @BurntSushi on 2023-12-21 16:57_

> Wasm is 32 bit if I remember it correctly.

Yeah but the pointers are 32-bit too right? So that should be fine. I think the note in hipstr is talking about platforms with tagged pointers (like CHERI), which are pretty obscure at present.

---

_Comment by @LaBatata101 on 2023-12-21 19:38_

> I'd also be interested in seeing how `hipstr` fairs. It [appears to have some useful properties](https://github.com/polazarus/hipstr#-similar-crates).
> 
> (Sorry, I know there are a billion "small string" crates...)

The problem with `hipstr` is that we would have to add lifetime annotations to the AST and `Tok` types.

---

_Comment by @MichaReiser on 2023-12-24 01:27_

We'll revisit after merging the hand-written parser.

---

_Closed by @MichaReiser on 2023-12-24 01:27_

---

_Branch deleted on 2024-08-12 07:53_

---
