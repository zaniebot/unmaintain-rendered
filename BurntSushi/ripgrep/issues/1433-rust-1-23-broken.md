```yaml
number: 1433
title: rust 1.23 broken?
type: issue
state: closed
author: luohao123
labels:
  - invalid
assignees: []
created_at: 2019-11-23T14:28:06Z
updated_at: 2019-11-23T14:34:31Z
url: https://github.com/BurntSushi/ripgrep/issues/1433
synced_at: 2026-01-12T16:13:23Z
```

# rust 1.23 broken?

---

_@luohao123_

```
error: `..=` syntax in patterns is experimental (see issue #28237)
    --> /home/jintian/.cargo/registry/src/mirrors.ustc.edu.cn-12df342d903acd47/regex-syntax-0.6.12/src/ast/parse.rs:1456:13
     |
1456 |             '0'..='7' => {
     |             ^^^^^^^^^

error: `..=` syntax in patterns is experimental (see issue #28237)
    --> /home/jintian/.cargo/registry/src/mirrors.ustc.edu.cn-12df342d903acd47/regex-syntax-0.6.12/src/ast/parse.rs:1467:13
     |
1467 |             '8'..='9' if !self.parser().octal => {
     |             ^^^^^^^^^

error: `..=` syntax in patterns is experimental (see issue #28237)
   --> /home/jintian/.cargo/registry/src/mirrors.ustc.edu.cn-12df342d903acd47/regex-syntax-0.6.12/src/hir/translate.rs:705:17
    |
705 |                 'A'..='Z' | 'a'..='z' => {}
    |                 ^^^^^^^^^

error: `..=` syntax in patterns is experimental (see issue #28237)
   --> /home/jintian/.cargo/registry/src/mirrors.ustc.edu.cn-12df342d903acd47/regex-syntax-0.6.12/src/hir/translate.rs:705:29
    |
705 |                 'A'..='Z' | 'a'..='z' => {}
    |                             ^^^^^^^^^

error: `..=` syntax in patterns is experimental (see issue #28237)
   --> /home/jintian/.cargo/registry/src/mirrors.ustc.edu.cn-12df342d903acd47/regex-syntax-0.6.12/src/lib.rs:259:16
    |
259 |         b'_' | b'0'..=b'9' | b'a'..=b'z' | b'A'..=b'Z' => true,
    |                ^^^^^^^^^^^

error: `..=` syntax in patterns is experimental (see issue #28237)
   --> /home/jintian/.cargo/registry/src/mirrors.ustc.edu.cn-12df342d903acd47/regex-syntax-0.6.12/src/lib.rs:259:30
    |
259 |         b'_' | b'0'..=b'9' | b'a'..=b'z' | b'A'..=b'Z' => true,
    |                              ^^^^^^^^^^^

error: `..=` syntax in patterns is experimental (see issue #28237)
   --> /home/jintian/.cargo/registry/src/mirrors.ustc.edu.cn-12df342d903acd47/regex-syntax-0.6.12/src/lib.rs:259:44
    |
259 |         b'_' | b'0'..=b'9' | b'a'..=b'z' | b'A'..=b'Z' => true,
    |                                            ^^^^^^^^^^^

error: aborting due to 7 previous errors

error: Could not compile `regex-syntax`.
warning: build failed, waiting for other jobs to finish...
error: failed to compile `ripgrep v11.0.2`, intermediate artifacts can be found at `/tmp/cargo-install.l09iDtb8E22z`

```

---

_Comment by @BurntSushi on 2019-11-23 14:34_

Please consider consulting the README: https://github.com/BurntSushi/ripgrep/blob/master/README.md#building

---

_Closed by @BurntSushi on 2019-11-23 14:34_

---

_Label `invalid` added by @BurntSushi on 2019-11-23 14:34_

---
