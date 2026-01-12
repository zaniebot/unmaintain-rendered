```yaml
number: 1615
title: Add nightly flag to SIMD cargo build command.
type: pull_request
state: closed
author: catleeball
labels: []
assignees: []
base: master
head: nightly-flag
created_at: 2020-06-11T23:34:27Z
updated_at: 2020-07-02T18:34:11Z
url: https://github.com/BurntSushi/ripgrep/pull/1615
synced_at: 2026-01-12T18:23:14Z
```

# Add nightly flag to SIMD cargo build command.

---

_@catleeball_

A tiny doc update that adds the `+nightly` flag to the example command to build ripgrep with simd extensions. Thought it might be nice in case folks have the stable rustc set to default and forget to add the flag.

---

_Comment by @BurntSushi on 2020-06-11 23:40_

I'm not sure I want to do this. I think it assumes you're using rustup. Probably not a terrible assumption, but probably not universally true either. The README does mention that night is required.

To be honest, I would just assume remove this section entirely. The additional optimizations gained by this are incredibly niche.

---

_Comment by @catleeball on 2020-06-11 23:45_

@BurntSushi That's a valid point! Instead of completely removing the section, would it make sense to instead move it to FAQ.md possibly? That way the info is still there without having to discover it in the source.

---

_Comment by @catleeball on 2020-07-02 18:34_

I'll close this PR, let me know if you'd like me to move the docs around any though. Thanks for making a lot of cool rust crates! :)  FYI, I've been using ripgrep at work for directories with bunches of >100mb configs and logs and it's been phenomenally faster than grep. 👍 

---

_Closed by @catleeball on 2020-07-02 18:34_

---

_Branch deleted on 2020-07-02 18:34_

---
