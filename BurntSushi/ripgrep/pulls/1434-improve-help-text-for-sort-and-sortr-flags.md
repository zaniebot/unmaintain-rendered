```yaml
number: 1434
title: "Improve help text for `--sort` and `--sortr` flags"
type: pull_request
state: closed
author: mikkovedru
labels:
  - rollup
assignees: []
base: master
head: help-improvements
created_at: 2019-11-23T17:12:01Z
updated_at: 2020-02-18T13:02:44Z
url: https://github.com/BurntSushi/ripgrep/pull/1434
synced_at: 2026-01-12T18:23:13Z
```

# Improve help text for `--sort` and `--sortr` flags

---

_@mikkovedru_

I improved the help documentation in the following manner and for the 
following reasons:
1. It's only logical to put the default sub-option on the first possible 
line, as well as to separately mention that it is indeed the default 
sub-option.
2. Additional options for the flags should describe the main points of 
their purpose without requiring user to read the whole help entry. In my 
opinion, the information sub-options' influence on multi-threading and 
speed are important enough to warrant their inclusion in each 
sub-option's description line text.

---

_Label `rollup` added by @BurntSushi on 2020-02-15 22:33_

---

_Closed by @BurntSushi on 2020-02-17 22:16_

---

_Branch deleted on 2020-02-18 13:02_

---
