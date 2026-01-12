```yaml
number: 1059
title: PCRE2 invalid utf support
type: issue
state: closed
author: zherczeg
labels:
  - enhancement
assignees: []
created_at: 2018-09-19T08:28:48Z
updated_at: 2019-04-14T18:53:47Z
url: https://github.com/BurntSushi/ripgrep/issues/1059
synced_at: 2026-01-12T16:13:22Z
```

# PCRE2 invalid utf support

---

_@zherczeg_

Hi,

I have been working on adding invalid UTF support to PCRE2. Here is my announcement:
https://lists.exim.org/lurker/message/20180917.192454.6e424732.en.html
Only one flag needs to be passed to enable it.

The feature is experimental, but I would be grateful if you could try it. If you experience issues let me know. Could you do some measurements as well?

---

_Comment by @BurntSushi on 2018-09-19 10:51_

Hi! Yes, I filed an issue for it here, which is where work needs to start: https://github.com/BurntSushi/rust-pcre2/issues/5

I do expect there to be at least some improvement, yeah! I'm not sure when I will get around to trying it though. It should be fairly easy to add and thread it through such that ripgrep uses it.

---

_Comment by @BurntSushi on 2018-09-19 10:53_

I think the Julia folks will also be interested in this feature! From what I know, they are currently enabling UCP and UTF modes, while disabling the UTF check and potentially running on invalid UTF-8: https://github.com/JuliaLang/julia/blob/8dd33262d3c88f17ec404e772a5167227c2b26af/base/regex.jl#L7-L8

---

_Comment by @zherczeg on 2018-09-19 12:51_

Thank you. If more people try it more errors could be revealed before the next pcre2 release.

---

_Label `enhancement` added by @BurntSushi on 2019-01-24 15:07_

---

_Comment by @BurntSushi on 2019-04-14 18:53_

I'm going to close this out in favor of https://github.com/BurntSushi/rust-pcre2/issues/5. I'm not sure when I'll get a chance to try this out, but it might not be until the next release (assuming it will be in 10.33).

---

_Closed by @BurntSushi on 2019-04-14 18:53_

---
