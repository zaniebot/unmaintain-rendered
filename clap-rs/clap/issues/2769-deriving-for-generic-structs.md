---
number: 2769
title: Deriving for generic structs
type: issue
state: closed
author: kuviman
labels:
  - A-derive
assignees: []
created_at: 2021-09-15T07:27:00Z
updated_at: 2021-11-16T16:12:49Z
url: https://github.com/clap-rs/clap/issues/2769
synced_at: 2026-01-10T01:27:25Z
---

# Deriving for generic structs

---

_Issue opened by @kuviman on 2021-09-15 07:27_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

3.0.0-beta.4

### Describe your use case

I would like to have a library with most common cli arguments, and then allow the main crate to specify additional, like:

```
#[derive(Clap)]
struct GenericArgs<T: Clap> {
    #[clap(long)]
    something_generic: Option<String>,
    #[clap(flatten)]
    app_args: T,
}
```

The derive macro currently does not support generic structs though

### Describe the solution you'd like

The easiest solution and one that works for me would be inserting just generic args as they were in the definition into the implementation.

As you see, I have put `T: Clap` trait bound in the struct definition since it will be needed in the implementation.

### Alternatives, if applicable

Other possibility is adding those trait bounds in the implementation, so that they are not needed in struct definition. Although this would probably not work for my usecase as I'm actually not using `T` directly, and instead use `<T as my::GenericAppTrait>::Args` that implements `Clap`, so putting a bound on `T` would not work

### Additional Context

_No response_

---

_Label `T: new feature` added by @kuviman on 2021-09-15 07:27_

---

_Label `C: derive macros` added by @pksunkara on 2021-09-18 16:26_

---

_Comment by @pksunkara on 2021-09-18 16:27_

Note: Structopt did some work towards this after we forked it

---

_Referenced in [clap-rs/clap#2809](../../clap-rs/clap/issues/2809.md) on 2021-10-11 19:07_

---

_Comment by @epage on 2021-10-11 19:16_

Structopt issue: https://github.com/TeXitoi/structopt/issues/128
Structopt PR: https://github.com/TeXitoi/structopt/pull/483

Unlike a lot of the [other ports from structopt](https://github.com/clap-rs/clap/issues/2809). this one needs a major re-work.  Structopt only has 1 trait, so they can just tack that one trait bound on everything.  We have several traits and have to select trait bounds according to a generic parameters usage.

I recommend we follow the approach of [serde](https://serde.rs/attr-bound.html)
- By default infer trait bounds from the fields that use them
  - e.g. `#[clap(flatten)] foo: T` would add a `T: Args` bound while `#[clap(subcommand)] foo: T` would add a `T: Subcommand` bound
- Add a `bounds` attribute for overriding inferred bounds

These can be split into two different PRs with the highest priority probably on the inferring.

---

_Referenced in [cucumber-rs/cucumber#134](../../cucumber-rs/cucumber/issues/134.md) on 2021-10-19 13:27_

---

_Referenced in [clap-rs/clap#3023](../../clap-rs/clap/pulls/3023.md) on 2021-11-14 09:44_

---

_Comment by @kuviman on 2021-11-14 10:09_

@epage

I believe there is a major difference between serde and clap in terms of how inferring bounds should work

Let's say we have a struct

```rust
struct Foo<T> {
    inner: Inner<T>,
}
```

In case of serde a trait bound `T: Serialize` is being inserted because `T` is contained in `Inner<T>`.
They do it because most likely there is `impl<T: Serialize> Serialize for Inner<T>`

But, in case of clap, I am not so sure that this is the most likely implementation. And, like in your example, we should probably insert `Inner<T>: Subcommand` instead of `T: Subcommand`. If we do otherwise, we can add incorrect bound. Because maybe there is actually an `impl<T: Args> Subcommand for Inner<T>`.

Another thing is that types implementing serde do happen to still be useful when trait bounds are not met. You cannot serialize them, but you can do something.

In case of types made for command line argument parsing, I believe their primary and only usecase is parsing command line arguments. We probably want to know if the trait is implemented properly as soon as possible, and that is when the type is defined. We want the type to implement `Args`/`Parser`/`Subcommand` not only sometimes, but always.

So, to me it makes more sense to put bounds on the type directly.

That being said, I have implemented basic generic support (just copy-pasting type's bounds to the impl) in #3023
Looks like structopt folks did the same

---

_Comment by @epage on 2021-11-15 15:12_

> We have several traits and have to select trait bounds according to a generic parameters usage.

I wish I could remember what I ran into with my port of the structopt work.  I see that structopt adds a trait, `StructOptInternal`, but that isn't relevant to us.  The closest relevant situation is `FromArgMatches`.  Maybe I forgot `FromArgMatches` was a supetrait for `Args` / `Subcommand`?

---

_Referenced in [clap-rs/clap#3032](../../clap-rs/clap/issues/3032.md) on 2021-11-16 15:07_

---

_Closed by @bors[bot] on 2021-11-16 16:12_

---
