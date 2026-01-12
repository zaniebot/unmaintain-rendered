```yaml
number: 407
title: "can't install 0.5.0 with homebrew"
type: issue
state: closed
author: cvlmtg
labels: []
assignees: []
created_at: 2017-03-13T08:46:40Z
updated_at: 2017-03-13T10:56:15Z
url: https://github.com/BurntSushi/ripgrep/issues/407
synced_at: 2026-01-12T16:13:21Z
```

# can't install 0.5.0 with homebrew

---

_@cvlmtg_

I've tried both an update and a clean install and I also untapped/tapped the formula again.

```
~ brew install burntsushi/ripgrep/ripgrep-bin
==> Installing ripgrep-bin from burntsushi/ripgrep
==> Downloading https://github.com/BurntSushi/ripgrep/releases/download/0.5.0/ripgrep-0.5.0-x86_64-a
==> Downloading from https://github-cloud.s3.amazonaws.com/releases/53631945/bbe71998-0779-11e7-9aae
######################################################################## 100,0%
Error: SHA256 mismatch
Expected: efdc64456fc64910c2998b25c402aac2ff17caf1507e8bcca4584b429ef7f7ae
Actual: 5bfa8872c4f2a5d010ddec1c213d518056e62d4dd3b3f23a0ef099b85343dbdd
Archive: /Users/matteo/Library/Caches/Homebrew/ripgrep-bin-0.5.0.tar.gz
To retry an incomplete download, remove the file above.
```


---

_Comment by @mitsuhiko on 2017-03-13 08:50_

Also the main brew repo is on 0.4 at the moment.

---

_Closed by @BurntSushi on 2017-03-13 10:51_

---

_Comment by @BurntSushi on 2017-03-13 10:53_

Should be fixed. I'll see about updating brew proper later today.

---

_Comment by @cvlmtg on 2017-03-13 10:56_

it worked, thanks!



---
