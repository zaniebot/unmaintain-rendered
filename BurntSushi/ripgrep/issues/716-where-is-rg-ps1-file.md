```yaml
number: 716
title: Where is _rg.ps1 file?
type: issue
state: closed
author: vors
labels: []
assignees: []
created_at: 2017-12-17T17:44:48Z
updated_at: 2017-12-17T19:34:24Z
url: https://github.com/BurntSushi/ripgrep/issues/716
synced_at: 2026-01-12T16:13:22Z
```

# Where is _rg.ps1 file?

---

_@vors_

Hello!

The powershell completion instructions been added in
https://github.com/BurntSushi/ripgrep/commit/d2c7a76a3c0f6c264ec0b1f04e1dcaeba3862a51


>For PowerShell, add . _rg.ps1 to your PowerShell profile (note the leading period). If the _rg.ps1 file is not on your PATH, do . /path/to/_rg.ps1 instead.

I could not find the `_rg.ps1` file in the final package or source code...
Is it in another repo?
cc @elirnm

---

_Comment by @okdana on 2017-12-17 18:23_

It's auto-generated during the build by [clap](https://github.com/kbknapp/clap-rs/)

---

_Comment by @vors on 2017-12-17 19:15_

Cool, so where where it is in the package? I'm trying to find it on mac (powershell is cross-platform) ripgrep v0.7.1, installed thru `brew`. Is it included only in the windows build?

---

_Comment by @okdana on 2017-12-17 19:29_

AFAIK it's included in all of the [official release archives](https://github.com/BurntSushi/ripgrep/releases)

The [Homebrew formula](https://github.com/Homebrew/homebrew-core/blob/master/Formula/ripgrep.rb) just ignores it though

The formula code has standardised install locations for bash/fish/zsh completions; i don't think PowerShell is a core formula (?), but if there's a similar location for it that would play nicely with Homebrew you might consider [submitting a request](https://github.com/Homebrew/brew/issues) to have them add it

---

_Comment by @vors on 2017-12-17 19:30_

On a side note (I don't know much about rust ecosystem), wow - the clap project looks awesome!

---

_Comment by @vors on 2017-12-17 19:34_

@okdana thank you for the quick reply!

---

_Closed by @vors on 2017-12-17 19:34_

---
