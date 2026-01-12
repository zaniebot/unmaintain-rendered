```yaml
number: 782
title: "Feature request: allow specifying argument multiple value terminator"
type: issue
state: closed
author: vi
labels:
  - A-parsing
assignees: []
created_at: 2016-12-21T22:49:08Z
updated_at: 2017-01-03T04:00:46Z
url: https://github.com/clap-rs/clap/issues/782
synced_at: 2026-01-12T16:14:09Z
```

# Feature request: allow specifying argument multiple value terminator

---

_@vi_

### Rust Version

`rustc 1.15.0-nightly (0bd2ce62b 2016-11-19)`

### Affected Version of clap

2.19.2

### Description

clap should be able to accept array of command line arguments, delimited by `;` or `--` or just end of command line arguments, suitable to be used for [spawning a command with arguments](https://doc.rust-lang.org/std/process/struct.Command.html#args.v) specified by user without any escaping/unescaping, like in [find(1)](https://linux.die.net/man/1/find) or [xargs(1)](https://linux.die.net/man/1/xargs).

---

_Comment by @kbknapp on 2016-12-27 04:13_

clap already supports `--` for ending argument parsing, there are also things like [`AppSettings::TrailingVarArg`](https://docs.rs/clap/2.19.2/clap/enum.AppSettings.html#variant.TrailingVarArg) which is essentially `--` but automatically for the final positional argument.

Is that what you're talking about?

---

_Label `T: RFC / question` added by @kbknapp on 2016-12-27 04:13_

---

_Comment by @vi on 2016-12-27 12:22_

This works, but has limitations:

1. It accepts only one array. How do I accept two VarArgs?
2. It is expected to work only at the end;
3. It is tied to positional arguments, not named arguments like with `myprog posarg1 posarg2 --named-vararg nva1 nva2 --nva3 nva4 \; posarg3 posarg4`
4. It may require additional parsing for recognizing the delimiter in application after clap returned. If there is already rich command line interface and positional arguments already have assigned meanings. TrailingVarArg may even already used.

Is this use case (like find(1) in Rust) too tricky and justifies doing additional command line parsing outside clap, inside application?

---

_Comment by @kbknapp on 2016-12-27 13:28_

Can you show me an example invocation of what you're trying to do?

```
$ prog --opt one,two,three;four,five,six
```

Is already supported. The result is two values of `opt` `one,two,three` and `four,five,six` which you could then split on `,` to create vecs.

---

_Comment by @vi on 2016-12-27 13:44_

It requires escaping of `,` or `;`. It does not look "transparent" for shell scripting.

It should be usable in something like this `script.sh`:

```
#!/bin/sh
prog --delimiter ';' --opt1 some_script1 "$@" ';' --opt2 some_script2 "$@" ';'
```

Arguments supplied to to `script.sh` are expected to to delivered to `some_script1` and `some_script2` as is, without any escaping/unescaping. It should not remove quotation marks. Empty arguments should remain empty. Exception: argument that consists solely of the delimiter `;`. The array of arguments gets into `script.sh`, gets duplicated by usage of two `"$@"`s, passed as is to `prog`, travels though it (remaining as array, not serialized to string) and arrives into `some_script1` and  `some_script2` as array.

---

_Comment by @kbknapp on 2016-12-28 02:52_

Ah ok, I think I understand now. Correct me if I'm wrong (again 😜 ), but you're looking for a way to *end* an array of values with a specified character? So really, it's a way to specify the array *terminator*, not *delimiter*.

I.e. you want `$ prog --prog-arg1 val1 val2 --passed-through-flag -o \; --prog-arg2` where only `--prog-arg1` and `--prog-arg2` are arguments of `prog`, but all the others are essentially captured values inside `--prog-arg1`

Currently, this isn't supported, as you've asserted. It would be an easy additoin though. There are a few issues slightly higher on the bucket list first, but I should be able to get this shortly.

---

_Label `C: args` added by @kbknapp on 2016-12-28 02:52_

---

_Label `C: parsing` added by @kbknapp on 2016-12-28 02:52_

---

_Label `D: intermediate` added by @kbknapp on 2016-12-28 02:52_

---

_Label `P4: nice to have` added by @kbknapp on 2016-12-28 02:52_

---

_Label `T: new feature` added by @kbknapp on 2016-12-28 02:52_

---

_Label `W: 2.x` added by @kbknapp on 2016-12-28 02:52_

---

_Label `T: RFC / question` removed by @kbknapp on 2016-12-28 02:52_

---

_Added to milestone `2.20.0` by @kbknapp on 2016-12-28 02:52_

---

_Assigned to @kbknapp by @kbknapp on 2016-12-28 02:52_

---

_Comment by @vi on 2016-12-28 14:31_

Yes, it is array terminator.

> It would be an easy addition though.

User-overridable separator may complicate this a bit. With fixed separator it is tricky to "recursively re-enter" the same program. Also currently named options don't accept array values. Also non-final positional arguments shoud be able to handle this: `prog pos1 trickypos2 pos2arg1 pos2arg2 \; pos3 pos4`. In this usage `pos1` is likely to be the user-specified delimiter.

---

_Comment by @kbknapp on 2016-12-28 17:00_

> User-overridable separator may complicate this a bit. 

It wouldn't be that hard depending on how exactly you define the CLI.

> Also currently named options don't accept array values. 

What do you mean by array values?

```
$ prog --arg val1 val2 val3 val4
```

This gives `arg` the values `["val1", "val2", "val3", "val4"]`

> `prog pos1 trickypos2 pos2arg1 pos2arg2 \; pos3 pos4`. In this usage `pos1` is likely to be the user-specified delimiter.

You'd have to do a double parse of the arguments, but since that takes nanoseconds, it's not a big deal.

Suppose this get implemented, you'd just do something like

```rust
let pre_matches = App::new("prog")
    .arg(Arg::with_name("pos1")
        .required(true))
    .arg(Arg::with_name("dummy-arg")
        .multiple(true)
        .allow_hyphen_values(true))
    .get_matches();

let matchse = App::new("prog")
    .arg(Arg::with_name("pos1")
        .required(true))
    .arg(Arg::with_name("pass-through")
        .long("pass")
        .multiple(true)
        .terminator(pre_matches.value_of("pos1").unwrap())
        .allow_hyphen_values(true))
    .arg(Arg::with_name("pos2"))
    .arg(Arg::with_name("pos3"))
    .get_matches();
```

Then you could easily run, 

```
$ prog \; --pass other --args -to pass --through \; pos2 pos3

OR

$ prog z --pass other --args -to pass --through z pos2 pos3
```

You could also do an optional terminator like so:

```rust
let pass_through_arg = Arg::with_name("pass-through")
        .long("pass")
        .multiple(true)
        .allow_hyphen_values(true);

let app = App::new("prog")
    .arg(Arg::with_name("term")
        .long("terminator")
        .takes_value(true))
    .arg(Arg::with_name("pos1"))
    .arg(Arg::with_name("pos2"));

let mut matches = app
    .arg(pass_through_arg.terminator(";")) // Set a default terminator of ;
    .get_matches();

matches = if let Some(term) = matches.value_of("term") {
    app.arg(pass_through_arg.terminator(term))
        .get_matches()
} else {
    matches
};
```

Then you could run, 

```
$ prog --pass other --args -to pass --through \; pos1 pos2

OR

$ prog --terminator z --pass other --args -to pass --through z pos1 pos2
```

---

_Comment by @vi on 2016-12-28 17:30_

Looks reasonable.

---

_Comment by @kbknapp on 2016-12-28 18:03_

OK. The only thing not implemented yet is the `Arg::terminator` part which should be easy. Named options already support arrays. :+1:

---

_Comment by @vi on 2016-12-28 20:45_

Are you planning to implement this yourself of will wait for a pull request?

---

_Comment by @kbknapp on 2016-12-28 22:54_

I plan on implementing it once I finish the current issue I'm working on. Depending on what sort of free time I've got it could be done and merged within the next few days. Of course, if someone beats me to it I'll gladly accept :wink:

---

_Renamed from "Feature request: accepting delimited array of arguments" to "Feature request: allow specifying argument multiple value terminator" by @kbknapp on 2016-12-29 00:56_

---

_Closed by @homu on 2017-01-02 20:59_

---

_Comment by @vi on 2017-01-03 00:48_

Should overridable terminator by double parsing pattern be documented somewhere as a best practice (to avoid flourishing of hardcoded separator command line APIs which are harmful in the long term)?

---

_Comment by @kbknapp on 2017-01-03 04:00_

@vi it wouldn't hurt to document it somewhere, perhaps in the `examples/`, and making a call out to it in the docs page? I think it's a little too specific for the actual docs page, but I wouldn't mind if there's a callout to go look at it if applicable.

I don't think there'll be a huge rush of hard-coded terminators though. 😜 

Once I get the big 2.20 update out, I can try to put an example together, unless someone beats me to it.

---

_Referenced in [clap-rs/clap#1125](../../clap-rs/clap/issues/1125.md) on 2017-12-08 23:13_

---

_Referenced in [epage/clapng#83](../../epage/clapng/issues/83.md) on 2021-12-06 16:37_

---
