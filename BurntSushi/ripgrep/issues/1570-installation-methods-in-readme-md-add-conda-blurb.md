```yaml
number: 1570
title: Installation methods in README.md - add conda blurb?
type: issue
state: closed
author: rltillett
labels:
  - question
assignees: []
created_at: 2020-05-06T19:26:16Z
updated_at: 2020-05-06T19:34:23Z
url: https://github.com/BurntSushi/ripgrep/issues/1570
synced_at: 2026-01-12T16:13:23Z
```

# Installation methods in README.md - add conda blurb?

---

_@rltillett_

As that miniconda is a somewhat popular tool for installing stuff on machines where users don't have sudo, and as that it looks like the `conda-forge` repo maintains a ripgrep recipe (and it is up-to-date at the time of my writing), you might consider mentioning a conda recipe in the Installation section of the README.md ?

Something like this gets the job done
```
conda install -c conda-forge ripgrep
```

Not exactly a feature request, but not a bug either, so I filed it here.

---

_Comment by @BurntSushi on 2020-05-06 19:34_

I'm not familiar with Conda, but as long as it doesn't have any telemetry embedded into installed programs, then I'm happy to include it in the README. Folks should feel free to just file a PR for small stuff like this. I don't usually track things like this in separate issues. Thanks!

---

_Closed by @BurntSushi on 2020-05-06 19:34_

---

_Label `question` added by @BurntSushi on 2020-05-06 19:34_

---
