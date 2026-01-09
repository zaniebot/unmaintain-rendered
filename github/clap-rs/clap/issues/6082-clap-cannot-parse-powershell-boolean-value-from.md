---
number: 6082
title: Clap cannot parse PowerShell boolean value from environment variable
type: issue
state: closed
author: h1-yena
labels:
  - C-bug
  - A-parsing
  - S-waiting-on-author
assignees: []
created_at: 2025-07-31T19:10:00Z
updated_at: 2025-08-06T15:33:45Z
url: https://github.com/clap-rs/clap/issues/6082
synced_at: 2026-01-07T13:12:20-06:00
---

# Clap cannot parse PowerShell boolean value from environment variable

---

_Issue opened by @h1-yena on 2025-07-31 19:10_

### Please complete the following tasks

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.88.0 (6b00bc388 2025-06-23)

### Clap Version

4.5.42

### Minimal reproducible code

```rust
use clap::Parser;

fn main() {
    let args = Args::parse();

    println!("{}", args.foo);
    println!("{}", args.bar);
}
#[derive(Parser, Debug)]
struct Args {
    #[arg(short)]
    foo: String,
    #[arg(short, env="BAR")]
    bar: bool,
}
```


### Steps to reproduce the bug with the above code

Under PowerShell 5:

```PowerShell
$Env:BAR=$true;
cargo run -- -f "Hello"
```

### Actual Behaviour

Output:
```
 Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.02s
     Running `target\debug\ps_bug_test.exe -f Hello`
error: invalid value 'True' for '-b'
  [possible values: true, false]

  tip: a similar value exists: 'true'

For more information, try '--help'.
error: process didn't exit successfully: `target\debug\ps_bug_test.exe -f Hello` (exit code: 2)
```

### Expected Behaviour

Output:
```
  Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.04s
     Running `target\debug\ps_bug_test.exe -f Hello`
Hello
true
```

### Additional Context

A possible workaround is that If the env-variable is set to "true", it does indeed work.

### Debug Output

_No response_

---

_Label `C-bug` added by @h1-yena on 2025-07-31 19:10_

---

_Comment by @epage on 2025-07-31 19:43_

Is there a precedence for tools outside of the powershell ecosystem to accept `True`?

What is `$false` and precedence for that?

---

_Comment by @epage on 2025-07-31 19:43_

btw a way to workaround this is a custom value parser.

---

_Comment by @h1-yena on 2025-08-01 20:32_

I am not sure about the precedence for tools outside of the powershell here, I only just started really using `clap` or even Rust for that matter so I was wondering why this behavior was happening!

The same error happens with `$false` as well, though.

Thank you for pointing out that work around! I did not know about that option!

```Rust
use clap::Parser;
use std::str::ParseBoolError;

fn main() {
    let args = Args::parse();

    println!("{}", args.foo);
    println!("{}", args.bar);
}
#[derive(Parser, Debug)]
struct Args {
    #[arg(short)]
    foo: String,
    #[arg(short, env="BAR", value_parser=parse_ps_boolean)]
    bar: bool,
}


fn parse_ps_boolean(arg: &str) -> Result<bool,ParseBoolError> {
    arg.to_lowercase().parse()
}
```




---

_Label `A-parsing` added by @epage on 2025-08-04 18:11_

---

_Label `S-waiting-on-author` added by @epage on 2025-08-04 18:11_

---

_Comment by @h1-yena on 2025-08-05 00:33_

Hey @epage, just making sure, is there anything you need from me? I am always glad to help out and I would love for an opportunity to practice some more Rust :)

---

_Comment by @epage on 2025-08-05 01:06_

At this point, we need more information. Without any convincing cases on conventions, I'd be inclined to close this.

---

_Comment by @h1-yena on 2025-08-05 01:20_

> Without any convincing cases on conventions, I'd be inclined to close this.

What kind of conventions do you mean?

---

_Comment by @AlphaGit on 2025-08-05 02:55_

Hi there! I'd like to add on the topic that yes, Powershell does indeed have boolean values that it serializes as `True` (uppercase T) and `False`(uppercase F).

https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_booleans?view=powershell-7.5

Note that Powershell as a language allows for the parsing of multiple different values into boolean, as it can be seen on that same link. I would personally not go that route, having all those rules for a boolean parser seems like way out of scope for the purpose of this project.

But as far as the boolean representation goes, yes, True and False (capitalized) are part of the PS conventions.

---

_Comment by @h1-yena on 2025-08-05 12:22_

I would also like to add that there is no such thing as a `true` in PowerShell 5. `true` evaluates to `True` and that is the value that the parser will find in the environment variable. Of course, one could also assign `"true"` (a string) to the variable, but that would go against data modelling principles and frankly feels kind of weird to do.

---

_Comment by @epage on 2025-08-05 18:21_

Powershell breaks other conventions (iirc it uses `-` for long flags) and that is not something we support either.  My perception is Powershell does enough of their own thing that I feel like we shouldn't implicitly support its conventions but give people the flexibility to support them if desired.  So when I'm asking for conventions, I'm looking for what patterns appear within the more traditional Unix-like command environment (coreutils, git, docker, etc).

Note that we may support a way to extend clap to allow for `-long` (#2468) but that would put flag styles in the same boat as we currently are with environment variable parsing (use `value_parser`).



---

_Comment by @h1-yena on 2025-08-06 15:14_

I understand where you are coming from! Yeah, it makes sense to me that this should be handled by a custom parser. Thanks for pointing out the possibility of adding a custom parser tho! :) I think the issue can be closed, then.

---

_Closed by @epage on 2025-08-06 15:33_

---
