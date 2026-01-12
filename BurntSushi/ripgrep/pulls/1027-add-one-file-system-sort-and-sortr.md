```yaml
number: 1027
title: add --one-file-system, --sort and --sortr
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/features
created_at: 2018-08-26T03:39:02Z
updated_at: 2018-08-27T00:35:34Z
url: https://github.com/BurntSushi/ripgrep/pull/1027
synced_at: 2026-01-12T18:23:13Z
```

# add --one-file-system, --sort and --sortr

---

_@BurntSushi_

Closes #321, Closes #404 

cc @okdana Mind checking if I got the completions right? If I got it this time, then I think I've grok'd it.

---

_Comment by @okdana on 2018-08-26 03:58_

No errors in the stuff you added, but:

* The exclusions for `--threads` should be changed from `(--sort-files)` to `(sort)` to make those groups mutually exclusive

* Since you're keeping `--sort-files` around for a while, i might suggest retaining a hidden spec for it, which would look like `!(threads)--sort-files`. (You could also leave one for `--no-sort-files`, but it would be irritating because then you can't just make the whole group exclusive — and it's got be a pretty rarely used option anyway — so i'd leave that out)

* I'd suggest changing the optarg description `sortby` to `sort method`; that'll make more sense in the description message that zsh produces

---

_Comment by @okdana on 2018-08-26 04:05_

btw, to give some context for the changes you're making:

<img width="458" alt="screen shot 2018-08-25 at 23 02 29" src="https://user-images.githubusercontent.com/122095/44624709-07eb1e00-a8bb-11e8-9f7a-7a6f2dfd8076.png">


---

_Comment by @BurntSushi on 2018-08-26 22:42_

@okdana I think I've fixed all of that!

Also, I spent half a day yak shaving. My default shell is now zsh. Let's see how this goes. :-)

---

_Merged by @BurntSushi on 2018-08-26 22:42_

---

_Closed by @BurntSushi on 2018-08-26 22:42_

---

_Branch deleted on 2018-08-26 22:42_

---

_Comment by @BurntSushi on 2018-08-26 22:44_

@okdana I am pretty blown away by the auto completing for `--colors` specifically. Very nicely done.

---

_Comment by @okdana on 2018-08-26 22:50_

Thanks! Let me know if you need anything re: zsh

---

_Comment by @BurntSushi on 2018-08-27 00:35_

@okdana I think I might take you up on that! I sent you mail. :-)

---
