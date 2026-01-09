---
number: 1254
title: "Appsetting::InferSubcommands can't match a command whose name is a substring of another"
type: issue
state: closed
author: vincentdephily
labels:
  - C-bug
  - A-parsing
  - E-medium
  - E-easy
assignees: []
created_at: 2018-04-19T10:32:14Z
updated_at: 2018-08-02T03:30:22Z
url: https://github.com/clap-rs/clap/issues/1254
synced_at: 2026-01-07T13:12:19-06:00
---

# Appsetting::InferSubcommands can't match a command whose name is a substring of another

---

_Issue opened by @vincentdephily on 2018-04-19 10:32_

### Rust Version
1.25.0

### Affected Version of clap
2.31.2

### Expected Behavior Summary
Assuming I have subcommands "foo", "foobar", and "football" with Appsetting::InferSubcommands, I should be able to give "foo", "foob", or "foot" parameters (it's ok to fail if I give "f" or "fo").

### Actual Behavior Summary
```
$ ./inferbug.crs foo
error: The subcommand 'foo' wasn't recognized
        Did you mean 'foo'?
```

### Sample Code or Link to Sample Code
```rust
#!/usr/bin/env run-cargo-script
//! ```cargo                                                                                                                                                                                                                                 
//! [dependencies]                                                                                                                                                                                                                           
//! clap = "2.31.2"                                                                                                                                                                                                                          
//! ```                                                                                                                                                                                                                                      
extern crate clap;
use clap::{App, AppSettings, SubCommand};
fn main() {
    let cli = App::new("clap-infer-bug")
        .setting(AppSettings::InferSubcommands)
        .setting(AppSettings::SubcommandRequiredElseHelp)
        .subcommand(SubCommand::with_name("foo"))
        .subcommand(SubCommand::with_name("foobar"))
        .subcommand(SubCommand::with_name("football"))
        .get_matches();
    println!("{:?}", cli.subcommand().0);
}
```



---

_Comment by @kbknapp on 2018-06-05 02:19_

Sorry for the wait, I've been away with my day job for a few weeks.

Interesting. At first glance I instantly wonder why you'd have a subcommand match the substring of another *and* use the infersubcommands...however after thinking about it for a second I find it valid `foo` isn't ambiguous if it exactly matches a subcommand.

This is a simple fix and I'd be happy to mentor someone to get the commit. What's happening is when `InferSubcommands` is used, it checks how many subcommands match the substring `foo` in this case, and if it returns only 1 it consider it a match. However, in this case it returns 3, and therefore considers it ambiguous.

Someone would probably just have to massage [these lines](https://github.com/kbknapp/clap-rs/blob/f74646de8c94d98db3e50f19846f050f6fee9c76/src/app/parser.rs#L680-L699) to fix this issue.

---

_Label `T: bug` added by @kbknapp on 2018-06-05 02:20_

---

_Label `P4: nice to have` added by @kbknapp on 2018-06-05 02:20_

---

_Label `C: subcommands` added by @kbknapp on 2018-06-05 02:20_

---

_Label `D: easy` added by @kbknapp on 2018-06-05 02:20_

---

_Label `C: parsing` added by @kbknapp on 2018-06-05 02:20_

---

_Label `M: mentored` added by @kbknapp on 2018-06-05 02:20_

---

_Label `good first issue` added by @kbknapp on 2018-06-05 02:20_

---

_Referenced in [clap-rs/clap#1298](../../clap-rs/clap/pulls/1298.md) on 2018-06-13 10:47_

---

_Closed by @kbknapp on 2018-06-18 00:36_

---
