```yaml
number: 558
title: TrailingVarArg with hyphens
type: issue
state: closed
author: marcbowes
labels:
  - C-bug
  - A-parsing
assignees: []
created_at: 2016-07-01T16:15:49Z
updated_at: 2018-08-02T03:29:51Z
url: https://github.com/clap-rs/clap/issues/558
synced_at: 2026-01-12T16:14:09Z
```

# TrailingVarArg with hyphens

---

_@marcbowes_

#278 shows how to use `TrailingVarArg` and a multiple arg to capture extra args. This works great, but I can't seem to get it working with args that start with hyphens:

```
[dependencies]
clap = "2.2.4"
```

``` rust
extern crate clap;

#[cfg(test)]
mod tests {
    use clap::*;

    #[test]
    fn test_vararg_1() {
        let m = App::new("myprog")
            .setting(AppSettings::TrailingVarArg)
            .subcommand(SubCommand::with_name("sub")
                .setting(AppSettings::TrailingVarArg)
                .arg(Arg::from_usage("<cmd>... 'commands to run'")))
            .get_matches_from(vec!["myprog", "sub", "arg1", "-r", "val1"]);

        let trail: Vec<&str> =
            m.subcommand_matches("sub").unwrap().values_of("cmd").unwrap().collect();
        assert_eq!(trail, ["arg1", "-r", "val1"]);
    }

    #[test]
    fn test_vararg_2() {
        let m = App::new("myprog")
            .setting(AppSettings::TrailingVarArg)
            .subcommand(SubCommand::with_name("sub")
                .setting(AppSettings::TrailingVarArg)
                .arg(Arg::from_usage("<cmd>... 'commands to run'")))
            .get_matches_from(vec!["myprog", "sub", "--arg1", "-r", "val1"]);
       // ^ ERROR due to --arg1
        let trail: Vec<&str> =
            m.subcommand_matches("sub").unwrap().values_of("cmd").unwrap().collect();
        assert_eq!(trail, ["--arg1", "-r", "val1"]);
    }
}
```

```
cargo test -- test_vararg_1
     Running target/debug/vararg-e311bd86d802e1d9

running 1 test
test tests::test_vararg_1 ... ok

test result: ok. 1 passed; 0 failed; 0 ignored; 0 measured
```

``` rust
cargo test -- test_vararg_2 
     Running target/debug/vararg-e311bd86d802e1d9

running 1 test
error: Found argument '--arg1' which wasn't expected, or isn't valid in this context

USAGE:
    myprog sub <cmd>...

For more information try --help
```


---

_Comment by @clux on 2016-07-01 16:20_

`TrailingVarArg` must be set inside the subcommand, or you have to make TrailingVarArg a global setting.


---

_Comment by @marcbowes on 2016-07-01 16:25_

Sorry, I messed up the description. Updated.


---

_Renamed from "SubCommand using TrailingVarArg" to "TrailingVarArg with hyphens" by @marcbowes on 2016-07-01 16:32_

---

_Comment by @kbknapp on 2016-07-01 17:17_

This appears to be a bug, thanks for filing! Like @clux said, the setting should be on the subcommand it affects. You can also use `global_setting` to set it on the parent command and propagate it down through all the child subcommands. Just an option :wink:

I'm looking into this bug now as to why it only works when the first argument doesn't start with a hyphen. I'll post back here with what I find.


---

_Label `T: bug` added by @kbknapp on 2016-07-01 17:17_

---

_Label `P2: need to have` added by @kbknapp on 2016-07-01 17:17_

---

_Label `D: easy` added by @kbknapp on 2016-07-01 17:17_

---

_Label `C: parsing` added by @kbknapp on 2016-07-01 17:17_

---

_Label `W: 2.x` added by @kbknapp on 2016-07-01 17:17_

---

_Label `C: settings` added by @kbknapp on 2016-07-01 17:17_

---

_Added to milestone `2.9.0` by @kbknapp on 2016-07-01 17:17_

---

_Comment by @kbknapp on 2016-07-01 17:55_

#560 fixes this.

Also, part of the problem is you need to add [`AppSettings::AllowLeadingHyphen`](http://kbknapp.github.io/clap-rs/clap/enum.AppSettings.html#variant.AllowLeadingHyphen), because it's also possible to use proper long options and flags with commands that accept `TrailingVarArg`s, so this distinguishes them.

This should be a passing test once the PR merges:

``` rust
#[test]
    fn test_vararg_2() {
        let m = App::new("myprog")
            .subcommand(SubCommand::with_name("sub")
                .setting(AppSettings::TrailingVarArg)
                .setting(AppSettings::AllowLeadingHyphen)   // <-- Only change
                .arg(Arg::from_usage("<cmd>... 'commands to run'")))
            .get_matches_from(vec!["myprog", "sub", "--arg1", "-r", "val1"]);
        let trail: Vec<&str> =
            m.subcommand_matches("sub").unwrap().values_of("cmd").unwrap().collect();
        assert_eq!(trail, ["--arg1", "-r", "val1"]);
    }
```


---

_Comment by @marcbowes on 2016-07-01 17:58_

That was fast! Thanks!

Looking forward to the completion work you've done in #560.


---

_Closed by @kbknapp on 2016-07-01 20:14_

---

_Referenced in [TeXitoi/structopt#153](../../TeXitoi/structopt/issues/153.md) on 2019-06-19 05:12_

---
