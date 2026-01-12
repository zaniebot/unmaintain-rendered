```yaml
number: 586
title: Optional option uses angle brackets in OPTIONS list
type: issue
state: closed
author: joshtriplett
labels:
  - A-help
assignees: []
created_at: 2016-07-18T22:25:46Z
updated_at: 2018-08-02T03:29:51Z
url: https://github.com/clap-rs/clap/issues/586
synced_at: 2026-01-12T16:14:09Z
```

# Optional option uses angle brackets in OPTIONS list

---

_@joshtriplett_

If I write the following:

``` rust
.arg_from_usage("--in-reply-to [Message-Id] 'Make the first mail a reply to the specified Message-Id'")
```

the corresponding --help output looks like this:

```
        --in-reply-to <Message-Id>    Make the first mail a reply to the specified Message-Id
```

Note the angle brackets, which normally indicate a required argument.

For an optional option, which is the normal case, I'd suggest leaving out both square brackets and angle brackets; the usage string indicates what options you have to supply.  The --help output should look like this:

```
        --in-reply-to Message-Id    Make the first mail a reply to the specified Message-Id
```


---

_Comment by @kbknapp on 2016-08-20 21:59_

@joshtriplett Do you mean if the `Message-Id` itself is optional, or the entire `--in-reply-to` is optional?


---

_Label `T: RFC / question` added by @kbknapp on 2016-08-20 21:59_

---

_Label `C: args` added by @kbknapp on 2016-08-20 21:59_

---

_Label `C: help message` added by @kbknapp on 2016-08-20 21:59_

---

_Label `W: 2.x` added by @kbknapp on 2016-08-20 21:59_

---

_Comment by @joshtriplett on 2016-08-20 22:02_

The entire --in-reply-to=some-message-id is optional.  (Wrapping the square brackets around the whole thing, `[--in-reply-to Message-Id]`, seems to produce a different issue; in that case, clap seems to treat it as a positional argument with a strange name.)


---

_Comment by @kbknapp on 2016-08-20 23:12_

Ah ok, I understand now. I have mixed feelings on this, particularly because I _do_ like not having the option to not place any brackets in the _declaration_ (i.e. `arg_from_usage`), but in the help text I'm mixed because the `<message-id>` is _required_ for `--in-reply-to`, even though `--in-reply-to` isn't required as a whole.

Perhaps in a usage string it'd be possible to place the whole thing in square brackets, and leave off the brackets for the value?


---

_Comment by @joshtriplett on 2016-08-20 23:24_

If the option appears in the usage string displayed at the top of the help, the square brackets should go around the entire thing, yes: `[--in-reply-to Message-Id]`.  Down in the list of options, however, that seems excessive, since almost all `--options` (as opposed to positional parameters) default to optional.  Mandatory `--options` seem like the uncommon case that needs more explicit marking.  

In `args_from_usage`, I've tried specifying `"[--in-reply-to Message-Id] '...help...'"` , and that produced unexpected results as mentioned above (clap thought that defined a positional argument with a strange name).

Ideally, all of the following should have some consistent syntax in both `args_from_usage` and in the generated help, documented in the clap documentation:
- A required `--option` with no value.
- A required `--option=value` with required value.
- A required `--option=value` with optional value.
- An optional `--option` with no value.
- An optional `--option` with required value (if you specify the option, you must specify its value).
- An optional `--option` with optional value.

So far, I've had to infer some of the syntax for `args_from_usage`, possibly incorrectly.


---

_Comment by @kbknapp on 2016-10-25 17:59_

Not exactly what you're asking for in this issue, but what are you feelings when displaying a help message with an option that takes 0 or more values?

Currently in clap it gets displayed like this:

```
OPTIONS:
    -o, --option <value>    help message
```

But I'm wanting to move to something like:

```
OPTIONS:
    -o, --option [<value>]    help message
```

I can't necessarily remove the angled brackets without some larger refactoring, but adding square brackets would be a very quick change.


---

_Comment by @joshtriplett on 2016-10-30 01:42_

Adding the square brackets seems like an improvement, even though the additional angle brackets make it still a bit confusing.

If it's "zero or more" rather than "zero or one", then it should have a `...` inside the square brackets, as well.


---

_Closed by @kbknapp on 2018-07-22 00:53_

---
