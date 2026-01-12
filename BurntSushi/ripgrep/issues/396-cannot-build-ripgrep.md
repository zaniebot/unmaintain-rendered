```yaml
number: 396
title: Cannot build ripgrep
type: issue
state: closed
author: ruipacheco
labels: []
assignees: []
created_at: 2017-03-08T00:06:35Z
updated_at: 2017-03-08T15:19:00Z
url: https://github.com/BurntSushi/ripgrep/issues/396
synced_at: 2026-01-12T16:13:21Z
```

# Cannot build ripgrep

---

_@ruipacheco_

```
$ cargo install ripgrep --verbose
    Updating registry `https://github.com/rust-lang/crates.io-index`
 Downloading ripgrep v0.4.0
error: [60] Peer certificate cannot be authenticated with given CA certificates (SSL certificate problem: self signed certificate in certificate chain)
```

Latest OSX with:

```
$ rustc --version
rustc 1.15.1
```

```
$ cargo --version
cargo 0.13.0 (eca9e15 2016-11-01)
```


---

_Comment by @BurntSushi on 2017-03-08 03:13_

Sorry, but this has nothing to do with ripgrep. I don't actually know what the problem is. Clearly there is some problem talking to crates.io.

Work around:

```
$ git clone git://github.com/BurntSushi/ripgrep
$ cd ripgrep
$ git checkout 0.4.0
$ cargo build --release
$ ./target/release/rg --version
```

---

_Closed by @BurntSushi on 2017-03-08 03:13_

---

_Comment by @ruipacheco on 2017-03-08 08:39_

$ cargo build --release
 Downloading clap v2.19.3
error: unable to get packages from source

To learn more, run the command again with --verbose.


---

_Comment by @BurntSushi on 2017-03-08 10:37_

Sorry, but i don't know how to debug your issue. I don't know why you can't contact crates.io. :-( cc @alexcrichton

---

_Comment by @BurntSushi on 2017-03-08 10:38_

Could you please run with the --verbose flag to get more details?

---

_Comment by @alexcrichton on 2017-03-08 15:19_

@ruipacheco are you behind a firewall or proxy of some kind? If so it may be causing problems. Do you have to configure other software for how it talks to the network?

---
