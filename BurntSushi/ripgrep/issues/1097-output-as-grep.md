```yaml
number: 1097
title: output as grep
type: issue
state: closed
author: nkh
labels:
  - question
assignees: []
created_at: 2018-10-31T17:14:47Z
updated_at: 2020-09-08T16:16:57Z
url: https://github.com/BurntSushi/ripgrep/issues/1097
synced_at: 2026-01-12T16:13:22Z
```

# output as grep

---

_@nkh_

#### What version of ripgrep are you using?
ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?
cargo

#### What operating system are you using ripgrep on?
KDE.

#### Describe your question, feature request

option to put the file name on the same line as the search result, multiple time, as grep does, which makes it a drop in replacement rather than another tool to handle in a special way.


---

_Comment by @okdana on 2018-10-31 17:35_

ripgrep has many many options to control the output format. `rg --no-heading -N` will give you output that looks like `grep -R`. This is also the format used by default when the output is being directed into a file or pipe. You can put those options in an alias or your ripgrep config file if you want it to always work like that. All of this is mentioned in the usage help, the [guide](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md), &c.

Also, i think @BurntSushi might point out that ripgrep is explicitly not designed to be a 'drop in replacement' for `grep`, although it can work like that in some cases. This is discussed in a few places in the documentation, but especially [here](https://github.com/BurntSushi/ripgrep/blob/master/FAQ.md#posix4ever)

---

_Closed by @BurntSushi on 2018-10-31 17:39_

---

_Label `question` added by @BurntSushi on 2018-10-31 17:39_

---

_Comment by @jacobeatsspam on 2020-09-01 18:21_

Unfortunately `--no-heading` does not include a ` ` following the `:`, making IDE ctrl+click navigation fail. I haven't dipped into rust yet or I'd submit a PR.

---

_Comment by @krobelus on 2020-09-01 18:37_

@jacobisaliveandwell probably your IDE could be smarter about this; grep does not add a space either.
You could add it using `rg foo --no-heading --column | sed -E 's/^[^:]*:([0-9]+:){2}/& /g'`

---

_Comment by @jacobeatsspam on 2020-09-08 16:16_

Indeed. My apologies. `grep` does this too. Something must have changed in vscode. disregard me.

---
