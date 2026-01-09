---
number: 592
title: Panic with positional arguments in subcommands
type: issue
state: closed
author: guiniol
labels: []
assignees: []
created_at: 2016-07-23T15:50:44Z
updated_at: 2016-08-24T14:15:23Z
url: https://github.com/clap-rs/clap/issues/592
synced_at: 2026-01-07T13:12:19-06:00
---

# Panic with positional arguments in subcommands

---

_Issue opened by @guiniol on 2016-07-23 15:50_

Positional arguments in subcommands make the program panic.
This can be reproduced by modifying the first example in the README to move `INPUT` to be a positional argument of the `test` subcommand.
This is the result:

```
$ RUST_BACKTRACE=1 cargo run -- test input
     Running `target/debug/testclap test input`
Value for config: default.conf
thread '<main>' panicked at 'called `Option::unwrap()` on a `None` value', src/libcore/option.rs:325
stack backtrace:
   1:     0x55949c6eb31f - std::sys::backtrace::tracing::imp::write::h3800f45f421043b8
   2:     0x55949c6edf7b - std::panicking::default_hook::_$u7b$$u7b$closure$u7d$$u7d$::h0ef6c8db532f55dc
   3:     0x55949c6edc03 - std::panicking::default_hook::hf3839060ccbb8764
   4:     0x55949c6e2ddd - std::panicking::rust_panic_with_hook::h5dd7da6bb3d06020
   5:     0x55949c6ee1c1 - std::panicking::begin_panic::h9bf160aee246b9f6
   6:     0x55949c6e3b1a - std::panicking::begin_panic_fmt::haf08a9a70a097ee1
   7:     0x55949c6ee15e - rust_begin_unwind
   8:     0x55949c7240cf - core::panicking::panic_fmt::h93df64e7370b5253
   9:     0x55949c7243a8 - core::panicking::panic::h9d5bd65bbb401959
  10:     0x55949c5f1a29 - _<std..option..Option<T>>::unwrap::hf0b1faac3ace462e
                        at src/libcore/macros.rs:21
  11:     0x55949c5e5a4a - testclap::main::hc2d6f4100c835050
                        at src/main.rs:43
  12:     0x55949c6ed848 - std::panicking::try::call::hbbf4746cba890ca7
  13:     0x55949c6f5c2b - __rust_try
  14:     0x55949c6f5bce - __rust_maybe_catch_panic
  15:     0x55949c6ed2ee - std::rt::lang_start::hbcefdc316c2fbd45
  16:     0x55949c5f1ee9 - main
  17:     0x7fd9d8e02740 - __libc_start_main
  18:     0x55949c5e5188 - _start
  19:                0x0 - <unknown>
error: Process didn't exit successfully: `target/debug/testclap test input` (exit code: 101)
```


---

_Referenced in [clap-rs/clap#581](../../clap-rs/clap/issues/581.md) on 2016-07-23 15:59_

---

_Comment by @kbknapp on 2016-07-23 17:31_

Interesting, I'm looking into this. Thanks!


---

_Label `T: bug` added by @kbknapp on 2016-07-23 17:31_

---

_Label `P2: need to have` added by @kbknapp on 2016-07-23 17:31_

---

_Label `C: args` added by @kbknapp on 2016-07-23 17:31_

---

_Label `C: subcommands` added by @kbknapp on 2016-07-23 17:31_

---

_Label `W: 2.x` added by @kbknapp on 2016-07-23 17:31_

---

_Assigned to @kbknapp by @kbknapp on 2016-07-23 17:31_

---

_Comment by @guiniol on 2016-07-31 16:49_

Good to know ^^
If I can help with anything, just let me know.


---

_Comment by @guiniol on 2016-08-07 18:13_

Hello again!

Since I cannot get this to work, I figured I would replace the subcommands with flags. However, each of the subcommand requires a positional argument. I wanted to have the positional arguments conflict with each other, but they cannot have the same index. What can I do?

Here is what I would like to do:

```
$ ./app --command1 arg-command1
$ ./app --command2 arg-command2
```

with `arg-command1` requiring `command1` and conflicting with `arg-command2` (because flags cannot be required).
Is there any way this would work with clap?

Cheers,


---

_Comment by @guiniol on 2016-08-07 19:34_

For future (temporary) reference, I ended up defining a arggroup with `command1` and `command2` and used a generic name instead of `arg-command1` and `arg-command2`.


---

_Label `P1: urgent` added by @kbknapp on 2016-08-20 22:21_

---

_Label `P2: need to have` removed by @kbknapp on 2016-08-20 22:21_

---

_Added to milestone `2.10.1` by @kbknapp on 2016-08-20 22:21_

---

_Comment by @kbknapp on 2016-08-20 22:37_

@guiniol I'm finally back from my travels! Could you give some more details on this error? If you could post the definition of the args, and the input that's making it panic I can reproduce easier and get this fixed.

I'm not able to reproduce this error, which makes me think it's possibly in the way the way the values of the args are being obtained, not the way the args themselves are being defined. So if you could link to your code that uses it I'd appreciate it!

Thanks!


---

_Removed from milestone `2.10.1` by @kbknapp on 2016-08-20 22:37_

---

_Comment by @guiniol on 2016-08-21 10:05_

Indeed, it was bit trickier to reproduce than what I thought. The problem seems to be with:

```
match matches.subcommand_name() {
```

I attached a minimal panicking example. The working example is the one in the main README.
[mwe.zip](https://github.com/kbknapp/clap-rs/files/428783/mwe.zip)

Additionally, it seems that `matches.is_present("debug")` returns false even if `-d` is set.


---

_Comment by @kbknapp on 2016-08-21 19:42_

The lines of code should read like this, to work correctly

``` rust
    match matches.subcommand() {
        ("test", Some(sub_matches)) => {
            // Same as original...

            println!("Using input file: {}", sub_matches.value_of("INPUT").unwrap());
        },
        _ => println!("Error"),
    }
```

Notice the `sub_matches` this is because subcommands have their own matches struct so that you can have multiple subcommands using the same arguments names without them all being clobbered by each other.

Edit: I noticed it's not in the readme that way, phew! :smile:


---

_Label `C: docs` added by @kbknapp on 2016-08-21 19:43_

---

_Label `C: args` removed by @kbknapp on 2016-08-21 19:43_

---

_Label `C: subcommands` removed by @kbknapp on 2016-08-21 19:43_

---

_Label `T: RFC / question` added by @kbknapp on 2016-08-21 19:44_

---

_Label `C: docs` removed by @kbknapp on 2016-08-21 19:44_

---

_Label `P1: urgent` removed by @kbknapp on 2016-08-21 19:44_

---

_Label `T: bug` removed by @kbknapp on 2016-08-21 19:44_

---

_Label `W: 2.x` removed by @kbknapp on 2016-08-21 19:44_

---

_Comment by @guiniol on 2016-08-24 14:15_

Thank you very much. That solved it!


---

_Closed by @guiniol on 2016-08-24 14:15_

---
