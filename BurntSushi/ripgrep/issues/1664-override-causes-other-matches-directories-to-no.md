```yaml
number: 1664
title: Override causes other matches directories to no longer grep recursively
type: issue
state: closed
author: nataliescottdavidson
labels:
  - wontfix
assignees: []
created_at: 2020-08-26T14:53:46Z
updated_at: 2020-08-26T15:22:35Z
url: https://github.com/BurntSushi/ripgrep/issues/1664
synced_at: 2026-01-12T16:13:24Z
```

# Override causes other matches directories to no longer grep recursively

---

_@nataliescottdavidson_

#### What version of ripgrep are you using?

ignore = "0.4.16"

#### How did you install ripgrep?

As a dependency in my `Cargo.toml`

#### What operating system are you using ripgrep on?

Ubuntu 18.04

#### Describe your bug.

Override causes other matches directories to no longer grep recursively

#### What are the steps to reproduce the behavior?

Minimized example here: https://github.com/nataliescottdavidson/walkbuilder-example

## How to reproduce issue

Run test-setup.sh

Output before using override: 

``` File "test_dir"
File "test_dir"
File "test_dir/other_nested_dir"
File "test_dir/other_nested_dir/file1.txt"
File "test_dir/nested_dir"
File "test_dir/nested_dir/file1.txt"
File "test_dir/nested_dir/file.txt"
File "test_dir/nested_dir/file2.txt"
File "test_dir/file.txt"
```

Expected output after using override:

```File "test_dir"
File "test_dir"
File "test_dir/other_nested_dir"
File "test_dir/other_nested_dir/file1.txt"
File "test_dir/nested_dir"
File "test_dir/nested_dir/file1.txt"
File "test_dir/nested_dir/file.txt"
File "test_dir/nested_dir/file2.txt"
File "test_dir/file.txt"
File "test_dir/.hidden_dir/file.txt"
File "test_dir/.hidden_dir/.hidden_file.txt"
```

Actual output after using override:

```
File "test_dir"
File "test_dir/other_nested_dir"
File "test_dir/nested_dir"
File "test_dir/.hidden_dir"


---

_Comment by @BurntSushi on 2020-08-26 14:58_

This is correct. Specifying an override creates a whitelist, which means that all paths yielded _must_ be in that whitelist. The behavior you're asking for doesn't really make sense. e.g., `rg foo -g '*.rs'` for example would wind up searching everything anyway. And that's the use case that `Overrides` is intended for.

---

_Closed by @BurntSushi on 2020-08-26 14:58_

---

_Label `invalid` added by @BurntSushi on 2020-08-26 14:58_

---

_Comment by @BurntSushi on 2020-08-26 14:59_

Thank you for the very nice reproduction though. That is much appreciated.

---

_Label `invalid` removed by @BurntSushi on 2020-08-26 14:59_

---

_Label `wontfix` added by @BurntSushi on 2020-08-26 14:59_

---

_Comment by @nataliescottdavidson on 2020-08-26 15:05_

Ah ok. Is there a way to produce the desired behavior of matching all non-hidden dirs and files recursively, + a single specified hidden dir and its contents?
 

---

_Comment by @BurntSushi on 2020-08-26 15:22_

It's a little awkward, but you could do use these globs (in this order):

```
*
!.*
.whitelist
```

This basically says, "match everything, but exclude all hidden dirs, but include this one hidden dir." At least, I think that should work.

---
