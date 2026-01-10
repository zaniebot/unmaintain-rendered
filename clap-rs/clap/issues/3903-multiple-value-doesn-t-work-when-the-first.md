---
number: 3903
title: "Multiple value doesn't work when the first argument attached to short flag"
type: issue
state: closed
author: key262yek
labels:
  - C-bug
  - A-parsing
  - S-wont-fix
assignees: []
created_at: 2022-07-09T12:39:40Z
updated_at: 2023-03-28T16:59:03Z
url: https://github.com/clap-rs/clap/issues/3903
synced_at: 2026-01-10T01:27:48Z
---

# Multiple value doesn't work when the first argument attached to short flag

---

_Issue opened by @key262yek on 2022-07-09 12:39_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.62.0 (a8314ef7d 2022-06-27)

### Clap Version

3.2.8

### Minimal reproducible code

```rust
use clap::Command;

fn main() {
    let sc = Command::new("test")
        .arg(
            Arg::new("m")
                .short('m')
                .takes_value(true)
                .multiple_values(true),
        )
        .get_matches_from(vec!["test", "-ma", "b", "c"]);

    assert_eq!(
        vec!["a", "b", "c"],
        sc.get_many::<String>("m")
            .unwrap()
            .into_iter()
            .map(|x| x.as_str())
            .collect::<Vec<&str>>()
    );
}
```


### Steps to reproduce the bug with the above code

cargo run

### Actual Behaviour

error: Found argument 'b' which wasn't expected, or isn't valid in this context

### Expected Behaviour

program ends without error

### Additional Context

In clap v2.34, following equivalent code works fine. 

```rust
use clap::App;

fn main(){
let sc = App::new("test")
        .arg(
            Arg::with_name("m")
                .short("m")
                .takes_value(true)
                .multiple(true),
        )
        .get_matches_from(vec!["test", "-ma", "b", "c"]);

assert_eq!(
        vec!["a", "b", "c"],
        sc.values_of("m")
            .unwrap()
            .into_iter()
            .collect::<Vec<&str>>()
    );
}
```

I want to ask, this result is whether a bug or what you expected

### Debug Output

error: Found argument 'b' which wasn't expected, or isn't valid in this context


---

_Label `C-bug` added by @key262yek on 2022-07-09 12:39_

---

_Referenced in [RustPython/RustPython#3629](../../RustPython/RustPython/issues/3629.md) on 2022-07-09 12:46_

---

_Referenced in [RustPython/RustPython#3512](../../RustPython/RustPython/pulls/3512.md) on 2022-07-09 12:47_

---

_Comment by @ciehanski on 2022-07-10 05:47_

+1. Migrated from structopt 0.3.26. When attempting to parse a `Vec<String>` in `derive` mode, I experience the same errors as @key262yek in their `builder` mode example.

```rust
#[clap(arg_required_else_help = true)]
Add {
    #[clap(value_parser)]
    job: String,
    #[clap(short = 'f', long = "files", value_parser)]
    file_path: Vec<String>,
}
```

```bash
$ cargo run add -f "test" "test1" "test2"
error: Found argument 'test2' which wasn't expected, or isn't valid in this context

USAGE:
    kip add --files <FILE_PATH> <JOB>

For more information try --help
```

This functionality worked prior in structopt so I'm assuming this may be a side effect of the v3 upgrade.

Versions:
- clap 3.2.8
- rustc 1.62.0 (a8314ef7d 2022-06-27)


---

_Referenced in [ciehanski/kip#101](../../ciehanski/kip/issues/101.md) on 2022-07-10 05:52_

---

_Comment by @epage on 2022-07-12 00:15_

@ciehanski your case is slightly different.  See https://github.com/clap-rs/clap/discussions/3213

---

_Comment by @epage on 2022-07-12 00:18_

> I want to ask, this result is whether a bug or what you expected

While I don't know the original intent of the change between 2 and 3, I would say this is expected behavior.

My way of thinking of this is to look at the `=` case.  `-m=a b c` doesn't make it look like `b c` are attached to `-m`.  I think its the case also with `-ma b c`.  The part I'm a little more unsure of is that we stop parsing when coming across delimited values, so `-m a,b c` means that `c` isn't attached to `-m`.

I'd be willing to hear other thoughts on this and we can dig into the history.  I'll leave this open for now for that conversation but I'm assuming this will be closed as expected.

---

_Label `A-parsing` added by @epage on 2022-07-12 00:18_

---

_Label `S-wont-fix` added by @epage on 2022-07-12 00:18_

---

_Comment by @key262yek on 2022-07-12 06:34_

I found `-m quopri -d` is working for both v2 and v3. (`-d` is a hyphen value for the argument `"m"`) But `-mquopri -d` is working only for v2. If the ambiguity was problem, I thought the both cases would be changed. So this one-side change made me think this is more like accidental change rather than intentional change, because both cases looking like similar ambiguous to me.

---

_Comment by @fanninpm on 2022-07-12 18:38_

@key262yek In CPython, `-c` and `-m` are ["interface options"](https://docs.python.org/3/using/cmdline.html#interface-options), and once the interpreter encounters an interface option, it parses that argument, _dumps the rest of the command_ into `sys.argv[1:]`, and stops parsing altogether. So, in CPython…

```text
python3 -m quopri -h
```

should have the same behavior as…

```text
python3 -mquopri -h
```

(here, the `-h` flag is being dumped into `sys.argv[1]`.)

I hope that helps.

---

_Comment by @key262yek on 2022-07-14 11:38_

@fanninpm I understand your two examples are equivalent. 
The reason why I cannot change the way of passing arguments is because it is in unittests of rustpython. 
Thus I need to find different way to parsing them without change argument form itself. 

---

_Comment by @fanninpm on 2022-07-14 16:43_

@key262yek Perhaps what is needed in this case is the [`allow_hyphen_values` method](https://docs.rs/clap/3.2.12/clap/builder/struct.Arg.html#method.allow_hyphen_values).

---

_Referenced in [RustPython/RustPython#4445](../../RustPython/RustPython/pulls/4445.md) on 2023-01-13 17:31_

---

_Closed by @epage on 2023-03-28 16:59_

---

_Referenced in [RustPython/RustPython#5414](../../RustPython/RustPython/pulls/5414.md) on 2024-09-25 05:26_

---
