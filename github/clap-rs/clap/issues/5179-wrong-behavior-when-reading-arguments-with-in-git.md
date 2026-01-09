---
number: 5179
title: Wrong behavior when reading arguments with / in git bash
type: issue
state: closed
author: oriongonza
labels:
  - C-bug
assignees: []
created_at: 2023-10-20T02:53:13Z
updated_at: 2023-10-23T12:49:27Z
url: https://github.com/clap-rs/clap/issues/5179
synced_at: 2026-01-07T13:12:20-06:00
---

# Wrong behavior when reading arguments with / in git bash

---

_Issue opened by @oriongonza on 2023-10-20 02:53_

The argument expands to git bash root folder. This doesn't happen with other commands (`echo /foo` works fine)
When the argument has only one letter (`/a`) it expands to `A:/`

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

1.73

### Clap Version

4.4.6

### Minimal reproducible code

```rust
#[derive(Parser, Debug)
struct Cli {
  pub foo: String
}
```

### Steps to reproduce the bug with the above code

under git bash:
cargo run -- '/foo'

### Actual Behaviour

foo = `"C:/Users/user/AppData/Local/Programs/Git/foo"`


### Expected Behaviour

foo = "/foo" 

### Additional Context

https://github.com/chmln/sd/issues/170

### Debug Output

_No response_

---

_Label `C-bug` added by @oriongonza on 2023-10-20 02:53_

---

_Comment by @epage on 2023-10-20 12:22_

If you run with the debug output, as requested in the template, you'll likely see that clap is just subject to whatever `std::env::args_os` returns and that this problem is likely an error in something before clap gets it.

---

_Comment by @CosmicHorrorDev on 2023-10-21 15:16_

Yeah, we've already confirmed that this is just `clap` using the args that it gets provided. Someone even made a demo program in C already (https://github.com/chmln/sd/pull/208#issuecomment-1746817623)

It's just some weird automagical path replacement that happens

---

_Closed by @epage on 2023-10-21 17:44_

---

_Comment by @oriongonza on 2023-10-22 17:35_

Yes, even in C it doesn't capture argv correctly but it would be nice if clap provided a fix. 
Most Rust CLI programs depend on this and git bash is widely used in Windows so if a general fix were to be possible this would be the place for it.

---

_Comment by @oriongonza on 2023-10-22 17:45_

`export MSYS_NO_PATHCONV=1` solves it, but I don't think it's possible to use this without making the user do it.

---

_Comment by @epage on 2023-10-23 12:49_

> Most Rust CLI programs depend on this and git bash is widely used in Windows so if a general fix were to be possible this would be the place for it.

We also don't automatically integrate `wild` or `argfile` but rely on people to do that directly.  We'd need a lot more information to make the decision to unilaterally workaround this for people (if there is even a way to workaround it), like why MSYS does this and if it is universally viewed (even by MSYS maintainers) as wrong.

---
