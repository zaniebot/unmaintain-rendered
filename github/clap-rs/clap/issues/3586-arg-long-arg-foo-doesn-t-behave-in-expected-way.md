---
number: 3586
title: "`arg!(--long-arg \"foo\")` doesn't behave in expected way"
type: issue
state: open
author: petrochenkov
labels:
  - C-bug
  - A-builder
  - S-waiting-on-design
assignees: []
created_at: 2022-03-28T15:33:45Z
updated_at: 2022-05-09T14:34:23Z
url: https://github.com/clap-rs/clap/issues/3586
synced_at: 2026-01-07T13:12:19-06:00
---

# `arg!(--long-arg "foo")` doesn't behave in expected way

---

_Issue opened by @petrochenkov on 2022-03-28 15:33_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.54.0

### Clap Version

3.1.6

### Minimal reproducible code

```rust
fn main() {
    let matches = Command::new("my_command").args(&[arg!(--long-option "Description")]).get_matches();
}
```


### Steps to reproduce the bug with the above code

```
cargo run
```

### Actual Behaviour

```
thread 'main' panicked at 'assertion failed: `(left == right)`
  left: `Some("long")`,
 right: `None`: Short flags should precede long flags
```

### Expected Behaviour

The code either builds an argument with long name `--long-option`, or errors at compile time.

### Additional Context

The issue was found when migrating code from clap 2.

This is a footgun affecting naively ported code that cannot be detected until runtime. 

### Debug Output

_No response_

---

_Label `C-bug` added by @petrochenkov on 2022-03-28 15:33_

---

_Label `A-builder` added by @epage on 2022-03-28 15:42_

---

_Label `S-waiting-on-design` added by @epage on 2022-03-28 15:42_

---

_Comment by @epage on 2022-03-28 15:43_

This seems to be a limitation of macro-rules that I couldn't find a way around.  You need to quote the long option

```
fn main() {
    let matches = Command::new("my_command").args(&[arg!(--"long-option" "Description")]).get_matches();
}
```

---

_Comment by @petrochenkov on 2022-03-28 15:52_

I found the `--"long-option"` form later, but the fact that the `--long-option` form is silently accepted while being incorrect is still a problem.

---

_Comment by @epage on 2022-03-28 16:06_

> but the fact that the --long-option form is silently accepted while being incorrect is still a problem.

which is why this issue is open but I'm not aware of any good solution.

Also, we have resigned ourselves in general to not being able to turn every error into a compile error but backstop with debug asserts.  We encourage people to add a test that calls `Command::debug_asserrt()` to help catch issues earlier in the development process.

---

_Comment by @evgeniy-terekhin on 2022-05-07 14:02_

@epage I guess one solution could be to allow defining short and long flags only in one exact order (-f --flag)
But I guess this might and probably will break someone's code

---

_Comment by @epage on 2022-05-08 01:17_

@evgeniy-terekhin I think I'm missing something with your suggestion.  We do only allow one order the problem is I didn't see a way to enforce that in macro rules without blowing up the number of cases we define and I didn't see a way for that to allow `--foo-bar` without quotes.

---

_Comment by @evgeniy-terekhin on 2022-05-08 06:54_

@epage well I duess I made this suggestion without giving it a proper thought.
Maybe converting this macro into a proc macro will enable better compile time errors.
I'll have to experiment on this a bit.

---

_Comment by @epage on 2022-05-09 14:34_

> Maybe converting this macro into a proc macro will enable better compile time errors.
I'll have to experiment on this a bit.

Yes, my guess is that will be the only way forward for improving this macro.  While that might seem counter to one of the reasons to use the builder API over the derive (macros hurt build times),. this macro could possibly have no dependencies like [xflags](https://github.com/matklad/xflags/tree/master/xflags-macros)

---
