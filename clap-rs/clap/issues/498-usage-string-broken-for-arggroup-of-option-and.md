```yaml
number: 498
title: Usage string broken for ArgGroup of option and argument
type: issue
state: closed
author: joshtriplett
labels:
  - C-bug
  - C-enhancement
assignees: []
created_at: 2016-05-07T02:51:39Z
updated_at: 2020-04-12T10:09:26Z
url: https://github.com/clap-rs/clap/issues/498
synced_at: 2026-01-12T16:14:09Z
```

# Usage string broken for ArgGroup of option and argument

---

_@joshtriplett_

Consider the following subcommand definition:

``` rust
    SubCommand::with_name("set-base")
        .about("Set the base commit for the patch series")
        .arg(Arg::with_name("base").help("Base commit").required(false))
        .arg_from_usage("-d, --delete 'Remove the base commit information'")
        .group(ArgGroup::with_name("base_or_delete")
            .args(&["base", "delete"])
            .required(true)),
```

(The same issue applies if I use `arg_from_usage` for `base`.)

The usage string looks like this:

```
    set-base [FLAGS] [[base]|--delete] [ARGS]
...
    [base]    Base commit
```

Note the extra square brackets around `base`; that part should look like `base|--delete`.

Also, the alternative between `base` and `--delete` should probably have something other than square brackets around it, since one of those two is required.


---

_Comment by @joshtriplett on 2016-05-07 02:58_

An interesting further complication: if I add a `.conflicts_with("delete")` to base, and pass both, I get the following error:

```
error: The argument '--delete' cannot be used with '[base]'

USAGE:
    git series set-base [base] --delete [[base]|--delete] [[base]|--delete]
```

That seems like two too many of each argument.


---

_Comment by @joshtriplett on 2016-05-07 06:57_

Using `.required_unless("delete")` instead of `.conflicts_with("delete")` produces slightly better results, but still a bit wrong:

```
USAGE:
    set-base [FLAGS] <base> [<base>|--delete]
```

`<base>` shouldn't appear twice.


---

_Comment by @kbknapp on 2016-05-07 22:29_

Thanks for taking the time to file these! I agree with all of the above, so it's actually like 3 bugs in 1, or a formating one for the first comment which is an easy fix that I agree with too.

I'll post back here as they're completed then put out 2.4.1.


---

_Label `T: bug` added by @kbknapp on 2016-05-09 00:11_

---

_Label `T: enhancement` added by @kbknapp on 2016-05-09 00:11_

---

_Label `P2: need to have` added by @kbknapp on 2016-05-09 00:11_

---

_Label `D: intermediate` added by @kbknapp on 2016-05-09 00:11_

---

_Label `W: 2.x` added by @kbknapp on 2016-05-09 00:11_

---

_Label `C: arg groups` added by @kbknapp on 2016-05-09 00:11_

---

_Label `C: usage strings` added by @kbknapp on 2016-05-09 00:11_

---

_Comment by @kbknapp on 2016-05-09 00:12_

Tracking for issues:
- [x] Duplicated args in usage string
- [x] formatting for required arg groups
- [x] formatting for positional arguments


---

_Assigned to @kbknapp by @kbknapp on 2016-05-09 00:12_

---

_Comment by @kbknapp on 2016-05-09 20:01_

@joshtriplett All tasks done, this should be merged with #499 at which point I'll put out v2.4.1


---

_Closed by @homu on 2016-05-10 00:12_

---

_Comment by @kbknapp on 2016-05-10 02:50_

due to some crazy test issues and refactoring woes, the next version is _actually_ v2.4.3 and is out on crates.io. This is the version that has the changes from this issue :wink: Sorry for the confusion!


---

_Label `C: usage strings` added by @pksunkara on 2020-04-12 10:09_

---

_Label `C: usage string parser` removed by @pksunkara on 2020-04-12 10:09_

---
