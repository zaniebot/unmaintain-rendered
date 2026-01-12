```yaml
number: 3258
title: "Globset: A simple way to check whether there are any wildcards"
type: issue
state: open
author: tgross35
labels: []
assignees: []
created_at: 2026-01-07T00:51:51Z
updated_at: 2026-01-07T19:43:42Z
url: https://github.com/BurntSushi/ripgrep/issues/3258
synced_at: 2026-01-12T16:13:25Z
```

# Globset: A simple way to check whether there are any wildcards

---

_@tgross35_

We had a use recently where it would be nice to check whether or not there are any wildcards in a string, to fall back to other matching algorithms if not. Would it be possible to add something like the following?

```rust
impl Glob {
    fn contains_wildcard(&self) -> bool;
}
```

Could also be a standalone function that takes an `&str`.

This can kind of be done by checking the output of `escape`, but that is suboptimal.

---

_Comment by @BurntSushi on 2026-01-07 14:18_

Can you say what the use case is? `globset` should already do literal matching optimizations.

Note that checking the output of `escape` could be misleading. e.g., `f[[]oo` matches `f[oo` but `globset::escape` turns it into `f[[][[][]]oo`. Would you consider `f[[]oo` as containing a wildcard? Or even `f[o]o`?

It's possible that just checking whether the glob contains a special character (`?`, `*`, `[`, `]`, `{`, `}`) or not via a simple substring test is sufficient. But it depends on the use case.

---

_Comment by @tgross35 on 2026-01-07 19:43_

> Can you say what the use case is? `globset` should already do literal matching optimizations.

This came up in triagebot, which used prefix matching but wanted to add glob support without breaking things. The case is kind of resolved currently by adding `*` to the end of the glob, but that means that e.g. `**/foo.rs` will also match `**/foo.rs.stderr`. Probably not a problem for triagebot, but that workaround might not be the desired behavior elsewhere.

> Note that checking the output of `escape` could be misleading. e.g., `f[[]oo` matches `f[oo` but `globset::escape` turns it into `f[[][[][]]oo`. Would you consider `f[[]oo` as containing a wildcard? Or even `f[o]o`?

I just meant checking whether the escaped string is different from the input string, as an approximation.

> It's possible that just checking whether the glob contains a special character (`?`, `*`, `[`, `]`, `{`, `}`) or not via a simple substring test is sufficient. But it depends on the use case.

Right, this would work fine. It would just be a convenience for globset to own the set of wildcard characters to look for (though I don't expect there to ever be any more), or for this to be a method on `Glob` that also takes settings into account (e.g. `\`).

---
