```yaml
number: 439
title: Is a required ArgGroup only for the current SubCommand, or global?
type: issue
state: closed
author: matthiasbeyer
labels:
  - C-enhancement
  - A-docs
assignees: []
created_at: 2016-03-06T11:39:34Z
updated_at: 2018-08-02T03:29:48Z
url: https://github.com/clap-rs/clap/issues/439
synced_at: 2026-01-12T16:14:09Z
```

# Is a required ArgGroup only for the current SubCommand, or global?

---

_@matthiasbeyer_

This is a bit unclear in the documentation (at least for me):

If I have an `ArgGroup` which is `::required(true)` - does this mean this ArgGroup is required _globally_ and a user _must always_ provide these arguments? Or does it only apply to the `SubCommand` instance level above the `ArgGroup` instance?

E.G.:

``` rust
App::new()
// ... args and subcommands
.subcommand(
    SubCommand::with_name("subcmd")
        .arg(Arg::with_name("foo")) // and more settings
        .arg(Arg::with_name("bar")) // and more settings
        .group(ArgGroup::new("foobar").args(&["foo", "bar"]).required(true))
)
```

Is "foo" or "bar" now enforced for `app <flags>`, or only for `app subcmd <flags>`?

---

If it is enforced globally: Feature request: flag for forcing `ArgGroup`s only for the enclosing `SubCommand`.


---

_Comment by @kbknapp on 2016-03-08 12:46_

@matthiasbeyer Sorry it took me so long to respond! 

It means that _either_ `foo` or `bar` **must** be present at runtime. It's a way of making one of the two args, but not both, be present. You can think of it like a "Make this arg required, if none of these other args are present" type setting.

Edit: And it's only for that subcommand instance, not the globally :wink:


---

_Label `T: RFC / question` added by @kbknapp on 2016-03-08 12:47_

---

_Renamed from "[Question]: Is a required ArgGroup  required only for the SubCommand or globally?" to "Is a required ArgGroup only for the current SubCommand, or globally?" by @kbknapp on 2016-03-08 12:48_

---

_Comment by @matthiasbeyer on 2016-03-08 12:48_

> And it's only for that subcommand instance, not the globally

Would be nice to state this explicitely in the docs! :+1: 


---

_Renamed from "Is a required ArgGroup only for the current SubCommand, or globally?" to "Is a required ArgGroup only for the current SubCommand, or global?" by @kbknapp on 2016-03-08 12:48_

---

_Comment by @kbknapp on 2016-03-08 12:49_

I'll mark this a docs update then, thanks for filing! :+1: 


---

_Label `T: enhancement` added by @kbknapp on 2016-03-08 12:49_

---

_Label `C: docs` added by @kbknapp on 2016-03-08 12:49_

---

_Label `D: easy` added by @kbknapp on 2016-03-08 12:49_

---

_Label `P3: want to have` added by @kbknapp on 2016-03-08 12:49_

---

_Label `W: 2.x` added by @kbknapp on 2016-03-08 12:49_

---

_Label `T: RFC / question` removed by @kbknapp on 2016-03-08 12:49_

---

_Comment by @matthiasbeyer on 2016-03-08 12:49_

Thanks for your awesome support!


---

_Closed by @homu on 2016-03-10 02:42_

---
