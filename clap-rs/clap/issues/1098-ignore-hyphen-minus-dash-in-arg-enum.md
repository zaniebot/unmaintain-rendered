---
number: 1098
title: "Ignore HYPHEN-MINUS (dash) in arg_enum!()"
type: issue
state: closed
author: behnam
labels: []
assignees: []
created_at: 2017-11-07T22:03:14Z
updated_at: 2018-08-02T03:30:14Z
url: https://github.com/clap-rs/clap/issues/1098
synced_at: 2026-01-10T01:26:43Z
---

# Ignore HYPHEN-MINUS (dash) in arg_enum!()

---

_Issue opened by @behnam on 2017-11-07 22:03_

Ref: <https://docs.rs/clap/2.27.1/clap/macro.arg_enum.html>

Would be great to support ignoring ASCII dash character (HYPHEN-MINUS) in the `FromStr` implementations inside the `arg_enum!()` macro.

The HYPHEN-MINUS character cannot be part of an enum variant name because of the limits of the language, and therefore, this won't be a breaking change to the current behavior, but only expanding acceptable values for values using an arg_enum-defined type.

Optionally, we would also do so for UNDERSCORE (U+005F LOW LINE), since it's recommended to not use them for Enum variant names. BUT, there are cases where it exists.

## Alternate approach
Except alternate strings to match per-variant, so `"some-bar"` can be defined as an alternate string form for `SomeBar` variant explicitly.

This approach gives users all the control, but makes it too verbose for simple, common use-cases.

## Another alternate approach

Having both solutions above, as they don't overlap: staying agile and flexible.

----

### Rust Version

rustc 1.23.0-nightly (e340996ff 2017-11-02)

### Affected Version of clap

2.27.1

### Expected Behavior Summary

Running command with `--foo some-bar` should match `Foo::SomeBar`.

### Actual Behavior Summary

Only `--foo somebar` matches `Foo::SomeBar` and `--foo some-bar` results in an error.

### Steps to Reproduce the issue

Add the above example to a clap app file.

### Sample Code or Link to Sample Code

```rust
arg_enum!{
    #[derive(Debug)]
    pub enum Foo {
        SomeBar,
        AnotherBaz
    }
}
```

### Debug output

N/A

---

_Comment by @kbknapp on 2017-11-09 01:50_

I would be hesitant to simply ignore `-` when matching as this could lead to typos, plus would cause us to match `Foo::SomeBar` to `s-omebar`, `so-mebar`, `som-ebar`, etc.

I would consider `Foo::Some_Bar` allowing `foo_bar` or `foo-bar` and this would be acceptable. Thoughts?

---

_Label `T: RFC / question` added by @kbknapp on 2017-11-09 01:50_

---

_Comment by @behnam on 2017-11-09 19:52_

Well, I see your point, @kbknapp.

I also like to point out that case-insensitive matching already has a similar issue, where value `AbcDef` is equal to `AbCedf`.

Ideally, instead of case-insensitive matching, we would use a case-aware matching, converting the input into CamelCase and perform exact match on the variant names.

`to_camel_case()` is provided by <https://crates.io/crates/case> and similar crates.

I'm not sure how much people rely on the current behavior and how far we can go with breaking compatibility. If we don't want to break anything, maybe we can do both in parallel: case-insensitive AND case-aware matching.

This would address your concern, while not expecting people to use underscores in variant names. And is backwards-compatible.

---

_Referenced in [clap-rs/clap#1103](../../clap-rs/clap/issues/1103.md) on 2017-11-12 19:06_

---

_Referenced in [clap-rs/clap#1037](../../clap-rs/clap/issues/1037.md) on 2017-11-12 19:15_

---

_Comment by @kbknapp on 2017-11-12 19:50_

I'm going to move this over to the `clap-derives`]() repo as I intend on this being solved in 3.x via CustomDerive which will replace this macro call.

---

_Comment by @kbknapp on 2017-11-12 19:51_

This issue was moved to kbknapp/clap-derives#3

---

_Closed by @kbknapp on 2017-11-12 19:51_

---

_Referenced in [clap-rs/clap#1662](../../clap-rs/clap/issues/1662.md) on 2020-02-03 09:29_

---
