---
number: 3185
title: "use `default_value_t` with `ArgEnum` without manually implementing `Display`"
type: issue
state: closed
author: danieleades
labels:
  - C-enhancement
  - A-derive
  - S-waiting-on-design
assignees: []
created_at: 2021-12-16T10:12:56Z
updated_at: 2022-02-13T02:46:57Z
url: https://github.com/clap-rs/clap/issues/3185
synced_at: 2026-01-10T01:27:35Z
---

# use `default_value_t` with `ArgEnum` without manually implementing `Display`

---

_Issue opened by @danieleades on 2021-12-16 10:12_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

master

### Describe your use case

deriving argenums with default values is currently a little clumsy, since you have to manually implement `Display`, and you need to manually ensure that the `Display` implementation is aligned with the derived `ArgEnum` implementation


for example

```rust
#[derive(Debug, Parser)]
pub struct List {

    #[clap(long, arg_enum, default_value_t]
    variant: Variant
}

#[derive(Debug, Clone, Copy, ArgEnum)]
enum Variant {
    ObjectA,
    ObjectB
}

impl std::fmt::Display for Variant {
    fn fmt(&self, f: &mut std::fmt::Formatter<'_>) -> std::fmt::Result {
        let s = match self {
            Self::ObjectA => "object-a", // or is it "object_a"?
            Self::ObjectB => "object-b",
        };
        write!(f, "{}", s)
    }
}
```

what i'd *like* is to omit the manual `Display` implementation

### Describe the solution you'd like

`#[derive(ArgEnum)]` should generate a `Display` implementation which is consistent with the input values that it accepts

### Alternatives, if applicable

- `clap` provide a `Display` derive that is implemented in terms of `ArgEnum`
- Specialize `default_value_t` support for `ArgEnum`

### Additional Context

one possible edge case is that the derived `Display` implementation could conflict with another `Display` implementation (in which case `default_value_t` shouldn't be used anyway!)

therefore the generated `Display` implementation could have a flag to opt in or out (i would vote for making it opt out)

---

_Label `C-enhancement` added by @danieleades on 2021-12-16 10:12_

---

_Renamed from "#[derive(ArgEnum) should add a `ToString` implementation" to "Simplify deriving of `Display` for `ArgEnum` to be used as `default_value_t`" by @epage on 2021-12-16 14:24_

---

_Label `A-derive` added by @epage on 2021-12-16 14:25_

---

_Label `S-waiting-on-design` added by @epage on 2021-12-16 14:25_

---

_Comment by @epage on 2021-12-16 14:29_

Your `Display` can be simplified to
```rust
    impl std::fmt::Display for ArgChoice {
        fn fmt(&self, f: &mut std::fmt::Formatter<'_>) -> Result<(), std::fmt::Error> {
            std::fmt::Display::fmt(self.to_possible_value().unwrap().get_name(), f)
        }
    }
```

At one point, we had an example of this but it looks like we dropped it as we iterated on it and only a [test](https://github.com/clap-rs/clap/blob/master/tests/derive/arg_enum.rs) remains.

I've added to the issue another alternative, we provide our own derive for `Display` that provided the above code snippet.  This makes the behavior opt-in and we can provide the appropriate cautions (will `panic` on skipped fields).

The main question I have it how do we then expose this.  Is this top-level in the crate?  Should we have this in a module?

---

_Comment by @danieleades on 2021-12-16 14:41_

> Your `Display` can be simplified to
> 
> ```rust
>     impl std::fmt::Display for ArgChoice {
>         fn fmt(&self, f: &mut std::fmt::Formatter<'_>) -> Result<(), std::fmt::Error> {
>             std::fmt::Display::fmt(self.to_possible_value().unwrap().get_name(), f)
>         }
>     }
> ```
> 
> At one point, we had an example of this but it looks like we dropped it as we iterated on it and only a [test](https://github.com/clap-rs/clap/blob/master/tests/derive/arg_enum.rs) remains.
> 
> I've added to the issue another alternative, we provide our own derive for `Display` that provided the above code snippet. This makes the behavior opt-in and we can provide the appropriate cautions (will `panic` on skipped fields).
> 
> The main question I have it how do we then expose this. Is this top-level in the crate? Should we have this in a module?

ah good to know.

makes me think that `default_value_t` on an `arg_enum` shouldn't require `Display` at all, it should just call `ArgEnum::to_possible_value`

---

_Comment by @danieleades on 2021-12-16 14:47_

maybe we should update the name of this issue again? It's not really about making it easier to derive `Display`, it's about not having to manually implement `Display`. That could *either* be by deriving it, or by not needing it at all.

---

_Renamed from "Simplify deriving of `Display` for `ArgEnum` to be used as `default_value_t`" to "Simplify using `default_value_t` with `ArgE" by @epage on 2021-12-16 14:48_

---

_Renamed from "Simplify using `default_value_t` with `ArgE" to "Simplify using `default_value_t` with `ArgEnum`" by @epage on 2021-12-16 14:48_

---

_Renamed from "Simplify using `default_value_t` with `ArgEnum`" to "use `default_value_t` with `ArgEnum` without manually implementing `Display`" by @danieleades on 2021-12-16 14:48_

---

_Comment by @epage on 2021-12-16 14:48_

> makes me think that `default_value_t` on an `arg_enum` shouldn't require `Display` at all, it should just call `ArgEnum::to_possible_value`

That seems like the simplest, low-design route.

---

_Comment by @epage on 2021-12-16 14:56_

Hmm, the main annoyance is our current implementation.  In one pass we parse the attributes and turn them into method calls but checking for `arg_enum` when generating the default requires knowledge of all of the attributes which we don't have when generating the method call.

---

_Comment by @danieleades on 2021-12-16 15:02_

> Hmm, the main annoyance is our current implementation. In one pass we parse the attributes and turn them into method calls but checking for `arg_enum` when generating the default requires knowledge of all of the attributes which we don't have when generating the method call.

have you got a link to the relevant code?

---

_Referenced in [clap-rs/clap#3188](../../clap-rs/clap/pulls/3188.md) on 2021-12-16 15:12_

---

_Comment by @epage on 2021-12-16 15:12_

See https://github.com/clap-rs/clap/pull/3188

---

_Closed by @epage on 2021-12-16 15:31_

---

_Comment by @jamesmcm on 2022-02-12 13:50_

Why was the automatic Display implementation dropped btw? This was useful with StructOpt and the arg_enum! macro previously.

---

_Comment by @epage on 2022-02-13 02:46_

> Why was the automatic Display implementation dropped btw? This was useful with StructOpt and the arg_enum! macro previously.

While I wasn't around for that decision, I agree with not providing an `impl Dsplay`:
- Its clearer when it does exactly what you tell it to do (derive the `ArgEnum` trait) rather than doing more
- It composes better with whatever else the user might be doing

As shown earlier, its relatively easy to still derive `Display` (and `FromStr`).

---
