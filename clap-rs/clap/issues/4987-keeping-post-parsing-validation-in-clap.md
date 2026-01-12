```yaml
number: 4987
title: Keeping post parsing validation in clap
type: issue
state: closed
author: Easyoakland
labels:
  - C-enhancement
  - A-validators
assignees: []
created_at: 2023-06-24T23:43:30Z
updated_at: 2023-06-25T00:59:50Z
url: https://github.com/clap-rs/clap/issues/4987
synced_at: 2026-01-12T16:14:16Z
```

# Keeping post parsing validation in clap

---

_@Easyoakland_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

clap = "4.3.8"

### Describe your use case

TLDR: Post process validation function that runs with clap's own parsing to support arbitrary invariant validation.

I have a struct with complex multi-field properties and invariants that should be upheld when finished parsing. An example might looks like:
```rust
#[derive(Debug, Parser)]
struct Cli {
    a: usize,
    b: usize,
    c: usize,
}
```

I would like to be able to validate arbitrary invariants (ex `a+b<100`) on this struct after it has been fully validated and before returning from parse.

I see in the docs [here](https://docs.rs/clap/latest/clap/_derive/_tutorial/index.html#custom-validation) that you can already perform arbitrary validation after the `Parse()` call. However, it seems ugly that you have to include this extra parsing logic in the actual program code instead of defining it on the struct itself.
Its also problematic when integrating the cli with [klask](https://github.com/MichalGniadek/klask). The program runs through to [here](https://github.com/MichalGniadek/klask/blob/c8e5e41f4e4af8dd81cd77825184f1ac5afd4f2f/src/lib.rs#L313-L317C57) and should return an error but currently will continue until the user's main and then finally report the invalid args.

### Describe the solution you'd like

Some way of specifying a value parser `fn(self)->Self` that runs on the entire parsed struct before leaving clap's own validation logic. So `from_arg_matches_mut`, `try_get_matches_from_mut` and the like use this to validate after parsing the whole struct.
Whatever syntax is used for this it should also work for `#[derive(Args)]` so `subcommand`s can also handle their own validated invariants.
For example the following syntax could be created so the following program never panics in the actual user's main. Instead clap should use the `value_parser`=`Self::post_validate`:

```rust
use clap::Parser;

const MAX_VALUE: usize = 100;

#[derive(Clone, Debug, Parser(value_parser=Self::post_validate))]
struct Cli {
    a: usize,
    b: usize,
    c: usize,
}

impl Cli {
    fn post_validate(self) -> Result<Self, String> {
        if self.a + self.b > MAX_VALUE {
            Err(format!(
                "a and b should not add up to more than {MAX_VALUE}"
            ))
        } else {
            Ok(self)
        }
    }
}

fn main() {
    let opt = Cli::parse();
    assert!(
        opt.a + opt.b <= MAX_VALUE,
        "Clap didn't validate all invariants"
    );
    dbg!(opt);
}
```

### Alternatives, if applicable

1. Its not preferred but acceptable if the function must be free and not an associated to the struct.
2. Currently I can only think of accomplishing post parse validation by:
```rust
use clap::{CommandFactory, Parser};

const MAX_VALUE: usize = 100;

#[derive(Clone, Debug, Parser /* (value_parser=Self::post_validate) */)]
struct Cli {
    a: usize,
    b: usize,
    c: usize,
}

impl Cli {
    fn post_validate(self) -> Result<Self, String> {
        if self.a + self.b > MAX_VALUE {
            Err(format!(
                "a and b should not add up to more than {MAX_VALUE}"
            ))
        } else {
            Ok(self)
        }
    }
}

fn handler(error: String) -> ! {
    let mut cmd = Cli::command();
    cmd.error(clap::error::ErrorKind::ArgumentConflict, error)
        .exit()
}

fn main() {
    let opt = Cli::parse().post_validate().map_err(handler).unwrap();
    assert!(
        opt.a + opt.b <= MAX_VALUE,
        "Clap didn't validate all invariants"
    );
    dbg!(opt);
}

```

### Additional Context

The klask link may show that it only supports clap v3 but several forks that support v4 exist ([1](https://github.com/Easyoakland/klask), [2](https://github.com/xosxos/klask))

---

_Label `C-enhancement` added by @Easyoakland on 2023-06-24 23:43_

---

_Comment by @epage on 2023-06-25 00:24_

I'm going to close in favor of #3008 though it will be exposed at the lower levels of clap, rather than at the derive levels.

---

_Closed by @epage on 2023-06-25 00:24_

---

_Label `A-validators` added by @epage on 2023-06-25 00:24_

---

_Comment by @Easyoakland on 2023-06-25 00:52_

So is there a better currently supported way of doing this or should I stick to `let opt = Cli::parse().post_validate().map_err(handler).unwrap();` until #3008 issue is resolved?

---

_Comment by @epage on 2023-06-25 00:59_

Post-parse validation is the current way of doing this

---
