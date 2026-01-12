```yaml
number: 1545
title: "pkg: fix brew tap version to 12.0.1 to match sha"
type: pull_request
state: merged
author: simrobin
labels: []
assignees: []
merged: true
base: master
head: fix_brew_tap_12_0_1
created_at: 2020-04-07T21:21:57Z
updated_at: 2020-04-08T07:30:07Z
url: https://github.com/BurntSushi/ripgrep/pull/1545
synced_at: 2026-01-12T18:23:14Z
```

# pkg: fix brew tap version to 12.0.1 to match sha

---

_@simrobin_

Hello!

This will fix this currently happening error :
```
> brew install ripgrep-bin

==> Installing ripgrep-bin from burntsushi/ripgrep
==> Downloading https://github.com/BurntSushi/ripgrep/releases/download/12.0.0/ripgrep-12.0.0-x86_64-apple-darwin.tar.gz
Already downloaded: /Library/Caches/Homebrew/downloads/71ad3a51a074950abeaa875fb35ac00adca54d0852b0e0f4ed627056994fbecc--ripgrep-12.0.0-x86_64-apple-darwin.tar.gz
Error: An exception occurred within a child process:
  ChecksumMismatchError: SHA256 mismatch
Expected: 59e78931a7577e76f40c937cae2782f2bbdcb40f056df5dd84247a671373525d
  Actual: ebbc518ac019fc78b6429ec2e64b823c59b15a6f942436f43b9afc23c9aad9ea
 Archive: /Library/Caches/Homebrew/downloads/71ad3a51a074950abeaa875fb35ac00adca54d0852b0e0f4ed627056994fbecc--ripgrep-12.0.0-x86_64-apple-darwin.tar.gz
To retry an incomplete download, remove the file above.
```

---

_@BurntSushi approved on 2020-04-07 23:45_

Ah whoops, thanks!

Note that I don't think there is much benefit to using the Homebrew tap these days. Is there any particular reason why you're using it instead of the official Homebrew package? (I ask because it'd be nice to just get rid of it some day.)

---

_Merged by @BurntSushi on 2020-04-07 23:45_

---

_Closed by @BurntSushi on 2020-04-07 23:45_

---

_Comment by @simrobin on 2020-04-08 07:30_

I was still following [this instruction](https://github.com/BurntSushi/ripgrep/commit/f3083e4574ad20881de66fdeb66d671f1cbdfda4#diff-04c6e90faac2675aa89e2176d2eec7d8L211-L218) which has been removed a year ago 😅.

I think I will migrate to the official Homebrew package now. 

Thanks for the quick review and suggestion 🙂. (and for maintaining this awesome tool!)

---
