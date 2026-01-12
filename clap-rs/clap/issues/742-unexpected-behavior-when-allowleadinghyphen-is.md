```yaml
number: 742
title: unexpected behavior when AllowLeadingHyphen is enabled
type: issue
state: closed
author: BurntSushi
labels:
  - C-enhancement
  - A-parsing
assignees: []
created_at: 2016-11-12T18:22:43Z
updated_at: 2018-08-02T03:29:56Z
url: https://github.com/clap-rs/clap/issues/742
synced_at: 2026-01-12T16:14:09Z
```

# unexpected behavior when AllowLeadingHyphen is enabled

---

_@BurntSushi_

I understand that there is a `AppSettings::AllowLeadingHyphen` and that it is application wide, but is there anyway to get a more targeted approach? For example, I have flag like this:

```
    -e, --regexp PATTERN ...   Use PATTERN to search. This option can be
                               provided multiple times, where all patterns
                               given are searched. This is also useful when
                               searching for a pattern that starts with a dash.
```

Users occasionally want to use a `PATTERN` that starts with a `-`. By default, clap disallows this to make failure modes better (which I think is a great choice by default). I can permit `PATTERN` to start with a leading `-` by enabling the `AllowLeadingHyphen` option application wide, but if I do this, I get unexpected behavior. Here's my clap app:

```rust
extern crate clap;

use clap::{App, AppSettings, Arg};

fn main() {
    let app = App::new("ripgrep")
        .setting(AppSettings::AllowLeadingHyphen)
        .arg(Arg::with_name("path")
             .multiple(true)
             .help("A file or directory to search. Directories are searched \
                    recursively."))
        .arg(Arg::with_name("regexp")
             .long("regexp").short("e")
             .takes_value(true)
             .multiple(true)
             .number_of_values(1)
             .conflicts_with("files")
             .value_name("pattern")
             .help("A regular expression pattern used for searching. \
                    Multiple patterns may be given."))
        .arg(Arg::with_name("files")
             .long("files")
             .help("Print each file that would be searched \
                    (but don't search)."));
    println!("{:?}", app.get_matches());
}
```

And here's an example invocation:

```
$ ./target/debug/clap-hyphen a b --files c
ArgMatches { args: {"path": MatchedArg { occurs: 4, vals: {1: "a", 2: "b", 3: "--files", 4: "c"} }, "files": MatchedArg { occurs: 1, vals: {} }}, subcommand: None, usage: Some("USAGE:\n    clap-hyphen [FLAGS] [OPTIONS] [--] [path]...") }
```

Notice that `--files` is correctly detected as a flag, but it is *also* put into the values of the `path` positional argument. If I remove the `AppSettings::AllowLeadingHypen` option, then the behavior returns to normal and `--files` is no longer part of `path`.

I'm not sure whether this is intended behavior or not, but I can think of at least two ways you might consider solving this problem if it is indeed unintended (but of course, keeping in mind that I'm unfamiliar with the implementation!):

1. If `--files` is determined to be an actual flag, then it shouldn't show up in the value list of another `Arg`.
2. Add an `ArgSettings::AllowLeadingHypen` that would let me explicitly say that only values for the `-e/--regexp` flag can start with a leading `-`. (I'm guessing there's probably a good reason why this doesn't already exist though!)

Thoughts? Have I missed something?

---

_Comment by @BurntSushi on 2016-11-12 18:25_

I wonder if there's a third option: don't permit leading hyphens in _positional_ arguments, only in _flag_ arguments. (Which may or may not be easier to implement, I don't know.)


---

_Comment by @kbknapp on 2016-11-12 20:46_

Actually, now that you mention it, I _do_ think there's a way I could easily allow targeted leading hyphens in options only. i.e. not command wide, and not in positional arguments. Let me play with an implementation real quick and see if I can get this working.

In an old implementation where this all stemmed from, it wasn't easy to determine for a single option, but this new idea may just work. Standby for updates!


---

_Referenced in [BurntSushi/ripgrep#233](../../BurntSushi/ripgrep/pulls/233.md) on 2016-11-13 02:46_

---

_Label `C: parsing` added by @kbknapp on 2016-11-14 21:57_

---

_Label `D: intermediate` added by @kbknapp on 2016-11-14 21:57_

---

_Label `P2: need to have` added by @kbknapp on 2016-11-14 21:57_

---

_Label `T: enhancement` added by @kbknapp on 2016-11-14 21:57_

---

_Label `W: 2.x` added by @kbknapp on 2016-11-14 21:57_

---

_Comment by @kbknapp on 2016-11-14 21:58_

Just an update, I'm still working this issue. It was a busy few days, so I didn't get near as far as I'd have liked. I should have a working PR in a few days.


---

_Comment by @BurntSushi on 2016-11-14 21:58_

@kbknapp No worries, it's a very small matter. Thanks for looking into it at all. :-)


---

_Referenced in [BurntSushi/ripgrep#215](../../BurntSushi/ripgrep/issues/215.md) on 2016-11-18 01:37_

---

_Closed by @homu on 2016-11-21 04:52_

---
