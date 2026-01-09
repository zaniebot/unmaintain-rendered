---
number: 3932
title: "clap_derive: Default value support for Vec of ValueEnums"
type: issue
state: closed
author: emersonford
labels:
  - C-enhancement
assignees: []
created_at: 2022-07-14T03:15:03Z
updated_at: 2022-07-25T21:45:05Z
url: https://github.com/clap-rs/clap/issues/3932
synced_at: 2026-01-07T13:12:20-06:00
---

# clap_derive: Default value support for Vec of ValueEnums

---

_Issue opened by @emersonford on 2022-07-14 03:15_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

3.2.10

### Describe your use case

Suppose you have the following:
```rust
#[derive(clap::ValueEnum, PartialEq, Debug, Clone)]
enum ArgChoice {
    Foo,
    Bar,
}

#[derive(Parser, PartialEq, Debug)]
struct Opt {
    #[clap(value_enum, value_parser)]
    arg: Vec<ArgChoice>,
}
```

providing default values for `arg` doesn't feel very ergonomic to me. As far as I'm aware, you have the following options:
1. `#[clap(value_enum, value_parser, default_value="foo,bar", value_delimiter=',')]` as shown in #3541
    * Doesn't feel super great to me since you're forced into using `value_delimiter` and can't easily set the default to "all variants". Also, if you ever use `rename_all`, you'd have to ensure `default_value` also gets changed.
2. `default_values(...)` with something like
    ```rust
    ArgChoice::value_variants().iter().map(|val| val.to_possible_value().unwrap().get_name()).collect::<Vec<_>>().as_slice()
    ```
    or
    ```rust
    &[ArgChoice::Foo.to_possible_value().unwrap().get_name(), ArgChoice::Foo.to_possible_value().unwrap().get_name()]
    ```
    * Requires a lot of boiler plate and repeated code.






### Describe the solution you'd like

Fixing `default_value_t` so you can do the following:
```rust
#[derive(clap::ValueEnum, PartialEq, Debug, Clone)]
enum ArgChoice {
    Foo,
    Bar,
}

#[derive(Parser, PartialEq, Debug)]
struct Opt {
    #[clap(value_enum, value_parser, default_value_t = vec![ArgChoice::Foo])]
    arg: Vec<ArgChoice>,
}
```

and

```rust
#[derive(clap::ValueEnum, PartialEq, Debug, Clone)]
enum ArgChoice {
    Foo,
    Bar,
}

#[derive(Parser, PartialEq, Debug)]
struct Opt {
    #[clap(value_enum, value_parser, default_value_t = ArgChoice::value_variants().to_vec())]
    arg: Vec<ArgChoice>,
}
```


### Alternatives, if applicable

I could see an argument for adding a `default_values_t` attr, but since `default_value_t` doesn't work to begin with for `Vec<impl ValueEnum>`, I feel like we might as well fix `default_value_t` (plus I personally find it more intuitive, if I used the "typed" version for a Vec, I should be able to pass a Vec).

### Additional Context

`default_value_t` doesn't work in any way:
```rust
#[derive(Parser, PartialEq, Debug)]
struct Opt {
    #[clap(
        value_enum, 
        value_parser, 
        default_value_t = ArgChoice::Foo,
    )]
    arg: Vec<ArgChoice>,
}
```

```
error[[E0308]](https://doc.rust-lang.org/stable/error-index.html#E0308): mismatched types
  --> src/main.rs:14:27
   |
14 |         default_value_t = ArgChoice::Foo,
   |                           ^^^^^^^^^^^^^^ expected struct `Vec`, found enum `ArgChoice`
15 |     )]
16 |     arg: Vec<ArgChoice>,
   |          -------------- expected due to this
   |
   = note: expected struct `Vec<ArgChoice>`
                found enum `ArgChoice`
```

```rust
#[derive(Parser, PartialEq, Debug)]
struct Opt {
    #[clap(
        value_enum, 
        value_parser, 
        default_value_t = vec![ArgChoice::Foo],
    )]
    arg: Vec<ArgChoice>,
}
```

```
error[[E0277]](https://doc.rust-lang.org/stable/error-index.html#E0277): the trait bound `Vec<ArgChoice>: ArgEnum` is not satisfied
  --> src/main.rs:14:9
   |
14 |         default_value_t = vec![ArgChoice::Foo],
   |         ^^^^^^^^^^^^^^^ the trait `ArgEnum` is not implemented for `Vec<ArgChoice>`
   |
   = help: the trait `ArgEnum` is implemented for `ArgChoice`
```

so changing `default_value_t`'s behavior here for a Vec of ValueEnums wouldn't be a breaking change.

---

_Label `C-enhancement` added by @emersonford on 2022-07-14 03:15_

---

_Referenced in [clap-rs/clap#3933](../../clap-rs/clap/pulls/3933.md) on 2022-07-14 03:46_

---

_Comment by @emersonford on 2022-07-14 04:31_

documenting this here from the PR:
> I realized I could apply this for all Vec<impl Display>, not just a Vec<impl ValueEnum>. 

so I added another commit to the PR to fix `default_value_t` for all `Vec` types

---

_Comment by @benny-n on 2022-07-14 15:21_

I realized that I need this feature today and this design is exactly what I had in mind! 
Looking forward to seeing this merged 🙂 

---

_Closed by @emersonford on 2022-07-25 21:45_

---
