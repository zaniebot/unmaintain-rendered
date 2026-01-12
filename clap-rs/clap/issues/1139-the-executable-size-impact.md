```yaml
number: 1139
title: The executable size impact
type: issue
state: closed
author: RazrFalcon
labels:
  - C-enhancement
assignees: []
created_at: 2018-01-05T18:16:36Z
updated_at: 2018-08-02T03:30:16Z
url: https://github.com/clap-rs/clap/issues/1139
synced_at: 2026-01-12T16:14:10Z
```

# The executable size impact

---

_@RazrFalcon_

A minimal *Hello World!* application after `strip -s` is 427 KiB on my machine. But `01b_quick_example.rs` is 1155 KiB which is almost 3x bigger.

It's a pretty huge impact and I think it worth creating an issue because such kind of libraries should be tiny (in my opinion).
Maybe it's worth investigating what causes such impact on the size.

I've also tested with my own library, `svgdom`, which is pretty big and I got only 803 KiB.

Used Cargo.toml:
```toml
[dependencies.clap]
version = "2"
default-features = false

[profile.release]
opt-level = 3
lto = true
```

---

_Comment by @kbknapp on 2018-01-09 17:43_

This is the exact purpose of #1143 :smile: 

The `01b_quick_example` is probably due to the usage parser, but I'll investigate more later.

I would however kind of like to keep things in perspective. While I agree binary sizes should be as small as possible, clap does *a lot* especially when compared to minimal arg parsers like `getopts`, `pirate`, or `argparse` where you end up implementing all those features manually and those implementations won't get counted against smaller arg parsing libs. So comparing binary sizes of arg parsers isn't super fair because it's not a true apples to apples comparison, even though it sounds like it should be.

I do very much want to decrease *needless bloat*, and that's what I'm starting to investigate :+1: 

---

_Label `T: enhancement` added by @kbknapp on 2018-01-09 17:44_

---

_Label `D: intermediate` added by @kbknapp on 2018-01-09 17:44_

---

_Label `P3: want to have` added by @kbknapp on 2018-01-09 17:44_

---

_Comment by @kbknapp on 2018-01-09 17:45_

Also, I forgot to add that I would expect opt-leve=3 and lto=true to increase binary sizes. Which I know is the default for clap, but this is also because the perf gains typically outweigh a few kb in size IMO. And if they don't, it's easy to change.

---

_Comment by @kbknapp on 2018-01-15 17:37_

Here's the table for different build options (again using `ripgrep` as the test binary and `clap` as the crate to measure). Since I was curious :smile: 

All build options were set in `ripgrep`s Cargo.toml, clean build in release mode

| Size  | LTO | Opt Level |
| :---: | :---: | :---: |
| 197.4KiB  | True  | 3 |
| 189.3KiB  | False  | 3 |
| 186.5KiB  | True  | 2 |
| 183.9KiB  | False  | 2 |
| 137.3KiB  | True  | 1 |
| 139.3KiB  | False  | 1 |
| 152.5KiB  | True  | s |
| 155.0KiB  | False  | s |
| 122.9KiB  | True  | z |
| 125.5KiB  | False  | z |



---

_Comment by @kbknapp on 2018-02-12 20:05_

I'm going to close this, as it's one of those things that could be forever tweaked and tuned. Once v3-beta is out, there will be a chance to re-address this with more specific goals/comments.

---

_Closed by @kbknapp on 2018-02-12 20:05_

---

_Comment by @RazrFalcon on 2018-02-13 07:40_

I'm satisfied already =) Thanks for your work.

---
