---
number: 385
title: Unable to pass a negative number as an argument
type: issue
state: closed
author: amandasaurus
labels:
  - C-bug
  - A-parsing
assignees: []
created_at: 2016-01-25T15:01:41Z
updated_at: 2018-08-02T03:29:47Z
url: https://github.com/clap-rs/clap/issues/385
synced_at: 2026-01-10T01:26:28Z
---

# Unable to pass a negative number as an argument

---

_Issue opened by @amandasaurus on 2016-01-25 15:01_

Here's a simple programme:

```
extern crate clap;
use clap::{App, Arg};

fn main() {
    let matches = App::new("myapp")
                    .arg(Arg::with_name("x").short("x").required(true).takes_value(true))
                  .get_matches();

    let x = matches.value_of("x").unwrap();
    println!("Value of x is {}", x);
}
```

It can run when you pass in `10` as an argument:

```
$ cargo run -- -x 10
     Running `target/debug/myapp -x 10`
Value of x is 10
```

However not if you pass in `-10` as an argument:

```
$ cargo run -- -x -10
     Running `target/debug/myapp -x -10`
error: The argument '-1' isn't valid
USAGE:
    myapp [FLAGS] -x <x>

For more information try --help
An unknown error occurred

To learn more, run the command again with --verbose.
```

Clearly if the argument (`x`) takes a value, then the next string in the command line should be treated as that value, and not attempted to be parsed as a argument in it's own right.


---

_Comment by @kbknapp on 2016-01-25 15:08_

Thanks for taking the time to file this! 

Try it without using `cargo run`. Such as `./target/debug/myapp -- -x 10`. It was actually news to me, but in #384 we found out that `cargo run` actually "eats" the `--` and therefore `clap` never sees it so it's as if you're passing `myapp -x 10` 


---

_Label `T: RFC / question` added by @kbknapp on 2016-01-25 15:08_

---

_Comment by @amandasaurus on 2016-01-25 15:48_

I ran it as "./target/debug/myapp -x -10` and got the same result.


---

_Comment by @kbknapp on 2016-01-25 16:21_

Ah ok I misunderstood at first. This is a bug. Thanks!


---

_Label `T: bug` added by @kbknapp on 2016-01-25 16:22_

---

_Label `C: args` added by @kbknapp on 2016-01-25 16:22_

---

_Label `D: easy` added by @kbknapp on 2016-01-25 16:22_

---

_Label `P3: want to have` added by @kbknapp on 2016-01-25 16:22_

---

_Label `C: parsing` added by @kbknapp on 2016-01-25 16:22_

---

_Label `W: 2.x` added by @kbknapp on 2016-01-25 16:22_

---

_Label `T: RFC / question` removed by @kbknapp on 2016-01-25 16:22_

---

_Comment by @kbknapp on 2016-01-25 16:26_

Also, for my curiosity did you run it as `myapp -x "-10"`? Because the difficulty comes in from the way `clap` determines if it's done parsing an options values is by looking for the leading `-`, this comes in to play when parsing multiple values such as `-x val1 val2 val3 -y val4`

The way I could fix this bug is by having something like if you set `arg.num_values(1)` it will parse the next value as it's value no matter what (regardless of leading hyphen). The downside is if someone makes a typo and forgets the value `-x -y val2` instead of `-x val1 -y val2` they will get a strange(r) error message about not expecting `val2` vs the normal message of `-x` missing a value.


---

_Comment by @amandasaurus on 2016-01-25 16:55_

> Also, for my curiosity did you run it as `myapp -x "-10"`?

Just tried that. Same buggy outcome. Even if that worked it would be a bug. However wrapping things in `"` is probably pointless, because shells like bash will strip that before calling the programme.

If I ran it with `-x "\"-10\""` it parsed, but it set the value of `x` to `"-10"`, not `-10`, so it's not the same thing at all.


---

_Comment by @kbknapp on 2016-01-25 17:59_

Ok good to know. Also, it'll always parse values to a string, it's up to the consumer to do otherwise. So `10` will always be `"10"` (same with negative numbers) because parsing to numbers isn't something everyone needs or wants to spend cycles on. Plus some may one `u8` some may want `i64`, etc. 

If you want to pull a number out of a value there is a macro to remove the boiler plate called `value_t!` which returns a `Result<T>`. So something like, `value_t!(m.value_of("x"), i64);`. Also if you'd like to exit with an error message upon a failed parse you can just append `.unwrap_or_else(|e| e.exit());` (Note, v2 soon to be release provides a better macro calling convention of `value_t!(m, "x", i64);` which still returns a `Result<T>`).

I'll play with some ideas about this bug though and post back with any updates.


---

_Added to milestone `v2.0` by @kbknapp on 2016-01-25 18:26_

---

_Referenced in [clap-rs/clap#390](../../clap-rs/clap/pulls/390.md) on 2016-01-26 15:00_

---

_Comment by @kbknapp on 2016-01-26 15:19_

Closed with #390 (This will be available shortly on 2.x release)


---

_Closed by @kbknapp on 2016-01-26 15:19_

---
