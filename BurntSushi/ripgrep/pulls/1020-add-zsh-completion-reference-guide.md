```yaml
number: 1020
title: Add zsh-completion reference guide
type: pull_request
state: merged
author: okdana
labels: []
assignees: []
merged: true
base: master
head: dana/completion-reference
created_at: 2018-08-20T14:04:56Z
updated_at: 2018-08-20T19:25:54Z
url: https://github.com/BurntSushi/ripgrep/pull/1020
synced_at: 2026-01-12T18:23:13Z
```

# Add zsh-completion reference guide

---

_@okdana_

As discussed in #1017 again — this adds a brief\* explanation of `_arguments` specifications to the zsh completion function, as a reference for other developers to use when making minor changes to the function. I mainly focussed on option specs, since that's the most common thing to change.

\* Not actually that brief, lol. Is it too wordy for a 'reference'? If you have suggestions as to how i could make it more 'skimmable' i'm definitely open to them. Or obv if you have any unanswered questions i can address them

ETA: Fixed very minor typo

---

_Review comment by @BurntSushi on `complete/_rg`:399 on 2018-08-20 15:42_

Oh my. This one paragraph just clarified so many things.

---

_@BurntSushi approved on 2018-08-20 15:52_

omg yes yes yes! Thank you. This clarifies so much for me. This should hopefully stave off my desire to have this file be auto generated. :-) For now, anyway. :P 

Out of curiosity, how similar is setting up auto-completions in bash to zsh? I don't think I realized how high quality these completions were. Now I want them in bash. :-)

---

_Merged by @BurntSushi on 2018-08-20 15:53_

---

_Closed by @BurntSushi on 2018-08-20 15:53_

---

_Comment by @okdana on 2018-08-20 16:42_

I'm not very knowledgeable about bash's completion system, but i know that it's functionally simpler than zsh's, and there's less abstraction built around it. It doesn't provide any of the fancy menu stuff that zsh has, and it's not especially configurable (mostly because there's not a lot to configure). I think it's possible to do some fancy things with it, but again since there's not much abstraction built around it you often have to do it manually (and so nobody bothers).

I think the `bash-completion` project provides some helpers to do certain things like handling long options with equals (`--color <TAB>` vs `--color=<TAB>`), but i haven't looked too much into that either.

zsh isn't super difficult to switch to, btw, if you ever feel compelled to try it. The default configuration is terrible, but if you set it up right it can be made to work almost like a super-set of bash. I was able to switch the default interactive shell in our product at work over to zsh without any of our tech-support people really even noticing the difference (besides the improved completion).

---

_Comment by @BurntSushi on 2018-08-20 19:25_

Drats. I was afraid you'd say that. I'm not a bash power user by any means, so I'm sure I could switch to zsh, but I fear the yak shave. Some day, perhaps. :-)

---
