```yaml
number: 2419
title: Using index_of()/indices_of() with optional arguments that take optional values
type: issue
state: open
author: cptpcrd
labels:
  - C-enhancement
  - A-parsing
  - S-waiting-on-design
assignees: []
created_at: 2021-03-20T17:21:42Z
updated_at: 2023-06-15T19:27:10Z
url: https://github.com/clap-rs/clap/issues/2419
synced_at: 2026-01-12T16:14:13Z
```

# Using index_of()/indices_of() with optional arguments that take optional values

---

_@cptpcrd_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Describe your use case

I have multiple optional arguments with optional values. If several of them were specified *without* passing a value, I want to determine the indices of the arguments so I can find out which one was specified first. However, it seems that this is one of the few cases where `Arg::index_of()`/`Arg::indices_of()` *doesn't* work.

A simple example can be found below.

### Describe the solution you'd like

Ideally, `Arg::index_of()` would work for arguments with optional values.

### Alternatives, if applicable

Perhaps a new method, say `Args::index_of_opt()` (which would return the index of the flag like `-a` or --all`, instead of the value) could be added?

### Additional Context

I'm using `structopt`, but I've translated this to `clap` code.

Example with clap 2.33:

```rust
fn main() {
    use clap::{Arg, App};
    
    let app = App::new("Basic Test")
        .arg(Arg::with_name("abc")
            .short("a")
            .long("abc")
            .takes_value(true)
            .multiple(false)
            .min_values(0)
            .max_values(1));

    // I can get the index in all of these cases (because clap returns the index of the value)

    let matches = app.clone().get_matches_from(&["", "-a1"]);
    println!("{:?}", matches.index_of("abc"));

    let matches = app.clone().get_matches_from(&["", "--abc=1"]);
    println!("{:?}", matches.index_of("abc"));

    let matches = app.clone().get_matches_from(&["", "-a", "1"]);
    println!("{:?}", matches.index_of("abc"));

    let matches = app.clone().get_matches_from(&["", "--abc", "1"]);
    println!("{:?}", matches.index_of("abc"));

    // But I can't get the index in these cases (which is when I really need it!)

    let matches = app.clone().get_matches_from(&["", "-a"]);
    println!("{:?}", matches.index_of("abc"));

    let matches = app.clone().get_matches_from(&["", "--abc"]);
    println!("{:?}", matches.index_of("abc"));
}
```

This produces the following output:
```
Some(2)
Some(2)
Some(2)
Some(2)
None
None
```

<details>
<summary>Updated for clap 3 (produces the same output as the above version when run against the latest master):</summary>

```rust
fn main() {
    use clap::{Arg, App};
    
    let app = App::new("Basic Test")
        .arg(Arg::new("abc")
            .short('a')
            .long("abc")
            .takes_value(true)
            .multiple(false)
            .min_values(0)
            .max_values(1));

    // I can get the index in all of these cases (because clap returns the index of the value)

    let matches = app.clone().get_matches_from(&["", "-a1"]);
    println!("{:?}", matches.index_of("abc"));

    let matches = app.clone().get_matches_from(&["", "--abc=1"]);
    println!("{:?}", matches.index_of("abc"));

    let matches = app.clone().get_matches_from(&["", "-a", "1"]);
    println!("{:?}", matches.index_of("abc"));

    let matches = app.clone().get_matches_from(&["", "--abc", "1"]);
    println!("{:?}", matches.index_of("abc"));

    // But I can't get the index in these cases (which is when I really need it!)

    let matches = app.clone().get_matches_from(&["", "-a"]);
    println!("{:?}", matches.index_of("abc"));

    let matches = app.clone().get_matches_from(&["", "--abc"]);
    println!("{:?}", matches.index_of("abc"));
}
```
</details>

---

_Label `T: new feature` added by @cptpcrd on 2021-03-20 17:21_

---

_Added to milestone `3.0` by @ldm0 on 2021-03-20 18:21_

---

_Removed from milestone `3.0` by @pksunkara on 2021-03-20 21:45_

---

_Added to milestone `3.1` by @pksunkara on 2021-03-20 21:45_

---

_Label `C: matches` added by @pksunkara on 2021-03-20 21:45_

---

_Referenced in [epage/clapng#183](../../epage/clapng/issues/183.md) on 2021-12-06 21:14_

---

_Label `C: matches` removed by @epage on 2021-12-08 20:21_

---

_Label `A-parsing` added by @epage on 2021-12-08 20:21_

---

_Label `T: new feature` removed by @epage on 2021-12-08 21:10_

---

_Label `C-enhancement` added by @epage on 2021-12-08 21:10_

---

_Comment by @epage on 2021-12-13 16:36_

Currently, we only support indices for values, not arguments.  This will require some design work for what the API / behavior should be.

>  If several of them were specified without passing a value, I want to determine the indices of the arguments so I can find out which one was specified first.

Could you elaborate on your use case for why the order they are set matters?  Its helpful to have the wider context on these types of things.

> I'm using structopt, but I've translated this to clap code.

How are you getting the index with structopt?



---

_Removed from milestone `3.1` by @epage on 2021-12-13 16:36_

---

_Label `S-waiting-on-design` added by @epage on 2021-12-13 16:36_

---

_Comment by @cptpcrd on 2021-12-13 22:34_

> Could you elaborate on your use case for why the order they are set matters? Its helpful to have the wider context on these types of things.

I'm trying to match the behavior of a C program where the order matters. That isn't terribly uncommon for C programs; e.g. `tar -C`.

> How are you getting the index with structopt?

Something like this (see [docs](https://docs.rs/structopt/0.3.23/structopt/trait.StructOpt.html#required-methods)):

```
use structopt::StructOpt;

#[derive(Debug, StructOpt)]
struct Opt {
    #[structopt(short, long)]
    debug: bool,
}

fn main() {
    let matches = Opt::clap().get_matches_from(&["", "-d"]);
    let opt = Opt::from_clap(&matches);
    println!("{:?}", opt);
    println!("{:?}", matches.index_of("debug"));
}
```

---

_Comment by @fabianfreyer on 2023-05-15 16:33_

I'm running into a related issue (https://github.com/clap-rs/clap/discussions/4908). In my use case there are multiple configuration layers that override each other (e.g. `--config-file /path/to/file --env --url https://example.com/`, where ordering of each option matters. One of these options takes an optional argument. In this case, `index_of` doesn't return the index of the argument.

@epage:
> Currently, we only support indices for values, not arguments. This will require some design work for what the API / behavior should be.

Is this still true? The docs say:

> For flags (i.e. those arguments which don’t have an associated value), indices refer to occurrence of the switch, such as -f, or --flag. However, for options the indices refer to the values -o val would therefore not represent two distinct indices, only the index for val would be recorded. This is by design.

FWIW, this would be the behaviour I'd expect:
```rust
use clap;

fn main() {
    use clap::{Arg, Command};
    
    let app = Command::new("Basic Test")
        .arg(Arg::new("optional")
            .short('o')
            .num_args(0..=1))
        .arg(Arg::new("flag")
            .short('f')
            .num_args(0))
        .arg(Arg::new("value")
            .short('v')
            .num_args(1));

    let matches = app.clone().get_matches_from(&["", "-f"]);
    assert_eq!(matches.index_of("flag"), Some(1));

    let matches = app.clone().get_matches_from(&["", "-v", "42"]);
    assert_eq!(matches.index_of("value"), Some(2));

    let matches = app.clone().get_matches_from(&["", "-o", "42"]);
    assert_eq!(matches.index_of("optional"), Some(2));
    
    let matches = app.clone().get_matches_from(&["", "-o"]);
    assert_eq!(matches.index_of("optional"), Some(1)); // Fails, returns None.
}
```

I've drafted up PR #4909 to implement this.

---

_Referenced in [clap-rs/clap#4909](../../clap-rs/clap/pulls/4909.md) on 2023-05-15 17:34_

---

_Comment by @fabianfreyer on 2023-05-16 11:18_

Perhaps opening a PR was the wrong thing to do here -- what would be the best way to help move this forward?

---

_Comment by @epage on 2023-05-17 21:40_

> Is this still true? The docs say:

Nuance: it is true for `ArgAction::Set` and `ArgAction::Append`.  It is not true for `ArgAction::SetTrue`.

When you do `num_args(0..=1)`, that is still an `ArgAction::Set` even if it still sometimes acts like `ArgAction::SetTrue`.

> FWIW, this would be the behaviour I'd expect:

That works for you, but what about others? Users are relying on a mapping between values and indices.

> Perhaps opening a PR was the wrong thing to do here -- what would be the best way to help move this forward?

Examine the relevant use cases for indices and come up with a proposal that meets them.  If its a breaking change, evaluate if there is a way to opt into it now or whether we have to wait on clap v5.0.0 for this.

---

_Comment by @fabianfreyer on 2023-06-14 01:52_

Right, so, use cases. There seem to be two disjunct use cases here:
1. Keeping track of the order of _values_: this is currently supported, and there is a 1:1 mapping between an `ArgMatch`'s values (e.g. via `ArgMatches::get_many`), which do not include any occurrences of an `Arg` with no value.
    ```
    --arg --arg foo --arg bar
    ```
    Given the above arguments, the expected behavior would be:
    ```toml
    values = ["foo", "bar"]
    indices = [1, 2]
    ```
2. Keeping track of an `Arg`'s _occurrences_, independently of whether any values are passed: this is what was proposed by #4909. In this case, there would need to be a 1:1 mapping between the list of indices and the list of occurrences, e.g.:
    ```
    --arg --arg foo --arg bar
    ```
    Given the above arguments, the expected behavior would be:
    ```toml
    occurrences = [[], ["foo"], ["bar"]]
    indices = [1, 2, 3]
    ```

I currently see the following ways forward:
1. Introduce a separate index, e.g. `ArgMatches::ocurrence_indices()`. Pro: This would allow opt-in to this functionality without breaking changes. Contra: It's confusing and clutters `ArgMatches`.
2. Implement functionality as proposed in #4909. This would be a breaking change. An argument can be made that users who allow optional arguments should use `occurrences` to handle the cases they explicitely allow. This could be gated behind a feature flag which is removed in clap v5.0. \
**Pro**: The code for this is already around, and it's trivial to adapt.
**Contra**: Asking users to use `ocurrences` to iterate over optional values may be confusing.
3. Enforce downcasting in `ArgMatches::get_many` and similar functions  for optional argument values to types that support emptiness, e.g. `Option` (`num_args(0..=1)`) and/or iterable types such as `Vec` (`num_args(0..n)`). This would be a breaking change, which could be gated behind a feature flag which is removed in clap v5.0. \
**Pro**: indices would map directly to ocurrences _and_ values, and values for optional arguments are inherently optional (`Option` or empty Vecs etc), leading to more idiomatic code. \
**Contra**: Arguably the most breaking change.

A mixture of these approaches could also be possible, e.g. going with 1.) for clap v4 and implementing 3.) for clap v5 and feature-gating it for clap v4, then deprecating 1.) in clap v5.

I'd be curious to see what aspects I missed, because I'm sure there are some.

---

_Comment by @epage on 2023-06-14 13:39_

For some extra context, we did not expose grouping-by-occurrence until clap v4.1.0 (see #2924), so index per occurrence was not possible until then.

An extra challenge in all of this is #3846.  Would occurrence indexes (whether replacing value indexes or parallel to it) make that issue harder, easier, or no difference?

---

_Comment by @fabianfreyer on 2023-06-15 19:27_

TL;DR:
Ocurrence indices don't make #3846 harder. They do make it easier _if_ vectors of optional values are considered. Otherwise, there is no difference.

Long form:
I think that would depend on details. This issue is about having a mapping of indices to _ocurrences_, whereas #3846 is about having a mapping of values to their source locations (and hence indices). While not every value can be assigned an index (e.g. what index would a default value have?), if we constrain that to values set by arguments, then every value should be attributable to a specific occurrence of an argument, and hence, an index. 

Current value indices form a subset of the proposed occurrence indices, every value is part of an ocurrence, but not all ocurrences have values:
```
                --arg --arg foo --arg bar
value idx:                  ^0        ^1
occurrence idx:      ^0     ^1        ^2
```
Therefore, using ocurrence indices instead of value indices definitely don't make it harder to find an index for every value (i.e. #3846).

Now consider (since I'm not sure how to do `num_args` via derive) lists of optional values, e,g.:
```rust
#[derive(Args)]
struct MyArgs {
    // --arg1 --arg1=42 -> vec![None, Some(42)]
    #[clap(short, long, value_source)]
    pub arg1: Vec<Value<Option<u8>>>,

    // --arg2 --arg2 foo --arg2 foo bar -> vec![vec![], vec!["foo"], vec!["foo", "bar"]]
    #[clap(short, long, value_source)]
    pub arg1: Vec<Value<Vec<String>>>,
}
```
In these cases, finding an index to assign to the empty values would be possible using occurrence indices, but not possible using value indices. (i.e. making #3864 easier) However, since args taking a variable number of values seem to not be supported like that using derive (unless I'm mistaken?) at the moment, I'd say at the moment it doesn't make a difference.

---
