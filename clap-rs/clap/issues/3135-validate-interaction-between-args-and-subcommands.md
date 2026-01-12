```yaml
number: 3135
title: Validate interaction between args and subcommands
type: issue
state: open
author: epage
labels:
  - C-enhancement
  - A-validators
  - S-waiting-on-design
assignees: []
created_at: 2021-12-09T16:36:21Z
updated_at: 2022-04-15T12:23:54Z
url: https://github.com/clap-rs/clap/issues/3135
synced_at: 2026-01-12T16:14:14Z
```

# Validate interaction between args and subcommands

---

_@epage_

<a href="https://github.com/JayiceZ"><img src="https://avatars.githubusercontent.com/u/49588871?v=4" align="left" width="96" height="96" hspace="10"></img></a> **Issue by [JayiceZ](https://github.com/JayiceZ)**
_Monday Dec 06, 2021 at 10:56 GMT_
_Originally opened as https://github.com/TeXitoi/structopt/issues/515_

----

Hi, I am new to structopt. And I am not sure is it a structopt issue or a clap issue? Here is the situation I met:

I defined it, like:
```
struct Opt {
    #[structopt(long)]
    cat: Option<String>,

    #[structopt(long)]
    dog: Option<String>,

    #[structopt(long)]
    host: Option<String>,

    #[structopt(subcommand)]
    cmd: Option<Cmd>,
}

enum Cmd {
    Test {
        #[structopt(long)]
        dir: Option<String>,
    },
    Debug{
        #[structopt(long)]
        remote: Option<String>,
    },
}
```

And what I want is: When `Test` SubCommand is set, one of `cat` and `dog` should be set(or it means that they are in a `Group`, and the group is required when `Test` is set); And when `Debug` is set, `host` should be set.

I have read the doc and examples, but I am not sure how to implement this function? 😭


---

_Comment by @epage on 2021-12-09 16:36_

<a href="https://github.com/epage"><img src="https://avatars.githubusercontent.com/u/60961?v=4" align="left" width="48" height="48" hspace="10"></img></a> **Comment by [epage](https://github.com/epage)**
_Monday Dec 06, 2021 at 12:46 GMT_

----

From what I've seen so far, clap allows validation of relationship of args and groups but subcommands are effectively in another namespace, preventing them from interacting with that system.

In clap3, there is `App::error` to create clap-like errors for custom validation.  There is also interest in [generalizing clap's mechanisms](https://github.com/clap-rs/clap/discussions/2832) so people are less likely to run into hurdles like this.


---

_Label `A-validators` added by @epage on 2021-12-09 16:42_

---

_Label `C-enhancement` added by @epage on 2021-12-09 16:42_

---

_Renamed from "Question: how the fields are required by subcommand" to "Validate interaction between args and subcommands" by @epage on 2021-12-09 16:42_

---

_Comment by @epage on 2021-12-13 22:40_

This is also related to #1204

---

_Label `S-waiting-on-design` added by @epage on 2021-12-13 22:40_

---

_Comment by @epage on 2022-04-15 12:23_

See #3008 which should allow users to do this themselves

---

_Referenced in [clap-rs/clap#3008](../../clap-rs/clap/issues/3008.md) on 2022-04-15 12:24_

---

_Referenced in [clap-rs/clap#4442](../../clap-rs/clap/issues/4442.md) on 2022-11-02 15:19_

---
