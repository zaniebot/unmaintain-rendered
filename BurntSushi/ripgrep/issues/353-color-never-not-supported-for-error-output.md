```yaml
number: 353
title: "--color never not supported for error output"
type: issue
state: closed
author: andreastt
labels:
  - bug
  - help wanted
assignees: []
created_at: 2017-02-09T13:44:13Z
updated_at: 2017-10-22T01:54:16Z
url: https://github.com/BurntSushi/ripgrep/issues/353
synced_at: 2026-01-12T16:13:21Z
```

# --color never not supported for error output

---

_@andreastt_

My shell does not support colours and consequently I set the environmental variable `TERM=dumb` to instruct programs never to colourise output.

However, even when I pass `--color never` to rg, the error output includes colour escape codes as can be seen below:

```
% rg --color never -asd
[1;31merror:[0m Found argument '[33m-d[0m' which wasn't expected, or isn't valid in this context

USAGE:
    
    rg [OPTIONS] <pattern> [<path> ...]
    rg [OPTIONS] [-e PATTERN | -f FILE ]... [<path> ...]
    rg [OPTIONS] --files [<path> ...]
    rg [OPTIONS] --type-list

For more information try [32m--help[0m
```

---

_Comment by @kbknapp on 2017-02-09 16:44_

@andreastt that's from [`clap`](https://github.com/kbknapp/clap-rs)s (`rg`'s CLI parser) error message, not `rg`'s.

I think there's a hacky way to workaround this...but it's hacky. You'd essentially be parsing the CLI twice in the case of a `CLI` error and then double checking that `--color never`. An abbriviated version is something like:

```rust
let mut app = build_cli();
let args: Args = match app.get_matches_safe() {
    Err(e) => {
        if (env::args().find(|arg| arg == "--color").is_some() && env::arg().find(|arg| arg == "never")) || env::args().find(|arg| arg == "--color=never") {
            app.setting(AppSettings::ColorNever).get_matches();
        } 
        e.exit();
    },
    Ok(m) => m.into(),
};
```

There's probably a *far* better way to do all that, but I'm going a lack of sleep and 8 plane rides 😜 

---

_Comment by @BurntSushi on 2017-02-09 16:49_

@kbknapp Yeah, probably the "right" way here is for clap to recognize `TERM=dumb`, which should mean "no colors." (It's pretty standard in my experience, and [`termcolor`](https://github.com/BurntSushi/ripgrep/blob/master/termcolor/src/lib.rs#L116) will Do The Right Thing.)

---

_Comment by @kbknapp on 2017-02-09 16:49_

~~@andreastt if you wouldn't mind, could you file an issue in `clap`s repo too? This is something that I'd like to at least attempt to address on that side of things.~~ Actually if you could just post a comment in kbknapp/clap-rs#836 as a reminder to me, that's fine!

---

_Comment by @kbknapp on 2017-02-09 16:50_

@BurntSushi yep, once I get some time to sit down and actually implement kbknapp/clap-rs#836 that's my exact plan :wink:

---

_Comment by @BurntSushi on 2017-02-09 16:51_

@kbknapp While you're at it, note that the [`atty`](https://github.com/softprops/atty) crate now supports MSYS terminals and native Windows consoles. I've wondered whether `termcolor` itself should use the `atty` crate directly.

---

_Comment by @andreastt on 2017-02-09 16:55_

Filed https://github.com/kbknapp/clap-rs/issues/847 with clap.

---

_Label `bug` added by @BurntSushi on 2017-02-18 17:39_

---

_Label `help wanted` added by @BurntSushi on 2017-05-08 22:01_

---

_Comment by @BurntSushi on 2017-05-08 22:02_

If someone wants to fix this using [@kbknapp's hack](https://github.com/BurntSushi/ripgrep/issues/353#issuecomment-278699290), then I'd be open to it. It might be hard to write a regression test for it though.

---

_Comment by @nateozem on 2017-05-13 23:37_

Sure, I'll take a stab at it.

---

_Comment by @nateozem on 2017-05-17 10:13_

There's a PR for `clap-rs` to comply with terminfo when `TERM=dumb`  (https://github.com/kbknapp/clap-rs/pull/963). 
Anything else needs to be done? Still prefer to be colorless if `--color never`? 

---

_Comment by @BurntSushi on 2017-05-17 11:01_

@nateozem Let's try to get away with `TERM=dumb` support instead of introducing a hack.

---

_Comment by @BurntSushi on 2017-10-22 01:54_

It looks like the clap PR was merged so I'm marking this as fixed. (You still can't use `--color never`, but `TERM=dumb` should be respected.)

---

_Closed by @BurntSushi on 2017-10-22 01:54_

---
