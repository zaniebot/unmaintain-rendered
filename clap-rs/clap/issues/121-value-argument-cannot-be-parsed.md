```yaml
number: 121
title: "'-' value argument cannot be parsed"
type: issue
state: closed
author: Byron
labels:
  - C-bug
assignees: []
created_at: 2015-05-17T12:36:43Z
updated_at: 2015-05-17T14:41:24Z
url: https://github.com/clap-rs/clap/issues/121
synced_at: 2026-01-12T16:14:08Z
```

# '-' value argument cannot be parsed

---

_@Byron_

In an argument like the following: 

``` Rust
Arg::with_name("foo")
                               .short("f")
                               .required(true)
                               .takes_value(true)
```

It seems impossible to provide a value that is or starts with a `-`. The latter is common to specify that input should be read from stdin, instead of from a file.

The actual output in my current (toy) program is:

```
$ trains -n -
The argument '--network <network>' requires a value but none was supplied

USAGE:
    trains --network <network>

For more information try --help
```


---

_Comment by @kbknapp on 2015-05-17 13:13_

Ah ok, right now `--` is special cased but `-` isn't, I just need to add it. I'm on mobile right now but its a trivial fix so I'll have it done tonight. Thanks!


---

_Label `bug` added by @kbknapp on 2015-05-17 13:14_

---

_Comment by @Byron on 2015-05-17 13:48_

Great to hear ! It would be great if you could publish that patch as well, so people building the tool tomorrow/early next week will have it. Thanks in advance.

The question i'd have is why '-' would need to be a special case. What I would expect is that if the argument takes any bounded amount of arguments, one would just pick them off the argument sequence without interpreting them at all. The only difficulty, as always, occurs if there are one or more arguments, which is when some special handling would be in order to prevent parsing subcommands as arguments for instance. It's just what came to my mind, I have no clue after all.


---

_Closed by @kbknapp on 2015-05-17 13:51_

---

_Comment by @kbknapp on 2015-05-17 14:01_

bc12e78 on master or v0.8.6 on crates.io fixes this ;)

The reason it's special cased is the way it decides when to stop parsing multiple arguments, so if you had a `-f val val val -r other` it stops parsing `-f` once it reaches a value starting with `-`, the problem was `-` alone starts with `-` :P But this is fixed now.

There are other ways to solve this, which I may look at in the future. The biggest issue is you can't just store the whole "flag" as the identifier, such as `-f` instead of simply `f` because short arguments can be combined such as `-rfbZ` is the same as `-r -f -b -Z`. I may look at this in the future, but it'd be a substantial change for an unknown gain - I'd have to test :)


---

_Assigned to @kbknapp by @kbknapp on 2015-05-17 14:02_

---

_Comment by @kbknapp on 2015-05-17 14:24_

As a side note, this issue also helped something I'd totally forgotten about. `-` is used in some Unix/Linux utilities to denote "stdin" so not being able to parse `-` would have been a deal breaker ;) so, thanks again!


---

_Comment by @Byron on 2015-05-17 14:41_

Yes, I see, there is more about argument parsing :) ! Totally forgot about the short flag concatenation.

Besides: You are welcome, it's always a pleasure to interact with this project !


---
