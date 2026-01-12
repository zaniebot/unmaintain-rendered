```yaml
number: 361
title: Allow Arg to be copied/cloned
type: issue
state: closed
author: LegNeato
labels: []
assignees: []
created_at: 2015-12-11T00:27:47Z
updated_at: 2018-08-02T03:29:47Z
url: https://github.com/clap-rs/clap/issues/361
synced_at: 2026-01-12T16:14:09Z
```

# Allow Arg to be copied/cloned

---

_@LegNeato_

(I'm a rust n00b, let me know if I am doing something stupid)

It doesn't appear I can use a generic shared argument in multiple places / subcommands due to ownership.

``` rust
        let shared_arg = Arg::with_name("shared")
            .required(true)
            .help("This is shared");

        let matches = app
            .subcommand(foo_command.arg(shared_arg))
            .subcommand(bar_command.arg(shared_arg))
```

Ideally `Arg` would let me copy/clone so I could reuse shared definitions.


---

_Comment by @LegNeato on 2015-12-11 02:13_

Ah, just saw https://github.com/kbknapp/clap-rs/issues/354. Closing.


---

_Closed by @LegNeato on 2015-12-11 02:13_

---

_Comment by @kbknapp on 2015-12-11 06:09_

No worries,  we should have this implemented soon :wink:


---

_Comment by @lipanski on 2015-12-18 17:24_

@LegNeato you can use `Arg::from` to copy the argument around:

``` rust
let shared_arg = Arg::with_name("shared").required(true).help("This is shared");

let matches = app
            .subcommand(foo_command.arg(Arg::from(&shared_arg)))
            .subcommand(bar_command.arg(Arg::from(&shared_arg)));
```

not sure how orthodox this is, but it did the job for me.

and yes, would be lovely to have #354 :)


---
