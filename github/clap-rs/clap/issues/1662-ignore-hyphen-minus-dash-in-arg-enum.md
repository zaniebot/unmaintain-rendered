---
number: 1662
title: "Ignore HYPHEN-MINUS (dash) in arg_enum!()"
type: issue
state: closed
author: kbknapp
labels:
  - A-derive
assignees: []
created_at: 2017-11-12T19:51:02Z
updated_at: 2020-04-22T18:43:51Z
url: https://github.com/clap-rs/clap/issues/1662
synced_at: 2026-01-07T13:12:19-06:00
---

# Ignore HYPHEN-MINUS (dash) in arg_enum!()

---

_Issue opened by @kbknapp on 2017-11-12 19:51_

_From @behnam on November 7, 2017 22:3_

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

_Copied from original issue: kbknapp/clap-rs#1098_

---

_Comment by @kbknapp on 2017-11-12 19:51_

I would be hesitant to simply ignore `-` when matching as this could lead to typos, plus would cause us to match `Foo::SomeBar` to `s-omebar`, `so-mebar`, `som-ebar`, etc.

I would consider `Foo::Some_Bar` allowing `foo_bar` or `foo-bar` and this would be acceptable. Thoughts?

---

_Comment by @kbknapp on 2017-11-12 19:51_

_From @behnam on November 9, 2017 19:52_

Well, I see your point, @kbknapp.

I also like to point out that case-insensitive matching already has a similar issue, where value `AbcDef` is equal to `AbCedf`.

Ideally, instead of case-insensitive matching, we would use a case-aware matching, converting the input into CamelCase and perform exact match on the variant names.

`to_camel_case()` is provided by <https://crates.io/crates/case> and similar crates.

I'm not sure how much people rely on the current behavior and how far we can go with breaking compatibility. If we don't want to break anything, maybe we can do both in parallel: case-insensitive AND case-aware matching.

This would address your concern, while not expecting people to use underscores in variant names. And is backwards-compatible.

---

_Comment by @kbknapp on 2017-11-12 19:51_

I'm going to move this over to the `clap-derives`]() repo as I intend on this being solved in 3.x via CustomDerive which will replace this macro call.

---

_Label `C: derive macros` added by @pksunkara on 2020-02-03 09:29_

---

_Added to milestone `3.0` by @pksunkara on 2020-02-03 09:29_

---

_Label `W: 3.x` added by @pksunkara on 2020-02-03 09:30_

---

_Label `W: 3.x` removed by @pksunkara on 2020-02-03 09:30_

---

_Label `C: macros` added by @CreepySkeleton on 2020-02-03 09:33_

---

_Label `W: 2.x` added by @CreepySkeleton on 2020-02-03 09:33_

---

_Label `W: 3.x` added by @CreepySkeleton on 2020-02-03 09:33_

---

_Referenced in [clap-rs/clap#1661](../../clap-rs/clap/issues/1661.md) on 2020-02-03 09:37_

---

_Label `C: builder macros` removed by @pksunkara on 2020-04-12 10:29_

---

_Label `C: arg enums` added by @pksunkara on 2020-04-12 10:29_

---

_Closed by @bors[bot] on 2020-04-22 18:24_

---

_Comment by @pksunkara on 2020-04-22 18:43_

Default is [kebab case](https://github.com/clap-rs/clap/blob/c1cb0ce3591e16dd05942fc224565649fd3dfa6b/clap_derive/tests/arg_enum.rs#L42-L69). You can also rename the case into [others](https://github.com/clap-rs/clap/blob/c1cb0ce3591e16dd05942fc224565649fd3dfa6b/clap_derive/tests/arg_enum.rs#L72-L92). Also, there are [aliases](https://github.com/clap-rs/clap/blob/c1cb0ce3591e16dd05942fc224565649fd3dfa6b/clap_derive/tests/arg_enum.rs#L220-L251) if renaming is not enough.

---
