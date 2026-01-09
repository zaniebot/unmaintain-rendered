---
number: 1607
title: "[Question/Feature] Is There a Way to Make Args that Behave Like `--help`"
type: issue
state: closed
author: zicklag
labels: []
assignees: []
created_at: 2019-12-12T03:10:57Z
updated_at: 2021-05-26T17:01:10Z
url: https://github.com/clap-rs/clap/issues/1607
synced_at: 2026-01-07T13:12:19-06:00
---

# [Question/Feature] Is There a Way to Make Args that Behave Like `--help`

---

_Issue opened by @zicklag on 2019-12-12 03:10_

I have a situation where I have created a custom `--doc` flag that works very similarly to the `--help` flag except it triggers a documentation pager like a manpage. The problem I have is that I have "required" arguments that should not be required when the `--doc` flag is present. I could usually use `required_unless("doc")`, but that doesn't work when the `--doc` flag is passed to a subcommand and the required argument is in a super-command. For example with this app:

```rust
App::new("test-app")
    .arg(Arg::with_name("testarg")
        .required_unless("doc")) // Doesn't work because `doc` is in a subcommand
    .subcommand(App::new("subcommand")
        .arg(Arg::with_name("doc")
            .long("doc"))
```

In the above app I have no way of triggering the `--doc` argument without passing `testarg` because `testarg` is required.

What I need is a way to "pre-check" for `--doc` flags and handle those first, only continuing like normal after I have verified that I don't need to handle the `--doc` flag. This is just like the `--help` flag works, but it is built-in and I can't make another argument follow that behavior.

---

_Comment by @CreepySkeleton on 2020-02-01 11:04_

I think this is solved by `exclusive`. I'll check later.

---

_Assigned to @CreepySkeleton by @CreepySkeleton on 2020-02-01 11:04_

---

_Label `not-sure` added by @CreepySkeleton on 2020-02-01 14:28_

---

_Comment by @CreepySkeleton on 2020-02-06 07:57_

OK, it feels a lot like `exclusive` indeed, and yet it's a bit different. `exclusive` is about "this option conflicts with every other" but the question is about "this option terminates processing right away".

---

_Label `cc: CreepySkeleton` removed by @CreepySkeleton on 2020-02-06 07:58_

---

_Label `C: options` added by @CreepySkeleton on 2020-02-06 07:58_

---

_Label `T: new feature` added by @CreepySkeleton on 2020-02-06 07:58_

---

_Label `W: 3.x` added by @CreepySkeleton on 2020-02-06 07:58_

---

_Unassigned @CreepySkeleton by @CreepySkeleton on 2020-02-06 07:58_

---

_Comment by @pksunkara on 2020-02-06 07:58_

I think `.global(true).exclusive(true)` should handle this case.

---

_Added to milestone `3.1` by @pksunkara on 2020-02-06 07:59_

---

_Comment by @CreepySkeleton on 2020-02-06 08:28_

Not really. `exclusive` will go boom if the arg is not the only one, but the expected behavior is to "ignore everything else if the arg is present".

---

_Comment by @bjeanes on 2020-05-19 01:40_

I also want to do this in order to tap into the completions logic and allow users to do something like:

```
my-cli --completions bash >> ~/.bashrc
```

However, `clap_generate` is (understandably) underdocumented at the moment, so y'all might already have plans on standardising how shells get access to those completions.

---

_Comment by @kbknapp on 2020-05-26 22:27_

Traditionally the only way to do something like this was to make `--doc` a global argument, use `required_unless` and then in consumer code check for `--doc` prior to actually doing any real work.

Since `exclusive` is a 3.x addition, we could downgrade it to *override* all other arguments instead of hard conflict and error. Reason being, we can't enforce process ending (minus error) without some kind of `handler` which is still undecided. cc @CreepySkeleton @pksunkara 

---

_Removed from milestone `3.1` by @pksunkara on 2020-05-26 22:30_

---

_Added to milestone `3.0` by @pksunkara on 2020-05-26 22:30_

---

_Label `W: 3.x` removed by @pksunkara on 2020-10-10 19:15_

---

_Comment by @pksunkara on 2020-10-10 19:18_

I think https://docs.rs/clap/3.0.0-beta.2/clap/enum.AppSettings.html#variant.SubcommandsNegateReqs will solve this issue

---

_Assigned to @ldm0 by @ldm0 on 2021-03-13 13:15_

---

_Comment by @pksunkara on 2021-05-26 11:54_

@zicklag Can you try by making the `doc` arg `global(true)` on latest master?

---

_Removed from milestone `3.0` by @pksunkara on 2021-05-26 11:54_

---

_Added to milestone `3.0` by @pksunkara on 2021-05-26 11:54_

---

_Comment by @zicklag on 2021-05-26 16:59_

@pksunkara That works!

```rust
use clap::*;

fn main() {
    let matches = App::new("test-app")
        .arg(Arg::new("doc").global(true).long("doc"))
        .subcommand(App::new("subcommand").arg(Arg::new("testarg").required_unless_present("doc")))
        .get_matches();

    match matches.subcommand() {
        Some(("subcommand", matches)) => {
            println!("running subcommand");
            if matches.is_present("doc") {
                println!("print docs!!");
            } else {
                println!("do stuff");
            }
        }
        None => {
            println!("running default command");
            if matches.is_present("doc") {
                println!("print docs!");
            } else {
                println!("do stuff");
            }
        }
        _ => unreachable!(),
    }
}
```

I'd call this complete. Thanks!

---

_Closed by @zicklag on 2021-05-26 17:01_

---
