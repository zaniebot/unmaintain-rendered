```yaml
number: 10086
title: Use dynamic dispatch to simplify reporters
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/reporter-dyn
created_at: 2024-12-21T23:12:24Z
updated_at: 2025-01-06T17:04:04Z
url: https://github.com/astral-sh/uv/pull/10086
synced_at: 2026-01-10T11:44:33Z
```

# Use dynamic dispatch to simplify reporters

---

_Pull request opened by @charliermarsh on 2024-12-21 23:12_

## Summary

Sort of undecided on this. These are already stored as `dyn Reporter` in each struct, so we're already using dynamic dispatch in that sense. But all the methods take `impl Reporter`. This is sometimes nice (the callsites are simpler?), but it also means that in practice, you often _can't_ pass `None` to these methods that accept `Option<impl Reporter>`, because Rust can't infer the generic type.

Anyway, this adds more consistency and simplifies the setup by using `Arc<dyn Reporter>` everywhere.


---

_Label `internal` added by @charliermarsh on 2024-12-21 23:45_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-12-21 23:54_

---

_@charliermarsh reviewed on 2024-12-21 23:57_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/source/mod.rs`:1425 on 2024-12-21 23:57_

I'm really annoyed by this whole `Facade` setup... I want to be able to do something like:

```rust
impl <T: Reporter> uv_distribution::Reporter for Arc<dyn T> {
    fn on_build_start(&self, source: &BuildableSource) -> usize {
        self.on_build_start(source)
    }

    fn on_build_complete(&self, source: &BuildableSource, id: usize) {
        self.on_build_complete(source, id);
    }

    fn on_checkout_start(&self, url: &Url, rev: &str) -> usize {
        self.on_checkout_start(url, rev)
    }

    fn on_checkout_complete(&self, url: &Url, rev: &str, id: usize) {
        self.on_checkout_complete(url, rev, id);
    }
}
```

But I run into orphan trait problems.

The general problem here is that I have multiple `Reporter` traits. And if you implement one of them, then you should get some others "for free". But for now, I have to do this:

```rust
/// A facade for converting from [`Reporter`] to [`uv_git::Reporter`].
pub(crate) struct Facade {
    reporter: Arc<dyn Reporter>,
}

impl From<Arc<dyn Reporter>> for Facade {
    fn from(reporter: Arc<dyn Reporter>) -> Self {
        Self { reporter }
    }
}

impl uv_git::Reporter for Facade {
    fn on_checkout_start(&self, url: &Url, rev: &str) -> usize {
        self.reporter.on_checkout_start(url, rev)
    }

    fn on_checkout_complete(&self, url: &Url, rev: &str, id: usize) {
        self.reporter.on_checkout_complete(url, rev, id);
    }
}
```

---

_@BurntSushi approved on 2025-01-06 15:49_

I'm a believer in using dynamic dispatch for stuff like this. We don't need the monomorphization benefits, I think, here, so it probably helps compile times to use dynamic dispatch. (Whether this specific change moves the needle much is unlikely, but I think the philosophy applied more universally might.)

I think your approach here is fine-ish. I did do a force push to this PR that tweaks it a bit to make the call sites a fair bit less annoying (although still a little more annoying than what they were before in the `Some` case). It uses the same basic idea as your approach, but cleans it up a little with an `impl dyn Reporter { ... }` block. (One of the many tricks to get around the straight jacket of dyn safety.)

I think with more time there's probably a cleaner refactoring lurking here, but I didn't want to burn too much more time on it. My _instinct_ is to encapsulate the dynamic dispatch somehow so that we aren't leaking the `Arc<dyn Trait>` out everywhere.

---

_Merged by @charliermarsh on 2025-01-06 17:04_

---

_Closed by @charliermarsh on 2025-01-06 17:04_

---

_Comment by @charliermarsh on 2025-01-06 17:04_

Thanks!

---

_Branch deleted on 2025-01-06 17:04_

---
