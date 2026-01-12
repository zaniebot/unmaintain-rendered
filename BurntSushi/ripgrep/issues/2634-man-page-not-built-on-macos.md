```yaml
number: 2634
title: man page not built on macOS
type: issue
state: closed
author: mario-grgic
labels: []
assignees: []
created_at: 2023-10-16T00:47:49Z
updated_at: 2023-10-16T01:21:04Z
url: https://github.com/BurntSushi/ripgrep/issues/2634
synced_at: 2026-01-12T16:13:24Z
```

# man page not built on macOS

---

_@mario-grgic_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 13.0.0 (rev dd07b899ff)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

### How did you install ripgrep?

Building ripgrep from git source:

```sh
export PCRE2_SYS_STATIC=1
cargo build --release --features 'pcre2'
```


### What operating system are you using ripgrep on?

macos 13.6

### Describe your bug.

I have asciidoctor, asciidoc and a2x installed, yet man page is not built. 

I can manually build the man page by first editing the template and manually replacing the `{VERSION}` string and then running

`asciidoctor --doctype manpage --backend=manpage rg.1.txt.tpl`

However, not sure why cargo build fails to do it.

### What are the steps to reproduce the behavior?

run cargo build on mac.

### What is the actual behavior?

no man page built.

### What is the expected behavior?

It would be nice to generate man page on macos.

---

_Comment by @mario-grgic on 2023-10-16 01:21_

Never mind. The man page is built, but I'm using the wrong tool to find it :D.

---

_Closed by @mario-grgic on 2023-10-16 01:21_

---
