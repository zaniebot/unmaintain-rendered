```yaml
number: 3262
title: "`$F` is treated as a capture group despite using `--fixed-strings`"
type: issue
state: closed
author: tgharib
labels:
  - wontfix
assignees: []
created_at: 2026-01-09T14:27:55Z
updated_at: 2026-01-10T15:28:57Z
url: https://github.com/BurntSushi/ripgrep/issues/3262
synced_at: 2026-01-12T16:13:25Z
```

# `$F` is treated as a capture group despite using `--fixed-strings`

---

_@tgharib_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

ripgrep 15.1.0

### How did you install ripgrep?

Using the Arch (btw) package manager `pacman`

### What operating system are you using ripgrep on?

CachyOS Linux

### Describe your bug.

`$F` is treated as a capture group but I am using `--fixed-strings` so it should be treated as a literal.

### What are the steps to reproduce the behavior?

```bash
cat <<EOF > test.md
Recall that we defined the sum of two elements of 𝐅𝑛 to be the element of 𝐅𝑛 obtained by adding corresponding coordinates; see 1.13. As we will now see, addition has a simple geometric interpretation in the special case of 𝐑2.
EOF
rg --fixed-strings "𝐅𝑛" --replace='$F^n$' test.md
```

### What is the actual behavior?

Actual output is:

```
rg: DEBUG|rg::flags::parse|crates/core/flags/parse.rs:97: no extra arguments found from configuration file
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:954: read CWD from environment: /home/owner
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1092: number of paths given to search: 1
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1103: is_one_file? true
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1278: found hostname for hyperlink configuration: Taha-Desktop-Linux
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1288: hyperlink format: ""
rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:175: using 1 thread(s)
rg: DEBUG|grep_regex::config|/usr/src/debug/ripgrep/ripgrep-15.1.0/crates/regex/src/config.rs:175: assembling HIR from 1 fixed string literals
1:Recall that we defined the sum of two elements of ^n$ to be the element of ^n$ obtained by adding corresponding coordinates; see 1.13. As we will now see, addition has a simple geometric interpretation in the special case of 𝐑2.
```

### What is the expected behavior?

Output expected is:

```
1:Recall that we defined the sum of two elements of $F^n$ to be the element of $F^n$ obtained by adding corresponding coordinates; see 1.13. As we will now see, addition has a simple geometric interpretation in the special case of 𝐑2.
```

---

_Comment by @BurntSushi on 2026-01-09 14:50_

This is intended and correct behavior. Even with `-F/--fixed-strings`, the capture group `$0` is still available. Moreover, there is no documentation suggesting or implying that the _interpretation_ of the `-r/--replace` string is dependent on whether `-F/--fixed-strings` is enabled. I don't think it would be a good idea to do so. It's quite subtle. The rule, "you always need to escape `$` to write a literal `$`" is simpler and not conditional.

---

_Closed by @BurntSushi on 2026-01-09 14:50_

---

_Label `wontfix` added by @BurntSushi on 2026-01-09 14:50_

---

_Comment by @tgharib on 2026-01-09 16:43_

Is there any way to treat the `--replace` string as literal? Sometimes I just want to do a straightforward search and replace without having to worry about escapes.

---

_Comment by @BurntSushi on 2026-01-09 16:55_

You could write a function that escapes `$`. But no, nothing in ripgrep that will do it.

---

_Comment by @tgharib on 2026-01-10 15:28_

Thanks by the way for this software! I've been using it for years.

---
