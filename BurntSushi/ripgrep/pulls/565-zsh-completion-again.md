```yaml
number: 565
title: zsh completion... again
type: pull_request
state: merged
author: okdana
labels: []
assignees: []
merged: true
base: master
head: bug/zsh-completion-again
created_at: 2017-07-26T02:33:50Z
updated_at: 2017-07-26T13:30:15Z
url: https://github.com/BurntSushi/ripgrep/pull/565
synced_at: 2026-01-12T18:23:13Z
```

# zsh completion... again

---

_@okdana_

Sorry for PR spam, found another issue with this. :/

I hadn't noticed that the old completion function didn't support completing values without a space after short options — doing something like `rg -C3 <tab>` instead of `rg -C 3 <tab>` would break the completion.

I fixed that. And, as discussed last time, i updated the test stuff so that it just pulls the optspecs straight out of the function. That should make the 'parsing' a lot more reliable (at the expense of requiring zsh for the test, obv).

Not sure if i put the zsh-installation stuff in the right place, guess we'll see what it says...

---

_Comment by @okdana on 2017-07-26 02:50_

Oh, i guess Travis doesn't actually use that arch. I looked for the `addons.apt.packages` file it mentioned but i don't see it. I don't know much about Travis unfortunately :/

Also i guess i should make it so that `script.sh` skips the test if zsh isn't installed? Rather than just exploding?

---

_Comment by @BurntSushi on 2017-07-26 10:42_

No worries about the PRs! This is kinda how I expect this sort of thing to go. Iterate and improve!

With respect to Travis, I think you can install zsh by putting a special incantation into the `.travis.yml`. Like this: https://docs.travis-ci.com/user/installing-dependencies/#Installing-Packages-on-Standard-Infrastructure

> Also i guess i should make it so that script.sh skips the test if zsh isn't installed? Rather than just exploding?

I think exploding is fine actually. That way, if zsh is somehow not installed, then we aren't tricked into believing the test is running and passing. If we want to test somewhere that zsh isn't feasible to install, then we should specifically ignore the test on that configuration.

---

_@BurntSushi reviewed on 2017-07-26 10:44_

---

_Review comment by @BurntSushi on `ci/install.sh`:50 on 2017-07-26 10:44_

I don't think I understand this. Why are you trying to install zsh only when testing on aarch64? aarch64 is ARM. I suspect ripgrep probably works on ARM, but I don't run CI on it.

---

_@okdana reviewed on 2017-07-26 13:02_

---

_Review comment by @okdana on `ci/install.sh`:50 on 2017-07-26 13:02_

Yeah, i kind of just copy-and-pasted the section above it without thinking too much about it. In hind-sight i see that it special-cased ARM because the C tool chain is arch-specific. But since you mentioned you didn't write these scripts i assume even that is probably just boiler-plate.

---

_@BurntSushi reviewed on 2017-07-26 13:09_

---

_Review comment by @BurntSushi on `ci/install.sh`:50 on 2017-07-26 13:09_

Ah, I [see now](https://github.com/BurntSushi/ripgrep/blob/master/ci/install.sh#L9). Yeah, all boiler plate. The original source is [rust-everywhere](https://github.com/japaric/rust-everywhere).

---

_Comment by @okdana on 2017-07-26 13:14_

I reverted the `install.sh` stuff and made it explode again if zsh is missing. I added the `zsh` package to `.travis.yml` per your link; that seems to sort it for Linux. Darwin doesn't need anything special because it ships with zsh. Looks like it worked!

---

_Comment by @okdana on 2017-07-26 13:16_

Oh, it doesn't like `pipe_fail` though. Not really necessary, will remove and rebase.

---

_Comment by @okdana on 2017-07-26 13:29_

Looks OK now i think

---

_Comment by @BurntSushi on 2017-07-26 13:30_

Awesome, looks great! Thanks so much!

---

_Merged by @BurntSushi on 2017-07-26 13:30_

---

_Closed by @BurntSushi on 2017-07-26 13:30_

---
