---
number: 695
title: Get raw value of subcommand
type: issue
state: closed
author: Yamakaky
labels: []
assignees: []
created_at: 2016-10-19T04:26:45Z
updated_at: 2018-08-02T03:29:54Z
url: https://github.com/clap-rs/clap/issues/695
synced_at: 2026-01-07T13:12:19-06:00
---

# Get raw value of subcommand

---

_Issue opened by @Yamakaky on 2016-10-19 04:26_

For example in `sub 1 + 1`, I would like to get the string `"1 + 1"` associated with the subcommand `sub`. `ArgSetting::Multiple` doesn't seem appropriate since it's more for `-v`-like arguments.


---

_Comment by @kbknapp on 2016-10-19 13:53_

Depending on the specific implementation of the subcommand, if all these values are stored in a single positional argument you could use `Arg::multiple` and then collect the values.

i.e.

``` rust

let m = App::new("test")
    .subcommand(SubCommand::with_name("sub")
        .arg(Arg::with_name("math")
            .multiple(true)))
    .get_matches();

let args = m.subcommand_matches("sub")
    .unwrap()
    .values_of("math")
    .collect::<Vec<_>>() // Vec<&str>: ["1", "+", "1"]
    .join(" ");

assert_eq!(&*args, "1 + 1");
```

Alternatively, if you just want to get truly raw subcommand args (all of them) you can use [external subcommands](http://kbknapp.github.io/clap-rs/clap/enum.AppSettings.html#variant.AllowExternalSubcommands) which contain a "special" arg of the subcommand name itself, which contains all the raw values.


---

_Label `T: RFC / question` added by @kbknapp on 2016-10-19 13:54_

---

_Comment by @Yamakaky on 2016-10-19 14:51_

Seems good, I'll try!


---

_Comment by @Yamakaky on 2016-10-19 20:44_

It works, thanks!


---

_Closed by @Yamakaky on 2016-10-19 20:44_

---
