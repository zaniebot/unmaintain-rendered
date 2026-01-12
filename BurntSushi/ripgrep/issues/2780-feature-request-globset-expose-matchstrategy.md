```yaml
number: 2780
title: "Feature Request (globset): Expose `MatchStrategy`"
type: issue
state: closed
author: sxyazi
labels: []
assignees: []
created_at: 2024-04-16T00:05:57Z
updated_at: 2024-04-16T11:33:24Z
url: https://github.com/BurntSushi/ripgrep/issues/2780
synced_at: 2026-01-12T16:13:24Z
```

# Feature Request (globset): Expose `MatchStrategy`

---

_@sxyazi_

Hey, I'm using globset to match a set of icon rules, which are mostly extension-based rules, like this:

```
*.jpg icon1
*.png icon2
*.gif icon3
```

They are a group of rules with similar structures, and I only need to get the first matching icon. So, I'm thinking of detecting the strategy of glob through `MatchStrategy::new()` and further encapsulating it.

Is it possible to make `MatchStrategy` public? If you think it makes sense, I can try to create a PR for it. Thanks for your work on globset!

---

_Comment by @BurntSushi on 2024-04-16 01:20_

No, it's an implementation detail. It's not totally clear to me why your use case needs it to be public.

---

_Comment by @sxyazi on 2024-04-16 01:40_

I want to know what kind of glob the user has set, such as whether it is just a simple extension glob. If it is, I can go through my own optimized processing logic without compiling a regular expression for it and performing regex matching, and only need to compare a small part of the path.

So I need to detect the type of glob the user has set and decide on a strategy, and I found that globset has implemented this very well just not make it public, of course I could refer to globset and re-implement it from scratch, but I think that's unnecessary workload.

---

_Comment by @sxyazi on 2024-04-16 01:57_

Add more details:

After reading the source code of `globset`, I found that only `GlobSet` uses this strategy optimization during matching, while single glob matching (`GlobMatcher`) does not (please correct me if I'm wrong). 

However, `GlobSet::matches()` matches all glob rules and then returns all indexes. For my needs (only needing to get the first matching icon), this is an unnecessary additional overhead because it could have ended early.

So what I need is a combination of strategy matching and early termination, which `globset` cannot provide since it uses `strats: Vec<GlobSetMatchStrategy>` to save each glob grouped according to the strategy, as the order between globs is no longer available, which is why it must calculate all the indexes and reorder them:

https://github.com/BurntSushi/ripgrep/blob/d922b7ac114c24d6800ae5f79d2967481f380c83/crates/globset/src/lib.rs#L406-L410

Therefore, I think if the `MatchStrategy` is made public, I can implement it myself.

---

_Comment by @BurntSushi on 2024-04-16 01:58_

globset already avoids compiling a regex in some cases. Perhaps more cases could be added. 

Sorry, but I don't want to expose internals like what you're requesting. I would be open to adding optimizations though.

---

_Closed by @BurntSushi on 2024-04-16 01:58_

---

_Comment by @sxyazi on 2024-04-16 02:09_

OK no problem. 

> globset already avoids compiling a regex in some cases. Perhaps more cases could be added.

I couldn't find the relevant code, could you give me some hints? I'll see if I can add it - I'm interested in optimizing extension globs like `*.jpg` in `GlobMatcher`.


---

_Comment by @BurntSushi on 2024-04-16 11:33_

I'm confused because you are literally asking about the very thing that does what you want. It even has an optimization path for extensions.

https://github.com/BurntSushi/ripgrep/blob/d922b7ac114c24d6800ae5f79d2967481f380c83/crates/globset/src/glob.rs#L155-L177

It does not appear to skip compiling the regex, though, it does not use it for matching. I'd be open to adjustments that skip the regex compilation where possible. I can't remember whether that will be hard or not though. It could be a big refactor. But maybe not.

---
