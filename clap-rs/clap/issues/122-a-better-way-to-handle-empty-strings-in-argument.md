---
number: 122
title: A better way to handle empty strings in argument values
type: issue
state: closed
author: Byron
labels:
  - C-enhancement
assignees: []
created_at: 2015-05-17T14:26:36Z
updated_at: 2015-05-18T01:40:54Z
url: https://github.com/clap-rs/clap/issues/122
synced_at: 2026-01-10T01:26:23Z
---

# A better way to handle empty strings in argument values

---

_Issue opened by @Byron on 2015-05-17 14:26_

Currently it is possible to pass an empty string to arguments defined like this (maybe it's a valid value to all of them):

``` Rust
Arg::new("from-stop")
             .required(true)
```

On the command-line, it's now possible to pass in an empty string like so:

``` bash
$ trains shortest-route A ""
```

I believe it's too far fetched to disallow empty strings in general, but it would be great if I could set a flag (maybe even it's default on) on the `Arg` type to let the parser handle empty values for me.

This could look like this:

``` Rust
Arg::new("to-stop")
       .allow_empty_values(false)
       .required(true)
```

If such a feature does not exist on parser level, each client will have to implement its own check.


---

_Comment by @kbknapp on 2015-05-17 16:22_

I think this is a valid suggestion. Let me play with an implementation and I'll post back with the solution.


---

_Assigned to @kbknapp by @kbknapp on 2015-05-17 16:22_

---

_Label `enhancement` added by @kbknapp on 2015-05-17 16:22_

---

_Comment by @kbknapp on 2015-05-17 17:04_

I have the initial work on this done, but I'm going to roll it into a larger update tonight since I don't have time to finish it until later today. I'll posy back later today once its complete ;)


---

_Comment by @Byron on 2015-05-17 17:23_

Thanks! No reason to hurry though, as I am just playing it safe and used
'stdin' instead of '-' for the final toy project.
And Google.rs will use this capability once available.
On Sun 17 May 2015 at 19:04 Kevin K. notifications@github.com wrote:

> I have the initial work on this done, but I'm going to roll it into a
> larger update tonight since I don't have time to finish it until later
> today. I'll posy back later today once its complete ;)
> 
> —
> Reply to this email directly or view it on GitHub
> https://github.com/kbknapp/clap-rs/issues/122#issuecomment-102823350.


---

_Closed by @kbknapp on 2015-05-17 22:23_

---

_Comment by @kbknapp on 2015-05-17 22:27_

Should be good now :) Use v0.9.1 on crates.io


---
