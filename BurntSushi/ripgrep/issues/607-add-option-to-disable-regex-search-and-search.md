```yaml
number: 607
title: Add option to disable regex search and search only for exact matches
type: issue
state: closed
author: hauleth
labels: []
assignees: []
created_at: 2017-09-19T13:41:44Z
updated_at: 2024-06-21T13:51:33Z
url: https://github.com/BurntSushi/ripgrep/issues/607
synced_at: 2026-01-12T16:13:22Z
```

# Add option to disable regex search and search only for exact matches

---

_@hauleth_

This option could also accept `--smart-case` and `--ignore-case` options with would allow to case independent searches.

---

_Comment by @BurntSushi on 2017-09-19 13:42_

@hauleth Why doesn't the `-F/--fixed-strings` flag, which is named after the corresponding flag in `grep`, work for your use case?

---

_Comment by @hauleth on 2017-09-19 13:43_

Oh, I've must missed that option. Sorry for this fuss.

---

_Closed by @hauleth on 2017-09-19 13:43_

---

_Comment by @cedricvanrompay on 2019-11-15 16:32_

I would suggest to add this to the FAQ or to add a section for it in the user guide because it's pretty hard to find in the user guide right now when you don't know the name of the option already.

---

_Comment by @viggy28 on 2021-05-27 05:08_

I didn't find `--fixed-literals`, rather `--fixed-strings` is available.

---

_Comment by @BurntSushi on 2021-05-27 05:21_

Yes, I misspoke. I corrected my comment.

---

_Comment by @guanghechen on 2024-06-21 13:42_

Hi, I just wonder if there is any workaround to search strings contain `\n` with the --fixed-strings option specified like the vscode did?

---

_Comment by @BurntSushi on 2024-06-21 13:51_

@guanghechen Please ask a question via Discussions: https://github.com/BurntSushi/ripgrep/discussions

Please show an example of what you want. Please show what you've tried and the output of what happened when you did.

---
