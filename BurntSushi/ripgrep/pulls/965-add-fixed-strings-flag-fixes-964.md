```yaml
number: 965
title: "Add --fixed-strings flag (fixes #964)"
type: pull_request
state: merged
author: okdana
labels: []
assignees: []
merged: true
base: master
head: dana/964
created_at: 2018-06-25T20:50:55Z
updated_at: 2018-06-26T20:19:35Z
url: https://github.com/BurntSushi/ripgrep/pull/965
synced_at: 2026-01-12T18:23:13Z
```

# Add --fixed-strings flag (fixes #964)

---

_@okdana_

Simple as this probably? I didn't add a test since i don't see any for other options like this, but let me know if you'd like one.

I also didn't update the completion function. I guess we need to decide whether it's useful to show all of the `--no-*` options; i'm leaning towards not usually. But i should add them as hidden options at least so that the exclusions are accurate. I'll do that eventually...

(Fixes #964)

---

_@BurntSushi approved on 2018-06-25 21:01_

LGTM, thanks! And yeah, I'm not too particular about adding tests for these since I'm not sure they are worth it.

I defer to you on how to handle the zsh completions. :-)

---

_Merged by @BurntSushi on 2018-06-25 21:02_

---

_Closed by @BurntSushi on 2018-06-25 21:02_

---

_Comment by @okdana on 2018-06-26 00:19_

I just realised i mistyped the commit message on this. Sorry for the confusion that's going to cause in the future. :/

---

_Comment by @BurntSushi on 2018-06-26 00:21_

@okdana Hah whoops. Missed that! Should be fine!

---

_Comment by @jsatk on 2018-06-26 20:19_

Thanks for this. 

---
