```yaml
number: 2641
title: "Unexpected behavior combining `AppSettings::TrailingVarArg` and `env` fallbacks"
type: issue
state: closed
author: eadwu
labels:
  - C-bug
assignees: []
created_at: 2021-07-29T21:58:54Z
updated_at: 2021-07-30T09:31:41Z
url: https://github.com/clap-rs/clap/issues/2641
synced_at: 2026-01-12T16:14:13Z
```

# Unexpected behavior combining `AppSettings::TrailingVarArg` and `env` fallbacks

---

_@eadwu_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.56.0-nightly (b70888601 2021-07-28)

### Clap Version

2.33.3

### Minimal reproducible code

```rust
use structopt::{clap::AppSettings, StructOpt};

#[derive(StructOpt, Debug)]
#[structopt(
    setting = AppSettings::TrailingVarArg,
)]
struct Arguments {
    #[structopt(env = "SHELL", raw(true), multiple(true), use_delimiter(false))]
    cmd: Vec<String>,
}

pub fn main() {
    let args = Arguments::from_args();

    println!("{:?}", args);
}
```


### Steps to reproduce the bug with the above code

```bash
❯  result/bin/binary --
❯  result/bin/binary -- $SHELL
❯  result/bin/binary -- $SHELL -i
❯  result/bin/binary -- -i
❯  result/bin/binary -- -i $SHELL
```

### Actual Behaviour

```bash
❯  result/bin/binary --
Arguments { cmd: ["/nix/store/pfk6bllfmp7k939wwd6l3iycb500mvz2-zsh-5.8/bin/zsh"] }
❯  result/bin/binary -- $SHELL
Arguments { cmd: ["/nix/store/pfk6bllfmp7k939wwd6l3iycb500mvz2-zsh-5.8/bin/zsh", "/nix/store/pfk6bllfmp7k939wwd6l3iycb500mvz2-zsh-5.8/bin/zsh"] }
❯  result/bin/binary -- $SHELL -i
Arguments { cmd: ["/nix/store/pfk6bllfmp7k939wwd6l3iycb500mvz2-zsh-5.8/bin/zsh", "-i", "/nix/store/pfk6bllfmp7k939wwd6l3iycb500mvz2-zsh-5.8/bin/zsh"] }
❯  result/bin/binary -- -i
Arguments { cmd: ["-i", "/nix/store/pfk6bllfmp7k939wwd6l3iycb500mvz2-zsh-5.8/bin/zsh"] }
❯  result/bin/binary -- -i $SHELL
Arguments { cmd: ["-i", "/nix/store/pfk6bllfmp7k939wwd6l3iycb500mvz2-zsh-5.8/bin/zsh", "/nix/store/pfk6bllfmp7k939wwd6l3iycb500mvz2-zsh-5.8/bin/zsh"] }
```

### Expected Behaviour

```bash
❯  result/bin/binary --
Arguments { cmd: ["/nix/store/pfk6bllfmp7k939wwd6l3iycb500mvz2-zsh-5.8/bin/zsh"] }
❯  result/bin/binary -- $SHELL
Arguments { cmd: ["/nix/store/pfk6bllfmp7k939wwd6l3iycb500mvz2-zsh-5.8/bin/zsh"] }
❯  result/bin/binary -- $SHELL -i
Arguments { cmd: ["/nix/store/pfk6bllfmp7k939wwd6l3iycb500mvz2-zsh-5.8/bin/zsh", "-i"] }
❯  result/bin/binary -- -i
Arguments { cmd: ["-i"] }
❯  result/bin/binary -- -i $SHELL
Arguments { cmd: ["-i", "/nix/store/pfk6bllfmp7k939wwd6l3iycb500mvz2-zsh-5.8/bin/zsh"] }
```

### Additional Context

https://github.com/TeXitoi/structopt/issues/487

`default_value = "SHELL"` works fine though

```bash
❯  result/bin/binary --
Arguments { cmd: ["SHELL"] }
❯  result/bin/binary -- $SHELL
Arguments { cmd: ["/nix/store/pfk6bllfmp7k939wwd6l3iycb500mvz2-zsh-5.8/bin/zsh"] }
❯  result/bin/binary -- $SHELL -i
Arguments { cmd: ["/nix/store/pfk6bllfmp7k939wwd6l3iycb500mvz2-zsh-5.8/bin/zsh", "-i"] }
❯  result/bin/binary -- -i
Arguments { cmd: ["-i"] }
❯  result/bin/binary -- -i $SHELL
Arguments { cmd: ["-i", "/nix/store/pfk6bllfmp7k939wwd6l3iycb500mvz2-zsh-5.8/bin/zsh"] }
```

### Debug Output

_No response_

---

_Label `T: bug` added by @eadwu on 2021-07-29 21:58_

---

_Comment by @epage on 2021-07-30 01:07_

Just taking notes on this:
- When you call `Arg::env`, we read the variable at that time and store both name and value
- `Validator::validate` calls `add_env` and then immediately `add_default`
  - `add_env` loops through all args
    - If it takes a value, we call `add_single_val`
    - If it doesn't, we first check if the env var's value is help or version?  That is surprising.  Then we call `add_index_to`.  I'm, assuming this just marks it as present
  - `add_default` loops through `get_opts` and then `get_positionals`
    - `add_value`

What stands out for this particular issue is:
```rust
            // Use env only if the arg was not present among command line args
            if matcher.get(&a.id).map_or(true, |a| a.occurs == 0) {
...
            if matcher.get(&arg.id).is_some() {
                debug!("Parser::add_value:iter:{}: was used", arg.name);
```

They are checking for what is "present" in different ways.  Seems like we should unify this.


(note: all of this was researched on clap3, I've not verified if this problem still exists on clap3 so this is all speculation so far)

---

_Comment by @epage on 2021-07-30 01:33_

Looks like this **is** fixed in clapv3

clapv2
```rust

#[test]
fn trailing_var_arg() {
    let env_name = "CLP_TEST_TRAILING_VAR_ARG";
    let env_value = "zsh";
    env::set_var(env_name, env_value);
    let app = App::new("df")
        .setting(clap::AppSettings::TrailingVarArg)
        .arg(
            Arg::with_name("cmd")
                .env(env_name)
                .takes_value(true)
                .multiple(true)
                .use_delimiter(false),
        );

    let m = app.clone().get_matches_from_safe(vec!["df", "--"]).unwrap();
    assert!(m.is_present("cmd"));
    assert_eq!(m.occurrences_of("cmd"), 0);
    assert_eq!(
        m.values_of("cmd").unwrap().collect::<Vec<_>>(),
        vec![env_value]
    );

    let m = app
        .clone()
        .get_matches_from_safe(vec!["df", "--", "-i"])
        .unwrap();
    assert!(m.is_present("cmd"));
    assert_eq!(m.occurrences_of("cmd"), 1);
    assert_eq!(m.values_of("cmd").unwrap().collect::<Vec<_>>(), vec!["-i"]);
}
```
Gives
```
thread 'trailing_var_arg' panicked at 'assertion failed: `(left == right)`
  left: `["-i", "zsh"]`,
 right: `["-i"]`', tests/env.rs:293:5
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

while clap3 passes
```rust

#[test]
fn trailing_var_arg() {
    let env_name = "CLP_TEST_TRAILING_VAR_ARG";
    let env_value = "zsh";
    env::set_var(env_name, env_value);
    let app = App::new("df").setting(AppSettings::TrailingVarArg).arg(
        Arg::new("cmd")
            .env(env_name)
            .takes_value(true)
            .multiple_values(true)
            .use_delimiter(false),
    );

    let m = app.clone().try_get_matches_from(vec!["df", "--"]).unwrap();
    assert!(m.is_present("cmd"));
    assert_eq!(m.occurrences_of("cmd"), 0);
    assert_eq!(
        m.values_of("cmd").unwrap().collect::<Vec<_>>(),
        vec![env_value]
    );

    let m = app
        .clone()
        .try_get_matches_from(vec!["df", "--", "-i"])
        .unwrap();
    assert!(m.is_present("cmd"));
    assert_eq!(m.occurrences_of("cmd"), 1);
    assert_eq!(m.values_of("cmd").unwrap().collect::<Vec<_>>(), vec!["-i"]);
}
```

---

_Closed by @pksunkara on 2021-07-30 09:31_

---
