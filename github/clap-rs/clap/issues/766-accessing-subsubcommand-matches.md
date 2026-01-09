---
number: 766
title: Accessing subsubcommand matches
type: issue
state: closed
author: kirillbobyrev
labels:
  - A-docs
  - E-medium
assignees: []
created_at: 2016-12-05T21:14:49Z
updated_at: 2018-08-02T03:29:57Z
url: https://github.com/clap-rs/clap/issues/766
synced_at: 2026-01-07T13:12:19-06:00
---

# Accessing subsubcommand matches

---

_Issue opened by @kirillbobyrev on 2016-12-05 21:14_

From what I have seen it is not clear how a sub-subcommand matches should be accessed.

What I expect to see is something like

```rust
let matches = App::from_yaml(yml).get_matches();
let subcommand_matches = matches.subcommand_matches(matches.subcommand_name().unwrap());
let subsubcommand_matches = subcommand_matches.subcommand_matches(matches.subcommand_name().unwrap());
```

However, I get

```rust
error: no method named `subcommand_matches` found for type `std::option::Option<&clap::ArgMatches<'_>>` in the current scope
```

With

```
rustc 1.13.0
clap-rs 2.19.1
```

Am I just missing something or is such kind of subsubcommand extraction not supported?

---

_Label `T: RFC / question` added by @kbknapp on 2016-12-05 21:38_

---

_Comment by @kbknapp on 2016-12-05 21:44_

It's not the typical way to do it, but you can change your code to work below:

```rust
let matches = App::from_yaml(yml).get_matches();

// subcommand_matches returns an option
let subcommand_matches = matches.subcommand_matches(matches.subcommand_name().unwrap()).unwrap();

let subsubcommand_matches = subcommand_matches.subcommand_matches(matches.subcommand_name().unwrap()).unwrap();
```

The more common way to do this is:

```rust

match matches.subcommand() {
    ("foo", Some(foo_matches)) => {
        // Handle the myprog foo subcommand
    },
    ("bar", Some(bar_matches)) => {
        // Handle myprog bar subcommand
        match bar_matches.subcommand() {
            ("baz", Some(baz_matches)) => {
                // handle myprog bar baz subcommand
             },
             _ => unreachabe!()
        }
    },
    _ => unreachabe!()
}
```

etc.

---

_Comment by @kirillbobyrev on 2016-12-05 21:48_

Great, thank you once again and apologies for a newbie question.

Do you think it's worth adding such example to `examples/`? Even though there are great examples, I would actually love to see even more as a newcomer.

---

_Comment by @kbknapp on 2016-12-05 22:20_

For sure! We can always use more examples. I'll add it on my next doc sweep unless someone else beats me to it.

---

_Label `C: docs` added by @kbknapp on 2016-12-05 22:20_

---

_Label `D: easy` added by @kbknapp on 2016-12-05 22:20_

---

_Label `M: mentored` added by @kbknapp on 2016-12-05 22:20_

---

_Label `P4: nice to have` added by @kbknapp on 2016-12-05 22:20_

---

_Referenced in [clap-rs/clap#768](../../clap-rs/clap/pulls/768.md) on 2016-12-07 02:56_

---

_Closed by @homu on 2016-12-08 02:52_

---
