```yaml
number: 1471
title: cargo install ripgrep   no longer works
type: issue
state: closed
author: flip111
labels:
  - invalid
assignees: []
created_at: 2020-01-23T14:12:49Z
updated_at: 2020-01-23T14:21:55Z
url: https://github.com/BurntSushi/ripgrep/issues/1471
synced_at: 2026-01-12T16:13:23Z
```

# cargo install ripgrep   no longer works

---

_@flip111_

In the readme it mentions `cargo install ripgrep` i believe when i ran that command ripgrep was installed in `$HOME/.local` (`bin` directory in there) EDIT: might have been mistaken and it was `$HOME/.cargo/bin`. However now i get this error:

```
error: Using `cargo install` to install the binaries for the package in current working directory is no longer supported, use `cargo install --path .` instead. Use `cargo build` if you want to simply build the package.
```

How can i compile from source and then install ?

---

_Comment by @flip111 on 2020-01-23 14:14_

My bad, i was just using `cargo install` seems to work when specifying `ripgrep`

---

_Closed by @flip111 on 2020-01-23 14:14_

---

_Label `invalid` added by @BurntSushi on 2020-01-23 14:21_

---

_Comment by @BurntSushi on 2020-01-23 14:21_

`cargo install ripgrep` installs ripgrep from crates.io. Running the command shown to you in your error message in a checkout of the ripgrep repo will compile and install from source.

---
