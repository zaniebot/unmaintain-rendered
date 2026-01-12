```yaml
number: 1066
title: "AppSettings::AllowLeadingHyphen makes app silence unknown unexpected arguments"
type: issue
state: closed
author: axelchalon
labels: []
assignees: []
created_at: 2017-10-12T15:34:12Z
updated_at: 2018-08-02T03:30:12Z
url: https://github.com/clap-rs/clap/issues/1066
synced_at: 2026-01-12T16:14:10Z
```

# AppSettings::AllowLeadingHyphen makes app silence unknown unexpected arguments

---

_@axelchalon_

_rustc 1.18.0 / clap 2.26.0_

When the global setting AllowLeadingHyphen is set and the app has at least one argument registered, Clap doesn't return an error if the program is called with an unknown, unexpected argument (it doesn't even have to start with a hyphen)

### Sample Code / Steps to Reproduce the Issue
```
App::new("My program")
    .global_setting(AppSettings::AllowLeadingHyphen) 
    .arg(Arg::from_usage("--some-argument"))
    .get_matches();
```

```
$ program hello
... (program runs fine)

Output of get_matches() :
ArgMatches { args: {}, subcommand: None, usage: Some("USAGE:\n    program [FLAGS]") }
```

---

_Referenced in [openethereum/parity-ethereum#6485](../../openethereum/parity-ethereum/issues/6485.md) on 2017-10-12 15:35_

---

_Referenced in [openethereum/parity-ethereum#6711](../../openethereum/parity-ethereum/pulls/6711.md) on 2017-10-12 15:35_

---

_Comment by @kbknapp on 2017-10-12 18:26_

This *should* only happen if there is something expecting a value, i.e. if the unknown/unexpected argument could *possibly* be a value to some other argument (either positional or option). This is because clap can't really know if something starting with a hyphen is supposed to be a value, or just an incorrect flag/option.

If this *isn't* the case, i.e. there's no possible way it could be a valid value and it's *still* not detecting the error then that's a bug.

I haven't tested this yet on my machine as I'll have to do that when I get home, so this may very well be the bug you're suggesting 😉 I just wanted to lay out how this is supposed to work in a bug free world.

On a side note, if possible I recommend only using this setting for individual args via [`Arg::allow_hyphen_values`](https://docs.rs/clap/2.26.2/clap/struct.Arg.html#method.allow_hyphen_values)

---

_Added to milestone `2.27.0` by @kbknapp on 2017-10-12 21:35_

---

_Closed by @kbknapp on 2017-10-25 01:49_

---
