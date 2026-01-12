```yaml
number: 1183
title: "Gitignore `{}` patterns are inconsistent with official git"
type: issue
state: closed
author: quark-zju
labels:
  - duplicate
assignees: []
created_at: 2019-01-31T22:21:08Z
updated_at: 2019-04-03T13:11:02Z
url: https://github.com/BurntSushi/ripgrep/issues/1183
synced_at: 2026-01-12T16:13:23Z
```

# Gitignore `{}` patterns are inconsistent with official git

---

_@quark-zju_

#### Versions

ignore crate: 0.4.6
git: 2.19.0

#### Problem

The `gitignore` man page does not mention that `{}` should be treated specially. The official git binary does not treat it specially. For example, `{a}` matches the literal `{a}`, not `a`.

The gitignore matcher in the `ignore` crate currently behaves differently.

Possible solution: Add an option in `globset` to disable `{}` handling. Make `gitignore` use it.

#### Reproduce code
 
```bash
git init foo; cd foo; echo '{a,b}' > .gitignore; touch a; touch '{a,b}'; git status
# prints "a" in "untracked files"; does not print "{a,b}"
```

```rust
use ignore::gitignore::GitignoreBuilder;
fn main() {
    let mut b = GitignoreBuilder::new(".");
    b.add_line(None, "{a,b}").unwrap();
    let m = b.build().unwrap();
    println!("'a' matched: {:?}", m.matched("a", false)); // Actual: Ignore; Expect: None
    println!("'{{a,b}}' matched: {:?}", m.matched("{a,b}", false)); // Actual: None; Expect: Ignored
}
```

---

_Renamed from "Official git does not support csh-style `{}` in `.gitignore`" to "Gitignore `{}` patterns are inconsistent with official git" by @quark-zju on 2019-02-01 18:48_

---

_Comment by @BurntSushi on 2019-04-03 13:10_

This ticket is a duplicate of #1221, but since that one has more activity, I'm going to close this one. Thanks for filing the bug!

---

_Closed by @BurntSushi on 2019-04-03 13:10_

---

_Label `duplicate` added by @BurntSushi on 2019-04-03 13:11_

---
