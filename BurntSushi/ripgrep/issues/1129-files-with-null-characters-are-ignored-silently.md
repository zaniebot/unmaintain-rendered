```yaml
number: 1129
title: Files with null characters are ignored silently
type: issue
state: closed
author: lily-mara
labels:
  - duplicate
assignees: []
created_at: 2018-11-29T17:34:50Z
updated_at: 2018-11-29T17:57:51Z
url: https://github.com/BurntSushi/ripgrep/issues/1129
synced_at: 2026-01-12T16:13:23Z
```

# Files with null characters are ignored silently

---

_@lily-mara_

#### What version of ripgrep are you using?

ripgrep 0.8.1
-SIMD -AVX

#### How did you install ripgrep?

cargo install

#### What operating system are you using ripgrep on?

macOS Sierra 10.12.6

#### If this is a bug, what are the steps to reproduce the behavior?

```shell
$ echo '\x00\ntest' > foo
$ rg test foo
```

#### If this is a bug, what is the actual behavior?

The file with `\x00` in it is ignored by ripgrep.

```
λ rg --debug test bar
DEBUG/grep::search//Users/nate/.cargo/registry/src/github.com-1ecc6299db9ec823/grep-0.1.8/src/search.rs:195: regex ast:
Literal {
    chars: [
        't',
        'e',
        's',
        't'
    ],
    casei: false
}
DEBUG/grep::literals//Users/nate/.cargo/registry/src/github.com-1ecc6299db9ec823/grep-0.1.8/src/literals.rs:38: literal prefixes detected: Literals { lits: [Complete(test)], limit_size: 250, limit_class: 10 }
DEBUG/globset//Users/nate/.cargo/registry/src/github.com-1ecc6299db9ec823/globset-0.4.1/src/lib.rs:429: built glob set; 0 literals, 2 basenames, 0 extensions, 0 prefixes, 1 suffixes, 1 required extensions, 0 regexes
```

#### If this is a bug, what is the expected behavior?

The string "test" should be found by ripgrep, or ripgrep should report an error indicating that the file will not be processed.

---

_Comment by @BurntSushi on 2018-11-29 17:57_

This is a duplicate of #306. I've added a comment there explaining more about what the next steps might be.

---

_Label `duplicate` added by @BurntSushi on 2018-11-29 17:57_

---

_Closed by @BurntSushi on 2018-11-29 17:57_

---
