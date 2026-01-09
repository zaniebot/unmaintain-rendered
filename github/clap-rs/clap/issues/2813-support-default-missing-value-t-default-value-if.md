---
number: 2813
title: "Support `default_missing_value_t`, `default_value_if_t`, etc"
type: issue
state: open
author: epage
labels:
  - C-enhancement
  - S-waiting-on-decision
  - A-derive
assignees: []
created_at: 2021-10-05T14:20:57Z
updated_at: 2021-12-13T21:22:41Z
url: https://github.com/clap-rs/clap/issues/2813
synced_at: 2026-01-07T13:12:19-06:00
---

# Support `default_missing_value_t`, `default_value_if_t`, etc

---

_Issue opened by @epage on 2021-10-05 14:20_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

v3.0.0-beta.4

### Describe your use case

`clap_derive` has a `default_value_t` attribute that takes a rust data type, instead of a string, and sets it as `default_value` on the `Arg`.

However, there are related functions that are missing similar `_t` magic methods

### Describe the solution you'd like

Add magic methods for other raw methods that accept a value corresponding to the field the attribute is applied to

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `T: enhancement` added by @epage on 2021-10-05 14:20_

---

_Label `T: new feature` added by @epage on 2021-10-05 14:20_

---

_Label `D: medium` added by @epage on 2021-10-05 14:20_

---

_Label `C: derive macros` added by @epage on 2021-10-05 14:20_

---

_Comment by @marjakm on 2021-11-04 16:00_

As I see it, those methods are all special cases of a pattern that uses a type to generate some code, but why allow only a few special cases not all such patterns?

default_value_t basically generates a call to

```rust
pub struct Arg {
    pub fn default_value(self, val: &'help str) -> Self;
}
```

as something like this:

```rust
arg = Arg::default_value(arg, generate_default_from_type!{T});
```

`generate_default_from_type` is a special macro in generator, adding `default_missing_value_t` support would mean creating a similar macro that also takes the type T. Why only allow this pattern for macros defined in the code generator, why not allow the user of the library to provide the macro (or maybe function) and generate code that provides the type to the user-defined macro (or function).

It could be implemented by allowing the user to annotate like such:

```rust
    struct Opt {
        #[clap(modify_t(my_mod))]
        arg: i32,
    }
    
    fn my_mod<T: Default + serde::Serialize>(a: Arg) -> Arg {
        let val: &'static str = todo!("serialize default T with serde_json, and leak boxed string");
        a.default_value(val)
         .default_value_if(todo!(), todo!(), todo!())
    }
```

Derive crate could generate something like this:

```rust
arg = my_mod::<T>(arg);
```

It would still be useful to have attributes such as `default_value_t` and `default_missing_value_t`, but they could be implemented via having the macros defined as functions somewhere and converting `default_missing_value_t` to `modify_t(generate_default_from_type)`.

---

_Comment by @marjakm on 2021-11-04 16:06_

I actually would like to generate `Arg::about` calls in just this way - the arg parses `Option<ComplexStruct>` via json, the default value should be the empty string, but I'd like to generate an example json in the about.

If you like the above-described solution, I could actually try implementing it and making a pull request

---

_Comment by @epage on 2021-11-04 16:10_

Let's split that out into its own issue to talk about.  This is specifically about providing natively typed magic methods that map to regular methods.

---

_Comment by @epage on 2021-11-04 16:10_

Also of note: we might want to see how https://github.com/clap-rs/clap/issues/2683 impacts this issue.

---

_Referenced in [clap-rs/clap#2990](../../clap-rs/clap/pulls/2990.md) on 2021-11-04 20:20_

---

_Referenced in [clap-rs/clap#2991](../../clap-rs/clap/issues/2991.md) on 2021-11-04 21:33_

---

_Referenced in [epage/clapng#211](../../epage/clapng/issues/211.md) on 2021-12-06 22:15_

---

_Label `T: new feature` removed by @epage on 2021-12-08 21:09_

---

_Label `S-waiting-on-decision` added by @epage on 2021-12-13 21:22_

---

_Label `D: medium` removed by @epage on 2021-12-13 21:22_

---

_Referenced in [clap-rs/clap#3333](../../clap-rs/clap/pulls/3333.md) on 2022-01-24 14:09_

---

_Referenced in [clap-rs/clap#3807](../../clap-rs/clap/issues/3807.md) on 2022-06-09 13:55_

---
