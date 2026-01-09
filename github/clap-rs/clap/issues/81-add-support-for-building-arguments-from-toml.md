---
number: 81
title: Add support for building arguments from toml
type: issue
state: closed
author: kbknapp
labels:
  - A-builder
assignees: []
created_at: 2015-04-29T01:25:01Z
updated_at: 2018-08-02T03:29:38Z
url: https://github.com/clap-rs/clap/issues/81
synced_at: 2026-01-07T13:12:19-06:00
---

# Add support for building arguments from toml

---

_Issue opened by @kbknapp on 2015-04-29 01:25_

Splitting from #80 to track separately. Same method though, via a `feature` due to additional `toml-rs` dep.


---

_Label `feature request` added by @kbknapp on 2015-04-29 01:53_

---

_Label `optional dep` added by @kbknapp on 2015-05-04 04:15_

---

_Comment by @kbknapp on 2015-05-04 04:16_

This may get dropped as I'm not sure toml is the best way to represent an app structure unless it's dynamically generated somehow...TBD


---

_Label `postponed` added by @kbknapp on 2015-05-11 17:44_

---

_Label `P4: nice to have` added by @kbknapp on 2015-07-15 04:19_

---

_Label `C: args` added by @kbknapp on 2015-07-15 04:19_

---

_Label `C: app` added by @kbknapp on 2015-07-15 04:19_

---

_Label `D: intermediate` added by @kbknapp on 2015-07-15 04:19_

---

_Label `W: wont do` added by @kbknapp on 2015-07-27 12:28_

---

_Label `W: postponed` removed by @kbknapp on 2015-07-27 12:28_

---

_Closed by @kbknapp on 2015-07-27 12:28_

---

_Referenced in [clap-rs/clap#216](../../clap-rs/clap/issues/216.md) on 2015-09-02 18:27_

---

_Comment by @vks on 2016-09-07 21:14_

>  I'm not sure toml is the best way to represent an app structure unless it's dynamically generated somehow...

What exactly is the problem? TOML allows nesting as well, but keeps it flat:

``` yaml
subcommands:
    - subcmd:
        about: demos subcommands from yaml
        version: "0.1"
        author: Kevin K. <kbknapp@gmail.com>
        args:
            - scopt:
                short: B
                multiple: true
                help: example subcommand option
                takes_value: true
            - scpos1:
                help: example subcommand positional
                index: 1
```

would be

``` toml
[subcommands.subcmd]
about = "demos subcommands from yaml"
version = "0.1"
author = "Kevin K. <kbknapp@gmail.com>"

[subcommands.args.scopt]
short = "B"
multiple = true
help = "example subcommand option"
takes_value = true

[subcommands.args.scpos1]
help = "example subcommand positional"
index = 1
```


---

_Comment by @kbknapp on 2016-09-07 21:43_

@vks it's not that it doesn't support it, it's that unless you're dynamically generating that, it would be a pain to write. YAML on the other hand pretty short and easy to read / mentally parse. Granted that a personal opinion, and if it's not shared I'd be happy to merge TOML support as well...I just don't have the bandwidth right now to add it :stuck_out_tongue_winking_eye: 


---

_Comment by @vks on 2016-09-07 22:08_

@kbknapp I think if we used serde we would get both YAML and TOML (and others).


---

_Comment by @kbknapp on 2016-09-08 19:44_

Potentially yes, I haven't worked with serde in quite a while, so I'd have to hit the books again to see how much work it'd be. I'm not opposed to it by any means, other than I'd like to keep these features working on stable and not have to require nightly.


---
