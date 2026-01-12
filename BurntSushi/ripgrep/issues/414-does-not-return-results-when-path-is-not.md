```yaml
number: 414
title: Does not return results when path is not specified on Bash on Ubuntu on Windows
type: issue
state: closed
author: wolf-dog
labels:
  - question
assignees: []
created_at: 2017-03-21T05:11:10Z
updated_at: 2018-02-02T14:06:12Z
url: https://github.com/BurntSushi/ripgrep/issues/414
synced_at: 2026-01-12T16:13:21Z
```

# Does not return results when path is not specified on Bash on Ubuntu on Windows

---

_@wolf-dog_

I encounted a problem that rg does not return results when path is not specified.

```sh
$ ls
foo.md
$ cat foo.md
FOO

$ rg -uuu --debug 'FOO' foo.md
DEBUG:grep::search: regex ast:
Literal {
    chars: [
        'F',
        'O',
        'O'
    ],
    casei: false
}
DEBUG:grep::literals: literal prefixes detected: Literals { lits: [Complete(FOO)], limit_size: 250, limit_class: 10 }
DEBUG:rg::args: will try to use memory maps
1:FOO

$ rg -uuu --debug 'FOO'
DEBUG:grep::search: regex ast:
Literal {
    chars: [
        'F',
        'O',
        'O'
    ],
    casei: false
}
DEBUG:grep::literals: literal prefixes detected: Literals { lits: [Complete(FOO)], limit_size: 250, limit_class: 10 }
```

works

- `rg 'FOO' foo.md`
- `rg 'FOO' *`
- `rg 'FOO' ./*`

doesn't work

- `rg 'FOO'`
- `rg 'FOO' ./`

rg ver 0.5.0(`ripgrep-0.5.0-x86_64-unknown-linux-musl.tar.gz`)

OS: Bash on Ubuntu on Windows(Ubuntu 14.04.5 LTS)
Shell: GNU bash 4.3.11(1)-release, fish version 2.5.0-168-g162053e

---

On normal(not BoUoW) Ubuntu, everything is fine.

rg ver 0.5.0(`ripgrep-0.5.0-x86_64-unknown-linux-musl.tar.gz`)

OS: Ubuntu 14.04.5 LTS
Shell: GNU bash 4.3.11(1)-release



---

_Comment by @BurntSushi on 2017-03-21 10:57_

Could you please specify what "doesn't work" means? Does ripgrep hang? Does it complete and just not print anything?

The fact that `./*` works but `./` doesn't is very strange.

I can't believe that in addition to handling native consoles and cygwin on Windows, we now also have to handle a third type of environment.

---

_Label `question` added by @BurntSushi on 2017-03-21 10:57_

---

_Comment by @wolf-dog on 2017-03-22 06:30_

"Doesn't work" means that ripgrep complete and not print anything.
It returns code 1 just like no result found.

> I can't believe that in addition to handling native consoles and cygwin on Windows, we now also have to handle a third type of environment.

Yeah, we believed that BoUoW will replace cygwin and bring us peace.
But the fact is...

---

_Comment by @bohrshaw on 2017-03-31 11:32_

I don't know if compiling from source works. But the binary downloaded from release page does not work on WSL.

    rg -uuu --debug t
    DEBUG:grep::search: regex ast:
    Literal {
        chars: [
            't'
        ],
        casei: false
    }
    DEBUG:grep::literals: literal prefixes detected: Literals { lits: [Complete(t)], limit_size: 250, limit_class: 10 }


---

_Comment by @BurntSushi on 2017-03-31 11:49_

@bohrshaw When you say "doesn't work" could you please be more specific? Does it terminate without printing any results? Or does it hang?

---

_Comment by @bohrshaw on 2017-03-31 12:51_

    rg .
    rg . .
    rg . *

Each immediately exits with exit code 1 and prints out nothing.

While `rg . README.md` works as expected.

`rg` version: 0.5.0

---

_Comment by @kbknapp on 2017-03-31 15:43_

I thought I ran into this yesterday as well on BoUoW. But not being a Windows guy, I shrugged it off. Upon seeing this issue I just tried to reproduce and can't seem to:

```
nix@DESKTOP:/tmp/rg-414$ rg -V
\ripgrep 0.5.0
nix@DESKTOP:/tmp/rg-414$ ls
scratch
nix@DESKTOP:/tmp/rg-414$ cat scratch
foo foo foo
foo foo foo
foo foo bar
foo foo bar
foo foo foo
foo foo foo
nix@DESKTOP:/tmp/rg-414$ rg bar
scratch
3:foo foo bar
4:foo foo bar
nix@DESKTOP:/tmp/rg-414$ rg bar ./
scratch
3:foo foo bar
4:foo foo bar
nix@DESKTOP:/tmp/rg-414$ rg .
scratch
1:foo foo foo
2:foo foo foo
3:foo foo bar
4:foo foo bar
5:foo foo foo
6:foo foo foo
nix@DESKTOP:/tmp/rg-414$ rg . *
1:foo foo foo
2:foo foo foo
3:foo foo bar
4:foo foo bar
5:foo foo foo
6:foo foo foo
```

Edit: in case it wasn't clear, this was using Bash on Ubuntu on Windows 10

---

_Comment by @BurntSushi on 2017-10-22 01:51_

Is this bug still reproducible? If it is, could someone try to provide more details on how to reproduce it? (Please consider that I am someone that doesn't even know what "Bash on Ubuntu on Windows 10" even is. :P)

---

_Comment by @bohrshaw on 2017-10-22 04:01_

It's not reproducible (for the my previous examples) with version 0.6.0.

---

_Comment by @BurntSushi on 2018-02-02 14:06_

Closing this as not reproducible. Happy to re-open if more data comes in.

---

_Closed by @BurntSushi on 2018-02-02 14:06_

---
