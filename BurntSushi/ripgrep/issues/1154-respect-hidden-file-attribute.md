```yaml
number: 1154
title: respect hidden file attribute
type: issue
state: closed
author: ghost
labels:
  - bug
assignees: []
created_at: 2019-01-08T12:39:03Z
updated_at: 2019-01-27T17:14:24Z
url: https://github.com/BurntSushi/ripgrep/issues/1154
synced_at: 2026-01-12T16:13:23Z
```

# respect hidden file attribute

---

_@ghost_

currently RipGrep considers a hidden file something starting with `.`, which is a
long time Linux convention

https://github.com/BurntSushi/ripgrep/blob/c3db8db93d048be6e4bc62c2d5bdf0e2fee65950/ignore/src/pathutil.rs#L16-L24

However the Windows convention of the hidden file attribute is not recognized by
RipGrep. So if you have some files on Windows with the hidden file attribute,
RipGrep will always show them regardless, unless they also happen to start with `.`

---

_Label `bug` added by @BurntSushi on 2019-01-08 12:41_

---

_Comment by @BurntSushi on 2019-01-08 12:44_

Seems reasonable to me. It looks like we need to check whether the [file attributes](https://docs.rs/winapi-util/0.1.1/x86_64-pc-windows-msvc/winapi_util/file/struct.Information.html#method.file_attributes) contain `FILE_ATTRIBUTE_HIDDEN`.

The one concern I have here is that if this is done naively, it will introduce an additional stat call for every file ripgrep looks at, by default. We should try to avoid that by looking for an existing stat call and reusing its information.

---

_Comment by @ghost on 2019-01-08 13:08_

@BurntSushi thanks for fast response

it probably goes without saying, but i am of that opinion that even if this is
implemented it should **not** be the default, as it seems we are talking about
a performance hit

users should need to opt into this, with the default remaining the faster
option. thank you!


---

_Closed by @BurntSushi on 2019-01-27 17:14_

---
