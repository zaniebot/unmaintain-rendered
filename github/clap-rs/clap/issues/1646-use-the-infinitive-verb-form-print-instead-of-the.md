---
number: 1646
title: Use the infinitive verb form (‘print’) instead of the third singular (‘prints’) in help messages
type: issue
state: closed
author: glts
labels: []
assignees: []
created_at: 2020-01-29T12:04:43Z
updated_at: 2020-02-01T19:44:11Z
url: https://github.com/clap-rs/clap/issues/1646
synced_at: 2026-01-07T13:12:19-06:00
---

# Use the infinitive verb form (‘print’) instead of the third singular (‘prints’) in help messages

---

_Issue opened by @glts on 2020-01-29 12:04_

I would like to propose switching the verb form used in the examples, but especially in the default help messages, to using the infinitive or imperative form. For example, ‘prints’ would become ‘print’.

The verb used in the default help messages such as ‘Prints version information’ clashes with the form I use in my help messages, for example ‘Suppress header rewriting’. Looking at a couple of CLI’s I’ve used most recently on my machine (cargo, rustup, git, rsync, grep, vim, emacs), I’m beginning to believe that the infinitive form, without the third person singular suffix *-s*, is preferable.

I do myself use the finite form when writing API documentation for functions (‘Prints version information’, read: ‘[this method] prints version information’), but I think options and flags are different. Options and flags change how a program does something or what a program does, they don’t themselves ‘do’ something like a function does.

Anyway, I don’t want to make a linguistic argument here, instead I want to argue for good defaults, and in this case, looking at existing CLI’s, in my opinion the infinitive verb form would be a better default.

Thank you for your consideration!

---

_Comment by @hadronized on 2020-01-29 12:37_

I think the proper enhancement would be to authorize to override the whole help message?

---

_Comment by @glts on 2020-01-29 15:07_

@phaazon My main motive here is good defaults. I like using tools with little customisation if possible, and since all CLI’s I’ve looked at use the infinitive form, I’ve opened this issue for consideration. Cheers.

---

_Comment by @hadronized on 2020-01-30 09:02_

I’m not sure there _is_ a good default. Consistency might also be biased towards the tools you use. I’m not saying you’re wrong. Just that I think _good defaults_ are never a thing, and it’s better to provide a way to customize them.

Also, you can still provide your custom help string by hand, so I’m not sure what the others are going to do with your issue. 😕 

---

_Comment by @CreepySkeleton on 2020-02-01 13:17_

@glts You can override this messages with `help_message` and `version_message` respectively. I think this resolves the issue, feel free to reopen.

---

_Closed by @CreepySkeleton on 2020-02-01 13:17_

---

_Label `T: RFC / question` added by @pksunkara on 2020-02-01 19:44_

---

_Label `W: wont do` added by @pksunkara on 2020-02-01 19:44_

---

_Referenced in [Rudo2204/rtend#4](../../Rudo2204/rtend/pulls/4.md) on 2020-08-11 11:06_

---

_Referenced in [clap-rs/clap#2500](../../clap-rs/clap/issues/2500.md) on 2021-05-26 19:25_

---
