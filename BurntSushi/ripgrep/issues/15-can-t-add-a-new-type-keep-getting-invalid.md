```yaml
number: 15
title: "Can't add a new type, keep getting ''Invalid arguments\"."
type: issue
state: closed
author: smaximov
labels:
  - bug
assignees: []
created_at: 2016-09-23T15:40:48Z
updated_at: 2016-09-24T22:56:06Z
url: https://github.com/BurntSushi/ripgrep/issues/15
synced_at: 2026-01-12T18:23:11Z
```

# Can't add a new type, keep getting ''Invalid arguments".

---

_@smaximov_

Trying to add a new type results in the "Invalid arguments." error followed by the usage message.

I have tried the example from README (`rg --type-add 'foo:*.foo,*.foobar'`) and some other patterns.

My environment:
- Fedora 24 (x86_64).
- Rust (tried both):
  - rustc 1.13.0-nightly (4f9812a59 2016-09-21)
  - rustc 1.11.0 (9b21dcd6a 2016-08-15)
- ripgrep: 0.1.16


---

_Comment by @BurntSushi on 2016-09-23 15:45_

Could you show precisely the command you're running and the full output?

I wonder if I messed up the docs. It looks like comma separated globs aren't supported, but they probably should be.

Using `--type-add` for each glob should do as a workaround for now. (You the globs are additive, so you can have multiple `--type-add` flags for the same type.)


---

_Label `bug` added by @BurntSushi on 2016-09-23 15:45_

---

_Comment by @smaximov on 2016-09-23 15:52_

I have just built ripgrep using the stable rust (rg was bumped to 0.1.17) with no effect.

> Could you show precisely the command you're running and the full output?

Sure:

```
$ rustup show                       
Default host: x86_64-unknown-linux-gnu

installed toolchains
--------------------

stable-x86_64-unknown-linux-gnu (default)
nightly-x86_64-unknown-linux-gnu

active toolchain
----------------

stable-x86_64-unknown-linux-gnu (default)
rustc 1.11.0 (9b21dcd6a 2016-08-15)
$ rg --version
0.1.17
$ rg --type-add 'foo:*.foo,*.foobar'
Invalid arguments.

Usage: rg [options] -e PATTERN ... [<path> ...]
       rg [options] <pattern> [<path> ...]
       rg [options] --files [<path> ...]
       rg [options] --type-list
       rg --help
       rg --version
$ rg --type-add 'foo:*.foo'         
Invalid arguments.

Usage: rg [options] -e PATTERN ... [<path> ...]
       rg [options] <pattern> [<path> ...]
       rg [options] --files [<path> ...]
       rg [options] --type-list
       rg --help
       rg --version
```

I use ZSH, but I've also tried bash to make sure it's not a shell issue.


---

_Comment by @smaximov on 2016-09-23 16:05_

> Using `--type-add` for each glob should do as a workaround for now.

Does adding a single glob at a time work for you? If it does, I'll try to debug this issue tomorrow with my setup. 


---

_Comment by @BurntSushi on 2016-09-23 16:41_

Running `rg --type-add 'foo:*.foo'` is indeed invalid. You need to actually provide a pattern to search. :-)


---

_Comment by @hodavidhara on 2016-09-23 17:21_

I haven't been able to add a type either. Tried a bunch of stuff:

```
$ rg --type-add 'ts:*.ts,*.tsx'
Invalid arguments.
$ rg --type-add 'ts:*.ts'
Invalid arguments.
$ rg --type-add ts *.ts
invalid definition (format is type:glob, e.g., html:*.html)
$ rg --type-add ts:*.ts
Invalid arguments.
```

I'm on os X el capitan (10.11.6) and Installed it with the homebrew command in the readme.


---

_Comment by @BurntSushi on 2016-09-23 17:29_

You need to provide a pattern to search. Ripgrep doesn't save any state or config anywhere. --type-add only applies to the current command.


---

_Comment by @hodavidhara on 2016-09-23 17:44_

Ahh, I guess I misunderstood from the documentation. I thought it would add it to the list of types returned by `rg --type-list` to be used with -t and -T in the future. So if I understand correctly `rg --type-add 'ts:*.ts' -tts foo` would be the same as `rg foo -g *.ts`?


---

_Comment by @BurntSushi on 2016-09-23 17:45_

@hodavidhara That sounds right yes! I'll work on improving the docs. :-)


---

_Comment by @smaximov on 2016-09-23 23:04_

> Ripgrep doesn't save any state or config anywhere. --type-add only applies to the current command.

I indeed expected `--type-add` to persist the new type to some sort of config , not to be just a modifier for the search (though it would be more useful that way, IMHO). Thanks for the clarification!


---

_Comment by @BurntSushi on 2016-09-24 02:31_

My kinda-sorta intention is to see how far we can get with configuring "default" options just by using aliases. So for example, `alias rg="--type-add 'foo:*.foo'"`. I agree that this can get a bit unwieldy if you have a lot of custom types, but I also kind of expect to be pretty lenient with putting types into `rg` itself (ya know, good defaults and all).


---

_Comment by @smaximov on 2016-09-24 02:52_

> My kinda-sorta intention is to see how far we can get with configuring "default" options just by using aliases. So for example, `alias rg="--type-add 'foo:*.foo'"`

Yes, that use-case makes sense, I haven't thought about it before.


---

_Comment by @cldwalker on 2016-09-24 14:25_

I also was confused and assumed custom types persisted. Seeing --type-add, --type-clear and --type-list, I assumed add persisted since there was a clear operation


---

_Closed by @BurntSushi on 2016-09-24 22:56_

---
