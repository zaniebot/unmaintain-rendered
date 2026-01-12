```yaml
number: 488
title: Should show filename for one file with vimgrep
type: pull_request
state: merged
author: ericbn
labels: []
assignees: []
merged: true
base: master
head: vimgrep-with-filename
created_at: 2017-05-25T01:03:43Z
updated_at: 2017-05-25T12:12:17Z
url: https://github.com/BurntSushi/ripgrep/pull/488
synced_at: 2026-01-12T18:23:13Z
```

# Should show filename for one file with vimgrep

---

_@ericbn_

With vim configured with:

    set grepprg=rg\ --vimgrep
    set grepformat^=%f:%l:%c:%m

and running the command `:grep 'vimgrep' doc/rg.1`, the output should be:

    doc/rg.1:446:8:.B \-\-vimgrep

but the actual output was:

    446:8:.B \-\-vimgrep

Same issue would happen if results only match one file. Ag behaves as expected.

---

_Comment by @BurntSushi on 2017-05-25 01:27_

Hmm, I think my original intention was that you'd want to pass `--with-filename` for this, but I'm guessing that the output of `--vimgrep` is completely useless if there's no file name, right?

---

_Comment by @ericbn on 2017-05-25 02:48_

The grep command for vim should be only configured as `set grepprg=rg\ --vimgrep`, right?

Now, the default grep format in vim is (see `:help grepformat`) `"%f:%l:%m,%f:%l%m,%f  %l%m"`,  meaning `<filename>:<linenumber>:<text>` or the variations that follow. Both `%f` and `%l` are the minimal elements required for a useful output.

Ag introduced the column number on it's `--vimgrep` output, requiring the extra `set grepformat^=%f:%l:%c:%m` configuration (the `^=` prepends to the existing default comma-separated list). Vim supports even more configurable elements (see `:help errorformat`), and having  the column number better positions the cursor when browsing the results.

TL;DR yeah, the `--vimgrep` output is useless without the file names.

---

_Comment by @BurntSushi on 2017-05-25 03:12_

> The grep command for vim should be only configured as set grepprg=rg\ --vimgrep, right?

I don't know. I don't use ripgrep inside of vim.

But I buy it! Thanks!

---

_Merged by @BurntSushi on 2017-05-25 03:12_

---

_Closed by @BurntSushi on 2017-05-25 03:12_

---

_Branch deleted on 2017-05-25 12:12_

---
