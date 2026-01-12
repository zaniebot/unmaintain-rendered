```yaml
number: 5942
title: "feat(builder): Make `clap::Error` be `Clone`"
type: pull_request
state: closed
author: ilyagr
labels: []
assignees: []
base: master
head: error-clone
created_at: 2025-03-08T05:17:36Z
updated_at: 2025-03-10T15:58:59Z
url: https://github.com/clap-rs/clap/pull/5942
synced_at: 2026-01-12T16:14:17Z
```

# feat(builder): Make `clap::Error` be `Clone`

---

_@ilyagr_

#5935

As he mentioned in https://github.com/clap-rs/clap/issues/5935#issuecomment-2707958021, @yuja just refactored the `jj` code so that it no longer requires this, quite easily in retrospect (https://github.com/jj-vcs/jj/pull/5929).

Still, I will list a few reasons to do this, since I still think it's worth doing. All of them amount to a "nice to have", not anything crucial. So, if this idea does not sit well with you, no pressure. 

---------------

Some reasons to make `clap::Error` be `Clone`:

- As you (@epage) suggested, `clap::Error`s could be cached
- `Clone` types are convenient for new Rust users who might not be experienced in solving ownership problems
- Since `clap::Error`s can be edited, one might want to edit them in a fallible way. It should be possible to save a backup copy in case the edit fails.
- More generally, all else being equal, it's nice to not require people to refactor their programs. In this case, one could argue that the refactor would improve their programs, but the improvement of avoiding a clone in the error path would be quite minor and might not be worth their time.
- There's little to no cost to doing so 

---
