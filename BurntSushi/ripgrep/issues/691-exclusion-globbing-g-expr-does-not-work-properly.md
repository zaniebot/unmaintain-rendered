```yaml
number: 691
title: "Exclusion globbing (`-g '!expr'`) does not work properly with double quotes"
type: issue
state: closed
author: akien-mga
labels: []
assignees: []
created_at: 2017-11-26T21:38:15Z
updated_at: 2017-11-26T22:40:10Z
url: https://github.com/BurntSushi/ripgrep/issues/691
synced_at: 2026-01-12T16:13:22Z
```

# Exclusion globbing (`-g '!expr'`) does not work properly with double quotes

---

_@akien-mga_

After hitting this issue relatively often since I started using ripgrep, I finally understood what the exact issue appears to be.

When trying to exclude a given folder from my search paths with the globbing option `-g`, it fails if the expression string is enclosed with double quotes (`"`). It works fine with simple quotes (`'`):
```
$ rg -g "!thirdparty" test
bash: !thirdparty: event not found

$ rg -g '!thirdparty' test
servers/visual_server.h:        RID _make_test_cube();
servers/visual_server.h:        RID test_texture;
[...]
```

I don't know if the issue can be fixed in ripgrep or if it's a limitation of Bash, but I've been using `'` and `"` interchangeably for many years in my shell without notable issues.

Environment:
- Mageia 6 x86_64
- ripgrep 0.7.1 / -AVX -SIMD, built with cargo 0.22.0 and rustc 1.21.0
- bash 4.3.48

*Edit:* Upgraded ripgrep to 0.7.1, same issue (report was originally about 0.6.0).

---

_Comment by @okdana on 2017-11-26 22:32_

That is just the way history expansion works in bash. It's similar to `$` in that it expands inside double-quotes but not single-quotes. You have four options:

* Use single-quotes to suppress expansion
* Use a back-slash to escape the `!` (`\!thirdparty`)
* Disable history expansion by running `set +H` or adding it to your profile
* Change the history-expansion character by setting the `histchars` variable; for example, `histchars='%^#'` will change it from `!` to `%`

---

_Comment by @akien-mga on 2017-11-26 22:40_

Thanks a lot for the detailed answer! I think I'll start using `\!` for ripgrep, that's something I should easily remember and avoid bad surprise when I pick the wrong quotes.

Closing as it's not a bug, though it might be worth adding a mention to this in the `--help` output if other shells work similarly - if it's only bash, it's maybe not worth cluttering the docs.

---

_Closed by @akien-mga on 2017-11-26 22:40_

---
