```yaml
number: 112
title: "Error: SHA256 mismatch when installing via Homebrew"
type: issue
state: closed
author: briantully
labels: []
assignees: []
created_at: 2016-09-27T05:31:27Z
updated_at: 2016-09-27T11:11:49Z
url: https://github.com/BurntSushi/ripgrep/issues/112
synced_at: 2026-01-12T18:23:11Z
```

# Error: SHA256 mismatch when installing via Homebrew

---

_@briantully_

It seems the brew formula has an incorrect sha256 specified for the "if Hardware::CPU.is_64_bit?" for version 0.2.1. It seems like the sha256 is still referencing 0.2.0?

`######################################################################## 100.0%
==> Downloading https://github.com/BurntSushi/ripgrep/releases/download/0.2.1/ripgrep-0.2.1-x86_64-apple-darwin.tar.gz
==> Downloading from https://github-cloud.s3.amazonaws.com/releases/53631945/0a168e3c-8426-11e6-91b8-757f7d926f39.gz?X-A
###### ################################################################## 100.0%

Error: SHA256 mismatch
Expected: f55ef5dac04178bcae0d6c5ba2d09690d326e8c7c3f28e561025b04e1ab81d80
Actual: f8b208239b988708da2e58f848a75bf70ad144e201b3ed99cd323cc5a699625f`


---

_Comment by @hackling on 2016-09-27 05:56_

Can confirm, having the same issue as well.


---

_Comment by @hackling on 2016-09-27 06:15_

Doesn't fix the issue, but lets you check out the application right before the update:

```
brew install https://raw.githubusercontent.com/BurntSushi/ripgrep/2b15832655539ce5550244621680fd1fe9cd3795/pkg/brew/ripgrep.rb
```


---

_Comment by @BurntSushi on 2016-09-27 11:11_

#115 fixed this. Sorry about the hiccup!


---

_Closed by @BurntSushi on 2016-09-27 11:11_

---
