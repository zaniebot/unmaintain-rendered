---
number: 2351
title: "build failure with `--all-features`"
type: issue
state: closed
author: matthiaskrgr
labels:
  - C-bug
assignees: []
created_at: 2021-02-16T21:06:51Z
updated_at: 2021-02-18T20:39:43Z
url: https://github.com/clap-rs/clap/issues/2351
synced_at: 2026-01-07T13:12:19-06:00
---

# build failure with `--all-features`

---

_Issue opened by @matthiaskrgr on 2021-02-16 21:06_

repo is @ 2b45011c9bed331319837eceb2b593e84b5b9cfa

`cargo check --all-features`
````
    Checking linked-hash-map v0.5.4
    Checking regex-syntax v0.6.22
   Compiling memchr v2.3.4
    Checking thread_local v1.1.3
    Checking terminal_size v0.1.16
    Checking yaml-rust v0.4.5
   Compiling clap_derive v3.0.0-beta.2 (/home/matthias/vcs/github/clap/clap_derive)
    Checking aho-corasick v0.7.15
    Checking textwrap v0.13.2
    Checking regex v1.4.3
    Checking clap v3.0.0-beta.2 (/home/matthias/vcs/github/clap)
error[E0425]: cannot find value `o` in this scope
    --> src/parse/parser.rs:1352:80
     |
1352 |                     debug!("Parser::remove_overrides:iter:{:?} => {:?}", name, o);
     |                                                                                ^ not found in this scope

error[E0425]: cannot find value `o` in this scope
    --> src/parse/parser.rs:1367:31
     |
1367 |                         name, o
     |                               ^ not found in this scope

error: aborting due to 2 previous errors
````


---

_Label `T: bug` added by @matthiaskrgr on 2021-02-16 21:06_

---

_Comment by @pksunkara on 2021-02-16 21:09_

@ldm0 This is a bug with your recent change

---

_Added to milestone `3.0` by @pksunkara on 2021-02-16 21:09_

---

_Referenced in [clap-rs/clap#2352](../../clap-rs/clap/pulls/2352.md) on 2021-02-17 04:08_

---

_Referenced in [clap-rs/clap#2349](../../clap-rs/clap/pulls/2349.md) on 2021-02-17 08:35_

---

_Closed by @pksunkara on 2021-02-18 20:39_

---
