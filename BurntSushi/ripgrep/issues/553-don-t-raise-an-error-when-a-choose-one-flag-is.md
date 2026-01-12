```yaml
number: 553
title: "Don't raise an error when a choose-one flag is specified repeatedly"
type: issue
state: closed
author: sarahgerweck
labels:
  - enhancement
  - question
assignees: []
created_at: 2017-07-13T01:16:06Z
updated_at: 2018-02-06T00:51:25Z
url: https://github.com/BurntSushi/ripgrep/issues/553
synced_at: 2026-01-12T16:13:22Z
```

# Don't raise an error when a choose-one flag is specified repeatedly

---

_@sarahgerweck_

There is currently no way to set custom defaults for Ripgrep. The recommended practice is to set an alias instead. However, this is somewhat thwarted by the fact that Ripgrep is too aggressive in validating that you never repeat yourself. This also has other consequences.

E.g., if you prefer smart case matching, you can make an alias like this:

```
alias rg='rg -S'
```

This _mostly_ works, but it means that I now get an error if I ever execute any commands that include `-S` in them. Ripgrep give the error below:
```
error: The argument '--smart-case' was provided more than once, but cannot be used multiple times
```

This is undesirable behavior both when writing scripts and when trying to use aliases as defaults. Both scripts and aliases face the same problem: building up a complex command line by nesting contexts. This rule means that this process needs to be globally aware of all the outer contexts.

I think it is much more common and more user friendly that commands like this use the last one specified for flags that are mutually exclusive, and don't raise errors if you specify the same flag twice. Otherwise, trying to build nontrivial command lines from variables or aliases is somewhat a nightmare.

Fortunately, it at least allows `rg -S -i`. I think this is a good example of why raising an error for `rg -S -S` is undesired behavior. The `rg -S -i` case is *more* overspecified than the `rg -S -S` case. Ultimately, the current rule doesn't actually protect you from any undesired behavior, but it does prevent useful use cases.

---

_Comment by @BurntSushi on 2017-07-13 01:34_

Right, agreed. My feeling is that this should get lumped into #196 (where I also left a [comment explaining some things just now](https://github.com/BurntSushi/ripgrep/issues/196#issuecomment-314943495)), although I can appreciate that these are really two orthogonal issues. It just so happens that persistent config is blocked on it. But we can make your situation better without needing to finish persistent config, and this issue is thorny enough that I think giving it its own ticket is a good idea.

As a possible work-around until this is fixed properly, if you prefix your command with `\` (e.g., `\rg -S pattern`), then your shell should ignore any aliases you have set up and invoke the command directly.

> This is undesirable behavior both when writing scripts and when trying to use aliases as defaults. Both scripts and aliases face the same problem: building up a complex command line by nesting contexts. This rule means that this process needs to be globally aware of all the outer contexts.

I think you might find that you have this problem regardless, although I can agree that the particular version of the problem you have might be the most annoying version of it. In particular, if you write a script that assumes your default config, then it might fail when the default config isn't present (on a different machine) or if it changes over times as your preferences evolve. My "answer" to that is to avoid using custom defaults inside scripts.

---

_Label `enhancement` added by @BurntSushi on 2017-07-13 01:34_

---

_Label `question` added by @BurntSushi on 2017-07-13 01:34_

---

_Comment by @sarahgerweck on 2017-07-13 01:38_

Yeah, I agree that custom settings can create problems with scripts & shared commands in general. The point I was making was that if I wanted to send a coworker a command I use, I could just add the `-S` that is part of my alias—but then I'll get complaints if I run it. You're right that I could work around this by temporarily unaliasing it or escaping it: it's just a matter of convenience.

Neither this nor #196 is an earth-shattering issue. Ripgrep is a great tool and a nice performance and `.gitignore`-compliance improvement over Silver Searcher: this is just a quality-of-life issue.

---

_Comment by @okdana on 2017-07-13 02:22_

btw, i believe [clap #976](https://github.com/kbknapp/clap-rs/issues/976) is the specific ticket that covers this limitation it has. I don't think i like the suggested solution (it should be the default behaviour, you shouldn't have to write a bunch of code for every single option) but that's where the immediate fix would come from i think.

tbh, these sorts of problems are common to almost every library that tries to provide a more 'modern' approach to argument-handling than `getopt(3)`/`getopt_long(3)`. Python's `argparse` (and `optparse` before it), Swift's Commander, PHP's Symfony Console and Zend Console, &c. — they all have really frustrating limitations when it comes to dealing with options that appear multiple times or whose position in the argument list is supposed to be meaningful.

`clap` is actually really functionally advanced compared to all of those, but you still run up against some fundamental issues that would be very simple to deal with if the library would just give you back the list of arguments so you could handle them yourself with a `for` loop.

Given all of what `clap` already does, though, this *particular* issue doesn't seem like it'd be too difficult to solve. Probably just a matter of time and priorities.

---

_Closed by @BurntSushi on 2018-02-04 15:40_

---

_Comment by @kbknapp on 2018-02-05 03:53_

kbknapp/clap-rs#976 has finally been fixed, implemented and released (v2.29.3). I know there is a work around for this, but just FYI.

---

_Comment by @BurntSushi on 2018-02-05 12:28_

@kbknapp Awesome! Thanks so much! I will switch over to that and get rid of my work-around. :-)

---

_Comment by @BurntSushi on 2018-02-06 00:21_

@kbknapp Hmm it looks like I can't just use `AllArgsOverrideSelf` because it causes flags that are supposed to be able to be specified multiple times to not work correctly. For example, if I enabled `AppSettings::AllArgsOverrideSelf`, and if I run

```
$ rg 'fn run' src/ --colors 'match:fg:blue' --colors 'path:fg:blue'
```

then only `--colors 'path:fg:blue'` will be applied and `--colors 'match:fg:blue'` will be ignored.

---

_Comment by @kbknapp on 2018-02-06 00:27_

🤦‍♂️ I fixed that for the switch case and forgot to fix it for the option case....standby

---

_Comment by @kbknapp on 2018-02-06 00:51_

Fixed in kbknapp/clap-rs#1166 



---
