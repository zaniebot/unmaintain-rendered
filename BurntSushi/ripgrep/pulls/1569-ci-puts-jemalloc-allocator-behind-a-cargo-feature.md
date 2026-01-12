```yaml
number: 1569
title: "ci: puts jemalloc allocator behind a cargo feature flag"
type: pull_request
state: closed
author: jonstites
labels: []
assignees: []
base: master
head: js/jemalloc
created_at: 2020-05-06T13:34:54Z
updated_at: 2021-09-07T00:05:44Z
url: https://github.com/BurntSushi/ripgrep/pull/1569
synced_at: 2026-01-12T18:23:14Z
```

# ci: puts jemalloc allocator behind a cargo feature flag

---

_@jonstites_

I have to say, thank you for making such incredibly nice, well-designed tools. I went through `ripgrep` and `xsv` a lot when I was first wrapping my head around Rust.

I saw an issue that I thought I could handle, so in an effort to be helpful, here it is.

This puts the jemalloc allocator behind a cargo feature as suggested here:
https://github.com/BurntSushi/ripgrep/issues/1542

The GitHub action for building releases appears to work:
https://github.com/jonstites/ripgrep/actions/runs/97156617

I did my best to make sensible choices, but I could be off the mark. Some thoughts:
- It might be confusing that there is a feature called "jemalloc" that doesn't actually use jemalloc unless on x86 musl. Cargo does not yet fully support platform-specific features. The feature could be called "jemalloc-musl-x86" or it could actually try to use "jemalloc" for any target. But jemallocator doesn't fully support all that many targets...
- This changes the default allocator for x86_64-unknown-linux-musl. This might lead to a big performance hit for someone. The feature could instead be for disabling jemalloc.
- I didn't touch the build-deb file, partly because I don't fully understand how .deb packaging works and partly because as best I can tell, jemalloc is disabled: https://salsa.debian.org/rust-team/debcargo-conf/-/blob/85f65b218cf86dbded7b66705617d3a5eb0b6a25/src/ripgrep/debian/patches/disable-jemallocator.diff

PS I am not someone who would take it personally if you requested big changes or decided that this issue isn't important enough for the added complexity. It's your project and your call on the code!

---

_Comment by @BurntSushi on 2020-05-09 02:32_

Hmmm... Thanks so much for putting in the work for this! Actually seeing it done kind of makes me think it's not worth doing. At minimum, I would probably either want to rename the `jemalloc` feature to `jemalloc-x86_64-musl` _or_ cause the `jemalloc` feature to enable jemalloc regardless of the platform (even if it isn't supported), like you suggested. But this does indeed lead to a case where building musl by default will now result in a slower ripgrep. I'm not quite sure how to mitigate that unfortunately. I think I would rather not have jemalloc configurable as a feature. :-( If disabling it ends up really being required, then I think just patching ripgrep is probably the best bet and is easy to do.

So thanks again for this, but I'm going to close this out for now.

---

_Closed by @BurntSushi on 2020-05-09 02:32_

---

_Comment by @hboetes on 2020-09-23 15:09_

I just applied this patch to my git repo and it works like a charm. Finally can build static binaries. Please apply to master.

---

_Comment by @jameshilliard on 2021-09-07 00:05_

> But this does indeed lead to a case where building musl by default will now result in a slower ripgrep.

I think you can just set it as optional but enabled by default like [this](https://doc.rust-lang.org/cargo/reference/features.html#the-default-feature) to fix that.

---
