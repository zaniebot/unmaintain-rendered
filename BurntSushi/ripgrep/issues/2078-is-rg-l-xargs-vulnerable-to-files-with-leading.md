```yaml
number: 2078
title: "Is \"rg -l | xargs\" vulnerable to files with leading dashes?"
type: issue
state: closed
author: jzinn
labels:
  - wontfix
assignees: []
created_at: 2021-11-19T18:57:41Z
updated_at: 2021-11-20T21:05:51Z
url: https://github.com/BurntSushi/ripgrep/issues/2078
synced_at: 2026-01-12T16:13:24Z
```

# Is "rg -l | xargs" vulnerable to files with leading dashes?

---

_@jzinn_

Should `rg -l` prepend `./` to relative paths when outputting to a non-interactive tty to protect against files named with leading dashes being unintentionally interpreted as flags by `xargs` commands?

`fd` is looking at this: https://github.com/sharkdp/fd/issues/760.  `rg -l` seems similar.

#### Edit

If it's a problem, it's bigger than `rg -l`:

Actual
```
$ echo there > hi
$ rg ere
hi
1:there
$ rg ere | cat
hi:there
```

Expected?

```
$ rg ere | cat
./hi:there
```


---

_Comment by @BurntSushi on 2021-11-19 19:21_

No, I don't think this is necessary. Not even GNU grep does this. But in either case, if you want to have `./` preprended to each path, then you just need to specify it:

```
$ rg -l 'fn is_empty'
GUIDE.md
crates/globset/src/lib.rs
crates/ignore/src/overrides.rs
crates/ignore/src/gitignore.rs
crates/ignore/src/types.rs
crates/matcher/src/lib.rs
crates/cli/src/process.rs

$ rg -l 'fn is_empty' ./
./GUIDE.md
./crates/globset/src/lib.rs
./crates/ignore/src/types.rs
./crates/ignore/src/gitignore.rs
./crates/ignore/src/overrides.rs
./crates/matcher/src/lib.rs
./crates/cli/src/process.rs

$ grep -rl 'fn is_empty'
crates/matcher/src/lib.rs
crates/cli/src/process.rs
crates/ignore/src/types.rs
crates/ignore/src/gitignore.rs
crates/ignore/src/overrides.rs
crates/globset/src/lib.rs
GUIDE.md

$ grep -rl 'fn is_empty' ./
./crates/matcher/src/lib.rs
./crates/cli/src/process.rs
./crates/ignore/src/types.rs
./crates/ignore/src/gitignore.rs
./crates/ignore/src/overrides.rs
./crates/globset/src/lib.rs
./GUIDE.md
```


---

_Comment by @jzinn on 2021-11-19 19:51_

Grep and posix have history and compatibility restraints, and this would be a breaking change.  Maybe it's a good idea to prefix, but compatibility prevents it?  Maybe @ken considered it and decided it's not necessary?

---

_Comment by @BurntSushi on 2021-11-19 20:02_

I don't see this as necessary. File paths starting with a `-` are not something I've ever seen personally, _and_ there's a way to work-around it.

---

_Closed by @BurntSushi on 2021-11-19 20:02_

---

_Label `wontfix` added by @BurntSushi on 2021-11-19 20:02_

---
