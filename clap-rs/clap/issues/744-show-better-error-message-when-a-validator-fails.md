```yaml
number: 744
title: show better error message when a validator fails
type: issue
state: closed
author: BurntSushi
labels:
  - C-enhancement
  - A-validators
assignees: []
created_at: 2016-11-13T01:33:53Z
updated_at: 2018-08-02T03:29:56Z
url: https://github.com/clap-rs/clap/issues/744
synced_at: 2026-01-12T16:14:09Z
```

# show better error message when a validator fails

---

_@BurntSushi_

Let's say a valid command line invocation is this:

```
$ rg -j10 foo
```

but a user types this instead:

```
$ rg -j10a foo
```

I have a validator function on the `j` flag that checks whether it's a valid number or not. If not, it returns an error so that the output looks like this:

```
error: invalid digit found in string
```

This is fine as far as it goes, but let's take a look at the output of an error when I try to use a value that clap knows is wrong for a different flag:

```
$ rg --color wat foo
error: 'wat' isn't a valid value for '--color <WHEN>'
        [values: always, auto, never]


USAGE:
    
    rg [OPTIONS] <pattern> [<path> ...]
    rg [OPTIONS] -e PATTERN ... [<path> ...]
    rg [OPTIONS] --files [<path> ...]
    rg [OPTIONS] --type-list

For more information try --help
```

This error message is a lot better. In particular, the really critical piece of information is the *name of the flag* that's causing the problem. For example, consider a more complex command line:

```
$ rg -C3 -j10a -maxdepth 3 -m1 foo
```

There are a lot more numbers here, so it's harder to match up the error message with the actual CLI usage.

I think clap knows the flag name when a validator function fails. Could it include that in the error message?

(This is easily my favorite benefit of clap btw. Its failure modes are oh so much better than Docopt. Of course, I knew that before, but still, it's nice. :-))

---

_Comment by @kbknapp on 2016-11-14 21:44_

Sorry for the wait, it's been a busy few days in my offline life! :smile:

The unsophisticated work around is to use a different validator per argument, at which point the error you return could include the name of argument. I know that's not optimal, just saying for completeness sake :wink:

I could easily add a prefix to the error such as, `error: argument '-j <val>' failed validation with message: blah` Or something to that effect. Thoughts?


---

_Label `D: easy` added by @kbknapp on 2016-11-14 21:44_

---

_Label `P4: nice to have` added by @kbknapp on 2016-11-14 21:44_

---

_Label `T: enhancement` added by @kbknapp on 2016-11-14 21:44_

---

_Label `W: 2.x` added by @kbknapp on 2016-11-14 21:44_

---

_Label `C: errors` added by @kbknapp on 2016-11-14 21:45_

---

_Label `C: validators` added by @kbknapp on 2016-11-14 21:45_

---

_Comment by @BurntSushi on 2016-11-14 22:25_

That sounds great! I might even shorten it to `error: invalid argument '-j <val>': blah` but either sounds good to me!


---

_Closed by @homu on 2016-11-21 04:52_

---
