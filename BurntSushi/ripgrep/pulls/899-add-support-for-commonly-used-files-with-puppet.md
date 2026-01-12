```yaml
number: 899
title: Add support for commonly used files with Puppet
type: pull_request
state: merged
author: strangelittlemonkey
labels: []
assignees: []
merged: true
base: master
head: add_puppet_support
created_at: 2018-04-29T06:57:55Z
updated_at: 2018-04-29T16:51:57Z
url: https://github.com/BurntSushi/ripgrep/pull/899
synced_at: 2026-01-12T18:23:13Z
```

# Add support for commonly used files with Puppet

---

_@strangelittlemonkey_

Puppet is primarily written in it's own format of .pp files, but
custom facts and functions are often written in Ruby. The templating
language is ERB and so this will allow scanning of any of the three
most commonly used formats for Puppet specific things.

---

_Merged by @BurntSushi on 2018-04-29 13:21_

---

_Closed by @BurntSushi on 2018-04-29 13:21_

---

_Comment by @BurntSushi on 2018-04-29 13:21_

@strangelittlemonkey Awesome, thank you!

---

_@CYBAI reviewed on 2018-04-29 13:28_

---

_Review comment by @CYBAI on `ignore/src/types.rs`:218 on 2018-04-29 13:28_

Does the `.erb` miss a `*` here?

---

_@BurntSushi reviewed on 2018-04-29 13:31_

---

_Review comment by @BurntSushi on `ignore/src/types.rs`:218 on 2018-04-29 13:31_

Sigh. Yup. Fixed on master. Thanks for noticing this!

---

_@strangelittlemonkey reviewed on 2018-04-29 16:51_

---

_Review comment by @strangelittlemonkey on `ignore/src/types.rs`:218 on 2018-04-29 16:51_

:(  I'm sorry, it was late.

---
