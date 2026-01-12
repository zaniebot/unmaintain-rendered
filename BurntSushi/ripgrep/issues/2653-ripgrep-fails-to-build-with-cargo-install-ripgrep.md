```yaml
number: 2653
title: "ripgrep fails to build with `cargo install ripgrep`: \"linking with 'link.exe' failed: exit code: 1327\""
type: issue
state: closed
author: JSorngard
labels:
  - bug
assignees: []
created_at: 2023-11-26T21:19:17Z
updated_at: 2023-11-26T22:38:17Z
url: https://github.com/BurntSushi/ripgrep/issues/2653
synced_at: 2026-01-12T16:13:24Z
```

# ripgrep fails to build with `cargo install ripgrep`: "linking with 'link.exe' failed: exit code: 1327"

---

_@JSorngard_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

failed to build repgrep 14.0.0

### How did you install ripgrep?

I am attempting to build it from source with the latest version of cargo on the latest version of rust (I ran `rustup update` before attempting the build) by doing `cargo install ripgrep`.

### What operating system are you using ripgrep on?

Windows 11 version 22H2

### Describe your bug.

ripgrep version 14.0.0 fails to build from source with `cargo install ripgrep` due to a link error.

### What are the steps to reproduce the behavior?

I just have to write `cargo install ripgrep` in the terminal to get the error, but I am not sure how to reproduce it on someone elses machine if it works for them.

### What is the actual behavior?

I am trying to build version 14 through cargo, but it fails with the error message 

    error: linking with \`link.exe\` failed: exit code: 1327 

followed by a very long collection of different paths, ending with 

     note:
          C:\Users\$USERNAME\.cargo\registry\src\index.crates.io-$HASH\ripgrep-14.0.0\pkg/windows/Manifest.xml : general error c1010070: Failed to load and parse the manifest. Det går inte att hitta sökvägen.
          LINK : fatal error LNK1327: failure during running mt.exe

The text "Det går inte att hitta sökvägen." is Swedish for "The path could not be found" (expected since I'm in Sweden).

Other rust programs build from source as expected.

### What is the expected behavior?

ripgrep should have built and installed succeessfully.

---

_Comment by @JSorngard on 2023-11-26 21:20_

I have also tried using nightly rust as well as all combinations of offered features.

---

_Closed by @BurntSushi on 2023-11-26 21:33_

---

_Label `bug` added by @BurntSushi on 2023-11-26 22:38_

---
