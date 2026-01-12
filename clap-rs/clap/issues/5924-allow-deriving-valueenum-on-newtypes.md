```yaml
number: 5924
title: Allow deriving ValueEnum on newtypes
type: issue
state: closed
author: edward-shen
labels:
  - C-enhancement
  - A-derive
assignees: []
created_at: 2025-02-21T19:53:43Z
updated_at: 2025-02-21T20:12:17Z
url: https://github.com/clap-rs/clap/issues/5924
synced_at: 2026-01-12T16:14:17Z
```

# Allow deriving ValueEnum on newtypes

---

_@edward-shen_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.5.20

### Describe your use case

Given an enum defined in a library:

```rust
// In my_lib
enum Foo {
    Bar,
}
```

I would like to directly generate clap bindings for this in my binary, _without_ having the library depend on clap:

```rust
// in my_cli
#[derive(Debug, Parser)]
struct Args {
    #[clap(long, value_enum)]
    foo: Foo,
}
```

This allows the library to not have a clap dependency while making it easy for CLIs to directly pass an enum to a library.

### Describe the solution you'd like

The orphan rule prevents us from implementing traits on foreign types, but I was hoping for something we can permit deriving the ValueEnum trait on newtype patterns, where the implementation of ValueEnum on this new type is manually implemented on the inner type:

```rust
// in my_cli
#[derive(Debug, clap::ValueEnum)] // Permitted, because CliFoo is a newtype.
struct CliFoo(Foo);

#[derive(Debug, Parser)]
struct Args {
    #[clap(long, value_enum)]
    foo: CliFoo,
}
```

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @edward-shen on 2025-02-21 19:53_

---

_Comment by @epage on 2025-02-21 20:12_

The problem is that the `derive` machinery only lets `ValueEnum` see the struct it is being applied to and not the foreign enum.  All we could do is allow forwarding of the `ValueEnum` trait to the inner type but that means the inner type has to implement `ValueEnum` which defeats the purpose.

As I'm not seeing anything we could do about this, I'm going to go ahead and close this.  If there is a reason for us to reconsider, let us know!

---

_Closed by @epage on 2025-02-21 20:12_

---

_Label `A-derive` added by @epage on 2025-02-21 20:12_

---
