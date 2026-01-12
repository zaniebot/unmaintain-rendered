```yaml
number: 1805
title: Compile under WASI
type: pull_request
state: closed
author: jcaesar
labels: []
assignees: []
base: master
head: master
created_at: 2021-02-16T15:50:06Z
updated_at: 2021-05-25T02:39:02Z
url: https://github.com/BurntSushi/ripgrep/pull/1805
synced_at: 2026-01-12T18:23:14Z
```

# Compile under WASI

---

_@jcaesar_

Hi, and thank you for this awesome tool.

I'm currently playing around with porting some [Rust Alternative Command Line Tools](https://zaiste.net/posts/shell-commands-rust/) to wasm32-wasi. For most tools, this requires some rather unsavoury code changes, but rg is a pretty exception. So I decided to see if you don't want to add support for wasi as a compilation target.

I did say "pretty", but there are some points…
* memmap2 does not want to compile under anything except windows and unix ([conscious decision](https://github.com/RazrFalcon/memmap2-rs/pull/10#issuecomment-778788586)), so it requires some macros
* I did default `is_readable_stdin` to false on unsupported platforms (because same_file returns errors on unsupported platforms, and imp translates errors to false), but I'm not sure that is correct. Would it be better to panic, or leave the function unimplemented on unsupported platforms?
* PCRE2 won't build since it requires(?) pthreads
* Cross can't run tests on wasm32-wasi

---

_Comment by @BurntSushi on 2021-02-18 12:03_

Thanks for the PR. Could you say why you want to do this?

---

_Comment by @jcaesar on 2021-02-21 14:11_

Why **I** want to do this? I suppose I'm intrigued by the idea of system-independent binaries.

But if you're asking what the benefits of merging this are… admittedly rather thin. To run wasm/wasi binaries, you do need a runtime like wasmtime, wasmer, or lucet, and those aren't (yet?) easily available through distros or installed on your typical windows machine. So for now, this would be a rather niche way of using ripgrep.

---

_Comment by @BurntSushi on 2021-02-21 14:25_

All righty. I would like to hold off then until there is a more concrete use case for running ripgrep in this way.

---

_Closed by @BurntSushi on 2021-02-21 14:25_

---

_Comment by @jcaesar on 2021-05-25 02:39_

(It seems that mmap2 will now (v0.2.3) compile under WASI. In case this gets reconsidered, the only remaining necessary change is to `crates/cli/src/lib.rs `.)

---
