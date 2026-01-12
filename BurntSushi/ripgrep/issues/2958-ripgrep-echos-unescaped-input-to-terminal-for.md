```yaml
number: 2958
title: ripgrep echos unescaped input to terminal, for filenames and regexes
type: issue
state: open
author: andychu
labels:
  - question
assignees: []
created_at: 2025-01-02T05:53:58Z
updated_at: 2025-01-02T16:08:14Z
url: https://github.com/BurntSushi/ripgrep/issues/2958
synced_at: 2026-01-12T16:13:25Z
```

# ripgrep echos unescaped input to terminal, for filenames and regexes

---

_@andychu_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

13.0.0

### How did you install ripgrep?

apt-get

### What operating system are you using ripgrep on?

Debian

### Describe your bug.

ripgrep echos unescaped input to terminal, for filenames and regexes

### What are the steps to reproduce the behavior?

red=$'\x1b[31m RED \x1b[0;0m'

rg pat "$red"    # filename

rg "$red'   # pattern

### What is the actual behavior?

I see red  text

### What is the expected behavior?

It should probably be escaped, at least in the filename case

I noticed this when testing a bunch of programs, as mentioned here

https://lobste.rs/s/qwcov5/deja_vu_ghostly_cves_my_terminal_title#c_js5pfq

There are two categories of programs:

- bash, perl, python2, lua, node JS, grep, ripgrep

vs.

- ls and cat (coreutils), python3, ruby, OSH/YSH

This is possibly debatable, but I think the latter behavior is better



---

_Comment by @BurntSushi on 2025-01-02 12:37_

I imagine it will do the same for file data too (which has been reported before), which ripgrep just dumps to stdout as is. I'm not really sure this is worth fixing.

---

_Label `question` added by @BurntSushi on 2025-01-02 12:37_

---

_Comment by @andychu on 2025-01-02 16:08_

Yeah I am not saying it's a security issue -- all this stuff with terminals is  kinda ambiguous

Just saying that programs do disagree on this, but the trend is toward the latter behavior.  At least with coreutils in 2016 - https://www.gnu.org/software/coreutils/quotes.html

I think it's more usable to see the quoted representation of what you typed, rather than  having invisible chars, or red chars

i.e. if there is a garbage filename on the file system, it could be more noticeable to the user in quoted form

(I never thought about the actual file contents on stdout, though come to think of it having a mode  to show that is something I might use, although it could also be a filter after grep/rg)





---
