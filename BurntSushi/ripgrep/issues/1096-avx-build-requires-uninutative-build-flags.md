```yaml
number: 1096
title: avx build requires uninutative build flags
type: issue
state: closed
author: valarauca
labels:
  - question
assignees: []
created_at: 2018-10-30T18:23:27Z
updated_at: 2018-10-31T11:05:03Z
url: https://github.com/BurntSushi/ripgrep/issues/1096
synced_at: 2026-01-12T16:13:22Z
```

# avx build requires uninutative build flags

---

_@valarauca_

### What rustc version are you using?

```
rustc 1.31.0-nightly (d586d5d2f 2018-10-29)
```

### What cargo version are you using?

```
cargo 1.31.0-nightly (2d0863f65 2018-10-20)
```

### What operating system are you trying to build on

```
fedora 28
4.18.16-200.fc28.x86_64
```

#### What command did you run?

```shell
cargo build --features "avx-accel" --release
```

Other attempts include

```shell
cargo build --features "avx-accel simd-accel" --release
```

### What finally works is

```shell
RUSTFLAGS=-Ctarget-cpu=native cargo build --features "avx-accel" --release
```

which is rather extremely unintuitive, is there a work around to this?

---

_Renamed from "avx build doesn't work" to "avx build requires uninutative build flags" by @valarauca on 2018-10-30 18:23_

---

_Comment by @BurntSushi on 2018-10-30 20:10_

This is documented in the README: https://github.com/BurntSushi/ripgrep/blob/master/README.md#building

I don't know what you mean by "extremely unintuitive," but I don't know of any other way. Hopefully the dependencies that require compile time target features for optimizations will eventually move to runtime target detection.

---

_Closed by @BurntSushi on 2018-10-30 20:10_

---

_Label `question` added by @BurntSushi on 2018-10-31 11:05_

---
