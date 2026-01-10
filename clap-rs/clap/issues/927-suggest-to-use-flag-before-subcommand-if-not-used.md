---
number: 927
title: Suggest to use flag before subcommand if not used correctly
type: issue
state: closed
author: torkleyy
labels:
  - C-enhancement
  - A-parsing
  - E-medium
assignees: []
created_at: 2017-04-10T11:34:09Z
updated_at: 2018-08-02T03:30:05Z
url: https://github.com/clap-rs/clap/issues/927
synced_at: 2026-01-10T01:26:38Z
---

# Suggest to use flag before subcommand if not used correctly

---

_Issue opened by @torkleyy on 2017-04-10 11:34_

Suggesting to use the flag *before* the subcommand would make the application a bit more user-friendly.

### Sample Code

```rust
App::new("myapp")
    .arg(Arg::with_name("foo")
        .long("foo"))
    .subcommand(SubCommand::with_name("bar"));
```

```bash
myapp bar --foo
```

### Actual Behavior

```
error: Found argument '--foo' which wasn't expected, or isn't valid in this context

USAGE:
    myapp bar

For more information try --help
```

### Expected Behavior

```
error: Found argument '--foo' which wasn't expected, or isn't valid in this context
      Did you mean to put '--foo' before the subcommand?

        myapp --foo bar
              ^^^^^

USAGE:
    myapp bar

For more information try --help
```

---

_Comment by @kbknapp on 2017-04-18 00:34_

Apologies for the late reply, I was travelling the past week. 

This would be a cool feature! I'd be willing to mentor anyone who wants to take a swing at this, although I'd change the wording slightly:

```
error: Found argument '--foo' which wasn't expected, or isn't valid in this context
      Did you mean to put '--foo' after the subcommand?

        myapp bar --foo
                  ^^^^^
USAGE:
    myapp bar

For more information try --help
```

The key code to add would be:

* Adding a variant [here, to `DidYouMeanMessageStyle`](https://github.com/kbknapp/clap-rs/blob/3f44dd4b91d15ec9287eb236e2d3a2fd068afad1/src/suggestions.rs#L70-L77)
* Handling the new variant [here](https://github.com/kbknapp/clap-rs/blob/3f44dd4b91d15ec9287eb236e2d3a2fd068afad1/src/suggestions.rs#L53-L58)
* One would *probably* be best off simply adding a new `did_you_mean_child_args` type function, which could use [`did_you_mean`](https://github.com/kbknapp/clap-rs/blob/3f44dd4b91d15ec9287eb236e2d3a2fd068afad1/src/suggestions.rs#L14-L31) as a template of sorts.
* And finally calling this new function [inside here](https://github.com/kbknapp/clap-rs/blob/3f44dd4b91d15ec9287eb236e2d3a2fd068afad1/src/app/parser.rs#L1585-L1610), but using some logic like 
  * if no subcommands, don't check for child args [obviously]
  * re-scan `env::args_os` for possible subcommands using `find_subcmd!(self, arg_os)` which correctly checks aliases.
  * You can get all the long version of flags and options with by [`Chain`](https://doc.rust-lang.org/core/iter/trait.Iterator.html#method.chain)ing the longs in `Parser::flags` and `Parser::opts` as in `self.flags.iter().filter_map(|f| f.s.long).chain(self.opts.iter().filter_map(|o| o.s.long))` which could probably be passed to your `did_you_mean_child_args`
 *  Consider invocation `$ prog foo --bar baz` where `--bar` is actually a child of `baz`. We'd need to ensure that when re-scanning `env::args_os` if an arg returns that it *is* a valid subcommand, but **does not** contain `--bar`, we continue searching. This will prevent from accidentally checking `foo` for `--bar` which we already know doesn't exist or we wouldn't be here. We then only return the error if after exhausting the entire list we still haven't found anything. I'd also like to see how this interacts with large argvs...such as shell glob expansions that could be in the thousands or tens of thousands (although I think this would be rare case).

---

_Label `C: errors` added by @kbknapp on 2017-04-18 00:37_

---

_Label `C: parsing` added by @kbknapp on 2017-04-18 00:37_

---

_Label `D: intermediate` added by @kbknapp on 2017-04-18 00:37_

---

_Label `M: mentored` added by @kbknapp on 2017-04-18 00:37_

---

_Label `P4: nice to have` added by @kbknapp on 2017-04-18 00:37_

---

_Label `T: enhancement` added by @kbknapp on 2017-04-18 00:37_

---

_Label `W: 2.x` added by @kbknapp on 2017-04-18 00:37_

---

_Referenced in [clap-rs/clap#980](../../clap-rs/clap/pulls/980.md) on 2017-06-08 02:30_

---

_Closed by @kbknapp on 2017-06-16 14:28_

---
