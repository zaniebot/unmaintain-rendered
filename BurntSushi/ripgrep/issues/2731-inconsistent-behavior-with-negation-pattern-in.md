```yaml
number: 2731
title: "Inconsistent behavior with negation pattern in `.gitignore`"
type: issue
state: closed
author: Nadrieril
labels:
  - bug
  - rollup
assignees: []
created_at: 2024-02-07T17:35:45Z
updated_at: 2025-09-20T01:08:21Z
url: https://github.com/BurntSushi/ripgrep/issues/2731
synced_at: 2026-01-12T16:13:24Z
```

# Inconsistent behavior with negation pattern in `.gitignore`

---

_@Nadrieril_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

### How did you install ripgrep?

Using nix

### What operating system are you using ripgrep on?

NixOS 23.11

### Describe your bug.

See below. Feels related to https://github.com/BurntSushi/ripgrep/issues/1050 but I think they're different.

### What are the steps to reproduce the behavior?

Make a repo with:

#### .gitignore

```gitignore
build/
!/some_dir/build/
```

#### some_dir/build/foo

```txt
string
```

### What is the actual behavior?

```shell
> rg -l string
some_dir/build/foo
> rg -l string some_dir
# nothing
> rg -l string some_dir/build
some_dir/build/foo
```

### What is the expected behavior?

`rg -l string some_dir` should find the file too.

---

_Renamed from "Surprising behavior with negation pattern in `.gitignore`" to "Inconsistent behavior with negation pattern in `.gitignore`" by @Nadrieril on 2024-02-07 17:50_

---

_Comment by @BurntSushi on 2024-02-07 18:32_

Please try the latest release of ripgrep.

---

_Comment by @Nadrieril on 2024-02-07 22:08_

Oh, sorry, that appears to be fixed in 14.0

---

_Closed by @Nadrieril on 2024-02-07 22:08_

---

_Closed by @Nadrieril on 2024-02-07 22:08_

---

_Reopened by @Nadrieril on 2024-02-07 22:13_

---

_Comment by @Nadrieril on 2024-02-07 22:15_

Ah nope, still a bug but it requires one more level of nesting:

```
ripgrep 14.1.0

features:-simd-accel,-pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 is not available in this build of ripgrep.
```

#### .gitignore

```gitignore
build/
!/some_dir/subdir/build/
```

#### some_dir/subdir/build/foo

```txt
string
```

### What is the actual behavior?

```shell
> rg -l string
some_dir/subdir/build/foo
> rg -l string some_dir
# nothing
> rg -l string some_dir/subdir
some_dir/subdir/build/foo
> rg -l string some_dir/subdir/build
some_dir/subdir/build/foo
```

---

_Comment by @Noratrieb on 2024-02-11 20:16_

I can reproduce this with the `ignore` crate directly and
```rust
use ignore::Walk;
fn main() {
    for result in Walk::new("./some_dir") {
        match result {
            Ok(entry) => println!("{}", entry.path().display()),
            Err(err) => println!("ERROR: {}", err),
        }
    }
}
```
(with the CWD in the directory with the `.gitignore`)

---

_Label `bug` added by @BurntSushi on 2024-02-11 20:20_

---

_Comment by @BurntSushi on 2024-02-11 20:21_

This is probably a duplicate of an existing bug, but they are difficult to find.

There are several open issues related to gitignore support that I don't plan on fixing until I've had a chance to rethink the ignore crate from first principles. It *might* happen within the next year.

---

_Comment by @ybc37 on 2024-02-15 10:08_

I found a similar bug when using wildcards in `.gitignore`. Let me know, if I should open a new issue.

Script to reproduce the bug:

```sh
#!/usr/bin/env sh

mkdir -p bug/foo/bar && cd bug
git init
echo "baz" > foo/bar/baz.txt
echo "/foo/bar/baz*.txt" > .gitignore

rg -l baz
# Output:
# rg: No files were searched, which means ripgrep probably applied a filter you didn't expect.
# Running with --debug will show why files are being skipped.

rg -l baz foo/
# Output:
# foo/bar/baz.txt
```

Version:

```
ripgrep 14.1.0

features:-simd-accel,+pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 10.42 is available (JIT is available)
```

---

_Comment by @ybc37 on 2024-02-16 09:40_

Actually, I just realized that the wildcard isn't even needed. The bug is reproducible with a `.gitignore` containing `/foo/bar/baz.txt` (see script in my previous comment).

---

_Comment by @RossSmyth on 2024-03-22 17:00_

It may be worth looking at https://github.com/Byron/gitoxide/tree/main/gix-ignore when refactoring, or somehow pulling in the logic from it as a library.

---

_Comment by @WalterScottYoung on 2024-11-15 12:28_

i think this is essentially the same bug as https://github.com/BurntSushi/ripgrep/issues/2836, and I tested https://github.com/BurntSushi/ripgrep/pull/2933 is able to fix this.

---

_Label `rollup` added by @BurntSushi on 2025-07-06 16:53_

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---
