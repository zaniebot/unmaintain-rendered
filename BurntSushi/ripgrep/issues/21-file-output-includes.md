```yaml
number: 21
title: "`--file` output includes './'"
type: issue
state: closed
author: cloudhead
labels:
  - enhancement
assignees: []
created_at: 2016-09-23T17:31:11Z
updated_at: 2016-09-24T23:18:51Z
url: https://github.com/BurntSushi/ripgrep/issues/21
synced_at: 2026-01-12T18:23:11Z
```

# `--file` output includes './'

---

_@cloudhead_

Hey, I'm playing around with `rg` as a replacement for `ag` in my [fuzzy file finder](https://github.com/cloudhead/neovim-fuzzy) and I noticed the output of `rg --files` includes a './' in the output, ex:

```
./src/file.c
./src/file2.c
```

In comparison to `ag -g ''` or `ack -g ''` which looks like this:

```
src/file.c
src/file2.c
```

Would you consider adding an option for this or changing it to match ack/ag? I'm aware I can use sed, but it would be nice if it was built-in.

Thanks


---

_Comment by @BurntSushi on 2016-09-23 17:35_

I think the current behavior matches `find` right?

I'm not opposed to adding a flag. What about changing the default output instead? 


---

_Comment by @cloudhead on 2016-09-23 17:40_

Yes, the current behaviour does match `find`. It has an option though (`-print`?) to change the output, but I'd say that's probably not worth implementing at this point. I'd be happy if the default didn't include `./`, I think in general it might be a better default.


---

_Label `enhancement` added by @BurntSushi on 2016-09-24 02:23_

---

_Closed by @BurntSushi on 2016-09-24 23:18_

---
