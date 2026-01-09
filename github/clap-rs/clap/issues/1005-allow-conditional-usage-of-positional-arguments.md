---
number: 1005
title: Allow conditional usage of positional arguments
type: issue
state: closed
author: kieraneglin
labels: []
assignees: []
created_at: 2017-07-20T03:05:03Z
updated_at: 2018-08-02T03:30:09Z
url: https://github.com/clap-rs/clap/issues/1005
synced_at: 2026-01-07T13:12:19-06:00
---

# Allow conditional usage of positional arguments

---

_Issue opened by @kieraneglin on 2017-07-20 03:05_

This is version agnostic, but I am using the YAML option.

I am working on an app [here](https://github.com/kieraneglin/diecast) for reference.

___

Simply, I want to be able to change the possible arguments and whether they're required or not based on the value of a preceding positional argument. 

Here's an example.  The program's name will be `program`.  The first argument will be the subcommand I want, `command`.  The second argument will be the `action` I want to perform.  `action` can be either `save` or `load`.

**If `program command save ...`**
Usage: `program command save <namespace: required> <name: required> <url: required>`

**If `program command load ...`**
Usage: `program command load <url: required>`

___

I hope that was clear enough.  I don't believe this functionality is present already (at least not when using a YAML file).

Thank you!


---

_Comment by @kbknapp on 2017-10-04 02:38_

Sorry for long wait, I've been swamped and just trying to get 3.x out the door :disappointed: 

You could use [`Arg::requires_if`](https://docs.rs/clap/2.26.2/clap/struct.Arg.html#method.requires_if) to make the other positionals required if the value equals either `save` or `load`. This could be used in conjunction with [`Arg::possible_values`](https://docs.rs/clap/2.26.2/clap/struct.Arg.html#method.possible_values).

If that doesn't work, make them flags, and use the [`Arg::requires`](https://docs.rs/clap/2.26.2/clap/struct.Arg.html#method.requires) for each.

Finally, you could just make `save|load` separate subcommands.

I'm gonna close this issue just to clean up the issue tracker a little, but feel free to reopen if this needs more addressing :+1: 

---

_Label `T: RFC / question` added by @kbknapp on 2017-10-04 02:39_

---

_Closed by @kbknapp on 2017-10-04 02:39_

---
