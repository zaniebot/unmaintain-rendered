---
number: 5127
title: Default value seems to be cached, resulting in wrong default
type: issue
state: open
author: ctron
labels:
  - C-bug
  - A-derive
  - S-blocked
assignees: []
created_at: 2023-09-15T09:14:57Z
updated_at: 2023-09-15T22:21:29Z
url: https://github.com/clap-rs/clap/issues/5127
synced_at: 2026-01-07T13:12:20-06:00
---

# Default value seems to be cached, resulting in wrong default

---

_Issue opened by @ctron on 2023-09-15 09:14_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.72.0 (5680fa18f 2023-08-23)

### Clap Version

4.4.3

### Minimal reproducible code

```rust
use clap::Parser;
use std::marker::PhantomData;

trait Provider {
    const VALUE: usize;
}

#[derive(Clone, Debug)]
struct A;

impl Provider for A {
    const VALUE: usize = 1;
}

#[derive(Clone, Debug)]
struct B;

impl Provider for B {
    const VALUE: usize = 2;
}

#[derive(Clone, Debug, clap::Args)]
struct Example<T: Provider> {
    #[arg(default_value_t = T::VALUE)]
    pub value: usize,

    #[arg(skip)]
    _marker: PhantomData<T>,
}

#[derive(Clone, Debug, clap::Parser)]
enum Cli {
    A(Example<A>),
    B(Example<B>),
}

fn main() {
    // prints 1, as expected
    //Cli::parse_from(["example", "a", "--help"]);
    // prints 2, bug!
    Cli::parse_from(["example", "b", "--help"]);
}

```


### Steps to reproduce the bug with the above code

Run the above example.

Important is that you have the same structure, where a type argument provides the default value, in more than one branch.

### Actual Behaviour

Initializes with the wrong default value. Shown in help and used as value.

### Expected Behaviour

Use the correct the default value.

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @ctron on 2023-09-15 09:14_

---

_Comment by @ctron on 2023-09-15 09:50_

Here's a test for this:

```rust
#[test]
fn default_values_type_arg() {
    trait DefaultProvider {
        const VALUE: usize;
    }

    #[derive(PartialEq, Debug)]
    struct A;
    #[derive(PartialEq, Debug)]
    struct B;

    impl DefaultProvider for A {
        const VALUE: usize = 1;
    }

    impl DefaultProvider for B {
        const VALUE: usize = 2;
    }

    #[derive(Args, PartialEq, Debug)]
    struct Example<T: DefaultProvider> {
        #[arg(default_value_t = T::VALUE)]
        value: usize,

        #[arg(skip)]
        _marker: PhantomData<T>,
    }

    #[derive(Parser, PartialEq, Debug)]
    enum Cli {
        A(Example<A>),
        B(Example<B>),
    }

    impl Cli {
        fn value(&self) -> usize {
            match self {
                Cli::A(args) => args.value,
                Cli::B(args) => args.value,
            }
        }
    }

    assert_eq!(
        A::VALUE,
        Cli::try_parse_from(["test", "a"]).unwrap().value()
    );
    assert_eq!(
        B::VALUE,
        Cli::try_parse_from(["test", "b"]).unwrap().value()
    );

    let help_a = utils::get_subcommand_long_help::<Cli>("a");
    assert!(help_a.contains("[default: 1]"));
    let help_b = utils::get_subcommand_long_help::<Cli>("b");
    assert!(help_b.contains("[default: 2]"));
}

```

---

_Comment by @ctron on 2023-09-15 09:51_

I think the issue is that default values are created with a static `OnceLock`. Which of course will initialize only once.

---

_Comment by @ctron on 2023-09-15 10:18_

Looks like that's why: https://github.com/rust-lang/rust/issues/22991

---

_Label `A-derive` added by @epage on 2023-09-15 22:21_

---

_Label `S-blocked` added by @epage on 2023-09-15 22:21_

---

_Referenced in [autonomys/subspace#3572](../../autonomys/subspace/pulls/3572.md) on 2025-06-10 10:39_

---
