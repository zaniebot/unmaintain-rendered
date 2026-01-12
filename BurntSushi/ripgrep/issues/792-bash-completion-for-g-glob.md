```yaml
number: 792
title: bash completion for -g glob
type: issue
state: closed
author: jzinn
labels:
  - question
assignees: []
created_at: 2018-02-12T22:24:15Z
updated_at: 2018-07-22T16:24:32Z
url: https://github.com/BurntSushi/ripgrep/issues/792
synced_at: 2026-01-12T16:13:22Z
```

# bash completion for -g glob

---

_@jzinn_

#### What version of ripgrep are you using?

Ripgrep:
```
$ rg --version
ripgrep 0.8.0 (rev 23d1b91ead)
-SIMD -AVX
```

Bash:
```
bash-4.4.12-13.fc27.src.rpm
```

Bash-completion:
```
bash-completion-2.6-2.fc27.src.rpm
```

#### What operating system are you using ripgrep on?

```
$ uname -a
Linux ammonite 4.14.16-300.fc27.x86_64 #1 SMP Wed Jan 31 19:24:27 UTC 2018 x86_64 x86_64 x86_64 GNU/Linux
```

```
$ cat /etc/redhat-release 
Fedora release 27 (Twenty Seven)
```

#### Describe your question, feature request, or bug.

**Bug**: `rg -g Foo<tab>` should not be replaced with `rg -g <GLOB>...`.

When typing an argument to `-g` and hitting the tab character, then whatever you have typed for the argument is erased and replaced with the characters `<GLOB>...`.  It would be better not to do anything at all to preserve what was typed.

Even better than doing nothing would be to use file names for completion, so ...

**Feature request**: `rg -g Foo<tab>` should use files in the current directory for tab auto-completion in Bash.  If possible, it should also work with negation (`rg -g "!"Foo<tab>`).

For example, the following should work:

```
touch Foo.txt
rg -g Fo<tab>  # hit tab to initiate completion
rg -g Foo.txt  # after completion
```

This should also work:

```
mkdir vendor
rg -g "!"vend<tab>  # hit tab to initiate completion
rg -g "!"vendor     # after completion
```

#### If this is a bug, what are the steps to reproduce the behavior?

Make a file:
```
touch Foo.txt
```

Attempt to use it with autocomplete:
```
rg -g Fo<tab>
```

#### If this is a bug, what is the actual behavior?

The command line is changed to
```
rg -g <GLOB>...
```

#### If this is a bug, what is the expected behavior?

Either maintain the original command:
```
rg -g Fo
```

or use file name:
```
rg -g Foo.txt
```


---

_Comment by @BurntSushi on 2018-02-12 22:27_

This sounds like a clap bug. Completion rules for ripgrep are provided on a best effort basis. Right now, the bash completion rules are automatically generated. I don't personally have the bandwidth to do anything more than that. (We either provide what clap provides, or someone else maintains them (like @okdana has been doing for zsh), or we drop them completely.)

---

_Label `question` added by @BurntSushi on 2018-02-12 22:27_

---

_Comment by @BurntSushi on 2018-02-12 22:27_

Thank you for the awesome bug report though. :-)

---

_Comment by @jzinn on 2018-02-12 22:28_

Thank you for the incredible response time!

---

_Comment by @kbknapp on 2018-02-12 23:11_

@jzinn would you mind filing a bug at [kbknapp/clap-rs](https://github.com/kbknapp/clap-rs) please? In general the completion script generation was meant to get to the 95% solution, so they're not perfect. I agree it shouldn't replace anything already typed on the command line with a generic `<GLOB>...`, and actually should ideally just fallback to file matching.

It should be fairly simple on my end to just remove all generic completions and simply use file completion which I would say is what people expect 99% of the time. Even when it's actually wrong and not what the flag is asking for, it still doesn't surprise people like totally erasing what they wrote and replacing with a generic statement.

---

_Comment by @jzinn on 2018-02-13 00:12_

Certainly.

---

_Comment by @BurntSushi on 2018-07-22 16:24_

This appears to have been fixed in clap.

---

_Closed by @BurntSushi on 2018-07-22 16:24_

---
