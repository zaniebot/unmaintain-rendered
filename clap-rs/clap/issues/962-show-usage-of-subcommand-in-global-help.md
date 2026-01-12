```yaml
number: 962
title: Show usage of subcommand in global help
type: issue
state: closed
author: pitkley
labels:
  - C-enhancement
  - A-help
  - E-medium
  - S-wont-fix
assignees: []
created_at: 2017-05-16T16:09:01Z
updated_at: 2022-11-08T05:10:27Z
url: https://github.com/clap-rs/clap/issues/962
synced_at: 2026-01-12T16:14:10Z
```

# Show usage of subcommand in global help

---

_@pitkley_

I don't know if this feature exists, if so, I have not found it yet. What I'm looking for is a way to see the usage of a subcommand in the help shown by the `help` subcommand or the global `--help` flag. Compare the actual behavior to the expected behavior for an example.

-----

### Rust Version

`rustc 1.17.0 (56124baa9 2017-04-24)`

### Affected Version of clap

`2.24.1`

### Actual Behavior Summary

```console
$ ./test --help
test 

USAGE:
    test [SUBCOMMAND]

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

SUBCOMMANDS:
    help          Prints this message or the help of the given subcommand(s)
    subcommand    subcommand help
```

The `SUBCOMMANDS:` section only shows the subcommands that exist, not what parameters they require.

One has to ask for the specific help:

```console
$ ./test subcommand --help
test-subcommand 

USAGE:
    test subcommand <PARAM>

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

ARGS:
    <test>
```

### Expected Behavior Summary

```console
$ ./test --help
test 

USAGE:
    test [SUBCOMMAND]

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

SUBCOMMANDS:
    help
                  Prints this message or the help of the given subcommand(s)
    subcommand <PARAM>
                  subcommand help
```

Instead of having to look at every individual help output, the `SUBCOMMANDS:` section shows the `USAGE:` section of the subcommands.

### Sample Code

```rust
extern crate clap;
use clap::{Arg, App, SubCommand};

fn main() {
    let matches = App::new("test")
        .subcommand(SubCommand::with_name("subcommand")
                        .about("subcommand help")
                        .arg(Arg::with_name("PARAM").required(true)))
        .get_matches();
}

```

### Debug output

I can supply this at a later time if required.

---

_Comment by @kbknapp on 2017-05-19 15:10_

Thanks for the suggestion and all the details! 👍 

This feature doesn't exist (yet) could be added as an `AppSettings` variant, although my only concern is recursively following that chain with multiple nested subcommands.

Since I'm actively working on 3x I think this is something I'll try to add then, because the code clean ups I've done so far would make this far easier to implement, and/or people to contribute to. However, if someone wants this done immediately I'd be willing to assist in mentoring or answering questions on how this could be done.

---

_Label `C: help pages gen` added by @kbknapp on 2017-05-19 15:10_

---

_Label `C: subcommands` added by @kbknapp on 2017-05-19 15:10_

---

_Label `D: intermediate` added by @kbknapp on 2017-05-19 15:10_

---

_Label `M: mentored` added by @kbknapp on 2017-05-19 15:10_

---

_Label `P4: nice to have` added by @kbknapp on 2017-05-19 15:10_

---

_Label `T: enhancement` added by @kbknapp on 2017-05-19 15:10_

---

_Label `T: new setting` added by @kbknapp on 2017-05-19 15:10_

---

_Label `W: 3.x` added by @kbknapp on 2017-05-19 15:10_

---

_Comment by @pitkley on 2017-05-19 16:30_

Thanks for the reply.

1. Regarding your concerns on recursion: I personally think that the first usage level "down" would be enough, I wouldn't even be sure how to print another level without the output getting crowded or illegible.

   I.e. for this code:

   ```rust
   extern crate clap;
   use clap::{Arg, App, SubCommand};
   
   fn main() {
       let matches = App::new("test")
           .subcommand(SubCommand::with_name("subcommand")
                           .about("subcommand help")
                           .arg(Arg::with_name("PARAM").required(true))
                           .subcommand(SubCommand::with_name("nested")
                                           .arg(Arg::with_name("NESTED").required(true)))
           .get_matches();
   }
   ```

   I would simply expect the usage on `subcommand` to gain `[SUBCOMMAND]`:

   ```console
   $ ./test --help
   test 
   
   USAGE:
       test [SUBCOMMAND]
   
   FLAGS:
       -h, --help       Prints help information
       -V, --version    Prints version information
   
   SUBCOMMANDS:
       help
                     Prints this message or the help of the given subcommand(s)
       subcommand <PARAM> [SUBCOMMAND]
                     subcommand help
   ```

   But if you could think of a way to cleanly print even those subcommands, it might be worth while implementing too.

2. Regarding an implementation for 2.x: while I would try and take a stab at it, especially since you offered your assistance, I personally can wait for 3.x to either have that feature or help with an implementation once 3.x is ready for contributions.

---

_Comment by @kbknapp on 2018-07-22 02:26_

I'm going to close this for the time being, but may re-address it once 3.x is out

---

_Closed by @kbknapp on 2018-07-22 02:26_

---

_Label `W: after v3 release` added by @kbknapp on 2018-07-22 02:26_

---

_Comment by @CreepySkeleton on 2020-02-02 02:19_

Leaving it closed for the time being 

---

_Reopened by @pksunkara on 2021-05-26 11:07_

---

_Label `W: after v3 release` removed by @pksunkara on 2021-05-26 11:07_

---

_Label `W: 3.x` removed by @pksunkara on 2021-08-13 10:40_

---

_Referenced in [epage/clapng#73](../../epage/clapng/issues/73.md) on 2021-12-06 16:34_

---

_Label `C: subcommands` removed by @epage on 2021-12-08 20:16_

---

_Label `A-help` added by @epage on 2021-12-08 20:16_

---

_Label `T: new setting` removed by @epage on 2021-12-08 20:38_

---

_Referenced in [clap-rs/clap#1334](../../clap-rs/clap/issues/1334.md) on 2021-12-08 21:50_

---

_Label `P4: nice to have` removed by @epage on 2021-12-09 17:01_

---

_Label `D: medium` removed by @epage on 2021-12-09 17:01_

---

_Label `S-waiting-on-mentor` added by @epage on 2021-12-09 17:01_

---

_Referenced in [clap-rs/clap#1398](../../clap-rs/clap/issues/1398.md) on 2021-12-09 19:29_

---

_Comment by @buck-ross on 2022-02-09 03:49_

Has there been any update on this? I see the issue has been re-opened, but there doesn't seem to be any follow-up that I can see.

---

_Comment by @epage on 2022-02-09 15:31_

No updates.  A lot of our focus right now is in other areas like API clean up, shrinking binary size, fixing bugs, etc.

I am curious if the solution proposed in https://github.com/clap-rs/clap/issues/1334 might be a viable alternative to meet the same needs.  If we can find more common solutions to problems, it can help us meet our goal of keeping binary sizes and compile times down,.

---

_Comment by @epage on 2022-11-08 05:10_

I lean in favor of #1334 instead and am closing in favor of that as usually someone is either caring about one command at a time or with them flattened and the build-time / binary size cost of supporting both wouldn't be great.

If people make the case for this being needed, I'm ok with re-opening this.

---

_Closed by @epage on 2022-11-08 05:10_

---

_Label `S-waiting-on-mentor` removed by @epage on 2022-11-08 05:10_

---

_Label `S-wont-fix` added by @epage on 2022-11-08 05:10_

---
