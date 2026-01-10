---
number: 1034
title: Support for default value in args_from_usage?
type: issue
state: closed
author: netvl
labels: []
assignees: []
created_at: 2017-08-21T02:31:16Z
updated_at: 2020-04-12T10:09:28Z
url: https://github.com/clap-rs/clap/issues/1034
synced_at: 2026-01-10T01:26:41Z
---

# Support for default value in args_from_usage?

---

_Issue opened by @netvl on 2017-08-21 02:31_

Right now I have the following thing in many of my projects:
```rust
        .arg(
            Arg::from_usage("-c, --config=[FILE] 'Path to the configuration file'")
                .default_value("~/.config/sshc/config.toml")
        )
        .args_from_usage(
            "-p, --profile=[PROFILE] 'Run the specified profile immediately'
             -d, --dry-run 'Just print the executed command if the profile is specified'"
        )
```

Here I want to specify a default value for one of the arguments. However, I cannot define the default value if I use the "from usage" format of arguments definitions, so since I use it for all other arguments it quite nastily breaks the uniformity of the arguments definitions.

Ideally, I would like to write the above as follows (or in a similar way):
```rust
        .args_from_usage(
            "-c, --config=[FILE] 'Path to the configuration file [default: ~/.config/sshc/config.toml]'
             -p, --profile=[PROFILE] 'Run the specified profile immediately'
             -d, --dry-run 'Just print the executed command if the profile is specified'"
        )
```

---

_Comment by @kbknapp on 2017-08-21 22:24_

I like this idea! I do the same in many of my programs so I know it'd be useful. I'm doing my best to get 3.x out the door, and in order to do that I need to put digital blinders on, which means I'll have to wait until 3.x is in a releasable state before adding this feature. It shouldn't be too much longer now though!

---

_Label `C: usage string parser` added by @kbknapp on 2017-08-21 22:24_

---

_Label `D: intermediate` added by @kbknapp on 2017-08-21 22:24_

---

_Label `T: new feature` added by @kbknapp on 2017-08-21 22:24_

---

_Label `W: 3.x` added by @kbknapp on 2017-08-21 22:24_

---

_Referenced in [clap-rs/clap#1522](../../clap-rs/clap/pulls/1522.md) on 2019-07-18 20:45_

---

_Referenced in [clap-rs/clap#1530](../../clap-rs/clap/pulls/1530.md) on 2019-08-02 20:35_

---

_Closed by @Dylan-DPC-zz on 2020-01-03 11:48_

---

_Label `C: usage strings` added by @pksunkara on 2020-04-12 10:09_

---

_Label `C: usage string parser` removed by @pksunkara on 2020-04-12 10:09_

---
