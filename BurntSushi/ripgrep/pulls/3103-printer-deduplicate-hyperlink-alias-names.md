```yaml
number: 3103
title: "printer: deduplicate hyperlink alias names"
type: pull_request
state: closed
author: ltrzesniewski
labels:
  - rollup
assignees: []
base: master
head: hyperlink-aliases
created_at: 2025-07-16T20:14:12Z
updated_at: 2025-09-20T11:40:21Z
url: https://github.com/BurntSushi/ripgrep/pull/3103
synced_at: 2026-01-12T18:23:15Z
```

# printer: deduplicate hyperlink alias names

---

_@ltrzesniewski_

This refactors the way hyperlink aliases are handled by embedding the code in a `HyperlinkAlias` type.

Currently, this removes a duplicate alias list from the `HyperlinkFormat` flag in `defs.rs`, but it should also provide the required features to avoid adding additional duplicates in the following PRs:

- #3096
- #3102 

/cc @ilyagr @okdana 



---

_Comment by @ltrzesniewski on 2025-07-16 20:24_

Well, apparently this would require bumping the MSRV to 1.80.

Not sure if that's ok or if I should try to find another way around this. I have another PR (#2865) which needs a bump to 1.79, so I admit a bump would be convenient 😅.

---

_Label `rollup` added by @BurntSushi on 2025-09-06 13:20_

---

_Comment by @BurntSushi on 2025-09-06 13:23_

Thank you for adding this! I ended up re-working this a bit to make the API a little less convenient, but in exchange, it should hopefully be easier to evolve in semver compatible ways if necessary. That is, the revised API doesn't encode as many assumptions.

I was somewhat on the fence about accepting this. I'm not really a huge fan of this much ceremony just to avoid a little duplication. In particular, there are other ways to ensure things don't get out of sync, i.e., a CI test or something. But I think overall this is a pretty small and reasonable change, and it might be useful to consumers of `grep-printer` to be able to see the list of supported aliases in a programmatic fashion.

---

_Comment by @ltrzesniewski on 2025-09-07 22:00_

Thanks for accepting this and using it in the other PRs! I thought the `grep-printer` crate was internal to ripgrep, so its API could be flexible. I'll keep this in mind.

Just one nitpick on `display_priority`: I think I'd rather use a signed type, so some aliases could eventually be forced to be at the _end_ of the list. Maybe `Option` wouldn't be needed in that case, as the default priority could be 0.


---

_Comment by @BurntSushi on 2025-09-07 22:33_

Thanks for checking it out! A signed type is probably wiser. But users of the crate can't rely on any specific priority that is given for an alias and there is no way for users to create a new alias or assign a priority. So its meaning is entirely under the control of the implementation.

But yeah, I am very grumpy about ripgrep's crates being on crates.io. I kind of regret doing it because it invites so much ceremony. But there are a healthy number of dependents of the `grep` crate (which is just a facade for crates like `grep-printer`). Because it's all on crates.io, a semver boundary really does have to be respected unfortunately.

I am more lenient about breaking changes than I usually am for more fundamental "ecosystem" crates. But I do still try to avoid them when it isn't too much of a pain otherwise. I'm just used to it after years of doing it with other crates.

---

_Comment by @BurntSushi on 2025-09-19 02:31_

I changed the type of priority from `Option<u16>` to `Option<i16>`.

The `Option` could probably be dropped, but I left it there. I think either option is workable.

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---

_Branch deleted on 2025-09-20 11:40_

---
