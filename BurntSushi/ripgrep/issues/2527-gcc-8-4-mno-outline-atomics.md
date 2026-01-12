```yaml
number: 2527
title: "Gcc 8.4 ```-mno-outline-atomics```"
type: issue
state: closed
author: predators46
labels: []
assignees: []
created_at: 2023-06-05T23:07:36Z
updated_at: 2023-06-05T23:36:39Z
url: https://github.com/BurntSushi/ripgrep/issues/2527
synced_at: 2026-01-12T16:13:24Z
```

# Gcc 8.4 ```-mno-outline-atomics```

---

_@predators46_

Openwrt
Gcc 8.4
Rust 1.70.0

i found an error while compiling in openwrt using gcc 8.4


```cargo:warning=aarch64-openwrt-linux-musl-gcc: error: unrecognized command line option '-mno-outline-atomics'; did you mean '-fno-inline-atomics'?```

---

_Renamed from "Gcc 8.4 -mno-outline-atomics" to "Gcc 8.4 ```-mno-outline-atomics```" by @predators46 on 2023-06-05 23:09_

---

_Comment by @BurntSushi on 2023-06-05 23:13_

Dunno. This isn't really a question I can answer. A C compiler shouldn't be needed at all for the default build. I believe it's only needed if you include PCRE2. Which means this is probably more of a PCRE2 question? But I'm just guessing. This is probably something you'll have to dig into on your own.

It might help others help you if you provide a way to reproduce your error. For example, you haven'y provided any build commands that you're using.

---

_Comment by @predators46 on 2023-06-05 23:20_

https://github.com/openwrt/packages/blob/master/utils/ripgrep/Makefile

---

_Comment by @predators46 on 2023-06-05 23:22_

if i use gcc 11 i can make it work. i want to use gcc 8

---

_Comment by @BurntSushi on 2023-06-05 23:28_

Try without PCRE2. Otherwise it is a non-goal of this project to support arbitrarily old compiler toolchains. Sorry.

---

_Closed by @BurntSushi on 2023-06-05 23:28_

---

_Comment by @predators46 on 2023-06-05 23:29_

Thanks

---

_Closed by @predators46 on 2023-06-05 23:29_

---

_Closed by @BurntSushi on 2023-06-05 23:36_

---
