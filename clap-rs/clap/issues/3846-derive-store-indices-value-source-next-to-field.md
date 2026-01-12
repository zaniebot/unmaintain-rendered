```yaml
number: 3846
title: "derive: Store indices, value source next to field values"
type: issue
state: open
author: stepancheg
labels:
  - C-enhancement
  - A-derive
  - S-waiting-on-design
assignees: []
created_at: 2022-06-16T23:25:20Z
updated_at: 2022-11-30T00:38:29Z
url: https://github.com/clap-rs/clap/issues/3846
synced_at: 2026-01-12T16:14:15Z
```

# derive: Store indices, value source next to field values

---

_@stepancheg_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

3.1.8

### Describe your use case

Be able to write something like this:

```rust
#[clap(
  indices,
)]
pub rrrr: Vec<(usize, String)>,
```

Currently to obtain indices, we need to work with `ArgMatches::indices_of` which is hard to work with when a command has subcommands which have other subcommands: it's too easy to miss some subcommand call, and `indices` won't return what's expected (especially when subcommands share flags, but at different nesting level).

### Describe the solution you'd like

.

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @stepancheg on 2022-06-16 23:25_

---

_Label `A-derive` added by @epage on 2022-06-17 03:05_

---

_Label `S-waiting-on-design` added by @epage on 2022-06-17 03:05_

---

_Comment by @epage on 2022-06-17 12:00_

Was just mentioning this to someone else that we need an issue for this.

The other piece of metadata we do not expose in the derive API is the `ValueSource`.

I have some reticence for putting it in the same field due to the complications of parsing and processing more complex types like that.

Another option is to take inspiration from `#[clap(from_global)]` and generalize it to a `#[clap(from(...))]` attribute with
- `global`, like `#[clap((from(global))]`
- `index`, like `#[clap(from(index), id = "foo")]`
  - require an explicit `id`
- `source`, like `#[clap(from(source), id = "foo")]`
  - require an explicit `id`

---

_Renamed from "derive: Store indices next to field values" to "derive: Store indices, value source next to field values" by @epage on 2022-06-17 12:00_

---

_Comment by @stepancheg on 2022-06-17 21:54_

> require an explicit id

That would be great if correctness of id is checked at compile time.

---

_Comment by @epage on 2022-06-17 21:59_

> > require an explicit id
> 
> That would be great if correctness of id is checked at compile time.

Something that could make it easier is #3171.  My main hesitancy about it is I feel like derive-macros generating something besides a trait impl is a bit magical from the users perspective (hard to know what exists and how to interact with it).

---

_Comment by @chipsenkbeil on 2022-07-13 23:24_

Going to copy over my discussion into this issue. 😄 

@epage for deriving value source, I'd personally be happy with having some wrapper type that gets populated in the derive macro. Having some way to have this baked into the struct would enable me to handle #748 manually as I could still provide a default value for all of my cli arguments, but also manually replace them with a config file I've loaded separately just by knowing if the value is a default value. E.g.

```rust
use clap::{Args, Value, parser::ValueSource};

#[derive(Args)]
struct MyArgs {
    /// This has no value source encoded
    #[clap(short, long)]
    pub name: String,

    /// This does have value source encoded
    #[clap(short, long, value_source)]
    pub age: Value<u8>,
}

// when accessing ...
let args: MyArgs = /* ... */;
assert_eq!(args.name, "some name");
assert_eq!(args.age, 37);
assert_eq!(args.age.source, ValueSource::CommandLine);

// if I had a config value I wanted to use only if the arg value came from the default
let age = args.age.replace_if_default(config.age);
assert_eq!(args.age, 28);
```

```rust
use std::ops::{Deref, DerefMut};
use clap::parser::ValueSource;

// Value itself is a wrapper that looks like
#[derive(Copy, Clone)]
struct Value<T> {
    inner: T,
    pub source: ValueSource,
}

impl<T> Value<T> {
    /// Returns the updated value if replaced, otherwise returns the original value
    pub fn replace_if_default(self, inner: T) -> Self {
        if self.source == ValueSource:: DefaultValue {
            Value { inner, source: self.source }
        } else {
            self
        }
    }
}

impl<T> Deref for Value<T> {
    type Target = T;

    fn deref(&self) -> &Self::Target {
        &self.inner
    }
}

impl<T> DerefMut for Value<T> {
    fn deref_mut(&mut self) -> &mut Self::Target {
        &mut self.inner
    }
}

impl<T> From<Value<T>> for T {
    // To enable passing this value around as the actual type
    fn from(value: Value<T>) -> Self {
        value.inner
    }
}

impl<T> PartialEq<T> for Value<T> where T: PartialEq {
    // To enable equality checks
    fn eq(&self, other: &T) -> bool {
        self.inner == other
    }
}

// do this for the other traits ...
```

_Originally posted by @chipsenkbeil in https://github.com/clap-rs/clap/discussions/3578#discussioncomment-3113058_

---

_Comment by @epage on 2022-07-14 02:00_

@chipsenkbeil Thanks for copying that over!

Having a `value_source` attribute helps avoid the challenges with inferring what is happening based on types along (unless we use deref specialization).

It does have a scaling issue.  Some users might want the source while others might want the index or both.  Also the source common to all values while the index is per value.

In exploring the various options, what are your thoughts on the parallel-field idea I had proposed earlier in the thread?

---

_Comment by @chipsenkbeil on 2022-07-24 17:42_

> In exploring the various options, what are your thoughts on the parallel-field idea I had proposed earlier in the thread?

@epage are you referring to the idea of `#[clap(from(index))]` and `#[clap(from(value))]`? It seems fine to me, but I don't have an understanding of index or the `from_global` that you were referencing earlier.

---

_Comment by @epage on 2022-07-25 13:00_

Yes.

`from_global`: clap has the concept of [global arguments](https://docs.rs/clap/latest/clap/builder/struct.Arg.html#method.global) which allows an argument to be defined on the parent command and it will automatically be defined on all subcommands and the value will be propagated back up to the parent command.  Think of flags like those to control logging levels.  `from_global` allows accessing the argument within the subcommand even though it isn't defined in that subcommand.

`index`: clap tracks the relative order that arguments appeared on the command line.  See [ArgMatches::indices_of](https://docs.rs/clap/latest/clap/struct.ArgMatches.html#method.indices_of) for more details.

---

_Comment by @epage on 2022-11-30 00:19_

This issue is unrelated. In the future, feel free to start a Discussion.

In this case, value source is stable / should work. We'll need more details, like a reproduction case. Please create a Discussion with that to not detract from this Issue.

---

_Referenced in [clap-rs/clap#2419](../../clap-rs/clap/issues/2419.md) on 2023-06-14 13:39_

---
