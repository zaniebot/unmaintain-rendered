```yaml
number: 2991
title: "Derive attribute that specializes a user-provided function that gets to modify `Arg`"
type: issue
state: closed
author: marjakm
labels:
  - C-enhancement
  - A-derive
  - S-wont-fix
assignees: []
created_at: 2021-11-04T21:24:43Z
updated_at: 2022-01-11T18:49:39Z
url: https://github.com/clap-rs/clap/issues/2991
synced_at: 2026-01-12T16:14:14Z
```

# Derive attribute that specializes a user-provided function that gets to modify `Arg`

---

_@marjakm_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

master  879dd23963f6ed968535d6ed7bc6f192f02a0a16

### Describe your use case

I have a bunch of messages defined in protobuf, I'd like to make a CLI to send those messages - `clap` should be able to parse the messages from terminal. I generate rust structs from protobuf definitions using [prost](https://crates.io/crates/prost) and I can automatically add `clap_derive` attributes, but I can't specify the attributes flexibly enough to solve my use-case. 

Problematic cases are when one struct contains another optional struct:

```rust
struct A {
   simple: u32,
   inner: Option<B>
}

struct B {
   a: u32,
   b: String,
}
```

When automatically adding attributes, I can differentiate between protobuf native (u32, bool, ..) types and complex types (another struct), but I don't know if the complex type is `Option<B>` or `Option<C>`.

What I want is to parse these complex types (for example `A::inner`) from from json string from the command line. The default value in this case should be None (empty string), which is simple enough to implement currently. But, I'd also like to generate an example json value for the Some case in the CLI help. But what annotation can I add to the field `A::inner`, where I only know that its a optional struct, but don't know which struct type (B or C) it actually is?

Its really easy for me to write a function to generate the string:

```rust
fn example_json<T: serde::Serialize>(t: T) -> &'static str { todo!() } 
```

All I'd need to have some attribute in `clap_derive` to call that function and I don't need to know which type is `A::inner`

```rust
#[derive(clap::Parser)]
struct A {
   simple: u32,
   #[clap(about = example_json]
   inner: Option<B>
}
```

which should call something like this in the right place:

```rust
Arg::about(arg, example_json::<Option<B>>())
```

`default_value_t` attribute actually does exactly the type-based fn specialization - I'd just like to be able to provide the generic fn and have it specialized by the field type.

### Describe the solution you'd like

I implemented something that would solve my problems here:

https://github.com/clap-rs/clap/pull/2990

### Alternatives, if applicable

_No response_

### Additional Context

Conversation started on #2813

---

_Label `T: new feature` added by @marjakm on 2021-11-04 21:24_

---

_Referenced in [clap-rs/clap#2990](../../clap-rs/clap/pulls/2990.md) on 2021-11-04 21:25_

---

_Comment by @epage on 2021-11-04 21:39_

Thank you! This context helps a lot!

So you are wanting to determine the value for an attribute based on the type of the field it is applied to, with the example being given of dynamically generating `about`, instead of manually doing it, to help with your code-gen use case.

This seems quite specialized.  The first question to answer is if this is needed and then the second is how.

A key part stuck out in your comment

> I don't know if the complex type is `Option<B>` or `Option<C>`.

This is what is leading you to wanting to defer to clap to generate this information.  Could you help me understand this problem a little more?  I'm wanting to learn more about how this clap struct generation is happening to see how all of this fits together and if there is a better, more general solution here.

If we do move forward with this,  `clap::Arg::modify_fn`  doesn't quite make sense on its own, only in the context of `clap_derive`.  I wonder if we can instead do this purely within `clap_derive`.

Its also unclear how https://github.com/clap-rs/clap/issues/2683 might impact any API design on `clap` in the future.

---

_Referenced in [epage/clapng#233](../../epage/clapng/issues/233.md) on 2021-12-06 22:23_

---

_Label `T: new feature` removed by @epage on 2021-12-08 21:08_

---

_Label `C-enhancement` added by @epage on 2021-12-08 21:08_

---

_Label `A-derive` added by @epage on 2021-12-13 22:10_

---

_Label `S-waiting-on-author` added by @epage on 2021-12-13 22:10_

---

_Comment by @epage on 2022-01-11 18:49_

At this time, this seems too specialized.  Going to close in this in favor of other options helping in the future, like #2683 

---

_Closed by @epage on 2022-01-11 18:49_

---

_Label `S-waiting-on-author` removed by @epage on 2022-01-11 18:49_

---

_Label `S-wont-fix` added by @epage on 2022-01-11 18:49_

---
