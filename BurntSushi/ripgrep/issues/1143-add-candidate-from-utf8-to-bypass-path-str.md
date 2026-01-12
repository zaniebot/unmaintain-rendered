```yaml
number: 1143
title: "Add `Candidate::from_utf8` to bypass `Path -> str` conversion?"
type: issue
state: closed
author: quark-zju
labels:
  - question
assignees: []
created_at: 2018-12-16T06:06:28Z
updated_at: 2019-01-24T14:58:52Z
url: https://github.com/BurntSushi/ripgrep/issues/1143
synced_at: 2026-01-12T16:13:23Z
```

# Add `Candidate::from_utf8` to bypass `Path -> str` conversion?

---

_@quark-zju_

In `globset`, patterns are `str` (as `Glob::new` takes a `str`), but paths to test are `Path` (as `GlobSet::is_match` takes a `Path`). It's a bit inconsistent.

There are 2 places where `Path` can be undesirable:
- Windows. Converting between `Path` and utf-8 `str` has a cost. In `globset`, that's `os_str_bytes`.
- Paths not generated from filesystem APIs. For example, source control software might store `path` in its internal form (ex. git tree objects or index), which might be utf-8 already. It can also be paths from a utf-8 `Makefile`, or some sort of source code.

It seems a `Candidate::from_utf8` API can be useful in this case.

If the idea sounds good, I can help draft a patch.

---

_Renamed from "globset: `Candidate::from_utf8` to bypass `Path -> str` conversion?" to "Add `Candidate::from_utf8` to bypass `Path -> str` conversion?" by @quark-zju on 2018-12-16 06:10_

---

_Comment by @BurntSushi on 2018-12-16 13:12_

> In `globset`, patterns are `str` (as `Glob::new` takes a `str`), but paths to test are `Path` (as `GlobSet::is_match` takes a `Path`). It's a bit inconsistent.

I don't agree. I don't think there is any inconsistency in a pattern having a different type than the thing it matches. The same is true for regexes themselves. Patterns are always `&str`, but they can match `&[u8]`.

> Windows. Converting between `Path` and utf-8 `str` has a cost. In `globset`, that's `os_str_bytes`.

Sorry, but I don't understand how your proposal resolves this. Could you please elaborate?

> Paths not generated from filesystem APIs. For example, source control software might store `path` in its internal form (ex. git tree objects or index), which might be utf-8 already. It can also be paths from a utf-8 `Makefile`, or some sort of source code.

Again, I don't understand how your proposal addresses this. If you have something that is known to be UTF-8, then you can convert it to a `&str` at zero cost using `unsafe`, at which point, you can build a `Path` at zero cost.

Overall, I don't think I understand what the intended semantics of this new `from_utf8` API should be, I don't understand what problem it's trying to solve and I don't understand how either of the two problems you've pointed out are addressed by your proposal. You might consider following up with a focus on the problem you're trying to solve and perhaps a couple of real examples.

---

_Label `question` added by @BurntSushi on 2018-12-16 13:13_

---

_Comment by @quark-zju on 2018-12-17 03:26_

> > In `globset`, patterns are `str` (as `Glob::new` takes a `str`), but paths to test are `Path` (as `GlobSet::is_match` takes a `Path`). It's a bit inconsistent.
> 
> I don't agree. I don't think there is any inconsistency in a pattern having a different type than the thing it matches. The same is true for regexes themselves. Patterns are always `&str`, but they can match `&[u8]`.

That's fair. I can understand this part.
 
> > Windows. Converting between `Path` and utf-8 `str` has a cost. In `globset`, that's `os_str_bytes`.
> 
> Sorry, but I don't understand how your proposal resolves this. Could you please elaborate?

For example, given a `str` that needs to be tested by `GlobSet::is_match`. It goes through `str -> Path -> str / bytes` internally. The `str -> Path` part is because the API only exposes `Path`. The `Path -> str` part is because `GlobSet::is_match` -> `Candidate::new` -> `pathutil::os_str_bytes`. [`os_str_bytes`](https://github.com/BurntSushi/ripgrep/blob/118b950085ff674c1c7142bc5e171dd2b4688934/globset/src/pathutil.rs#L95) has a cost on Windows.

If `Candidate` can be constructed from `str`, such cost would be removed. I made a mistake thinking that `Path` is `[u16]` on Windows. That's not true and the concern is relieved at least on Windows. It seems still nice to have a way to bypass the `str -> Path -> str` round-trip, though, since the stdlib does not specify the actual encoding used by `Path`.

> > Paths not generated from filesystem APIs. For example, source control software might store `path` in its internal form (ex. git tree objects or index), which might be utf-8 already. It can also be paths from a utf-8 `Makefile`, or some sort of source code.
> 
> Again, I don't understand how your proposal addresses this. If you have something that is known to be UTF-8, then you can convert it to a `&str` at zero cost using `unsafe`, at which point, you can build a `Path` at zero cost.

This was the main problem. I was under the (wrong) impression that `OsStr` (and `Path`) on Windows is some kind of `[u16]` so round-trip with `str` are expensive, partially because [some of the stdlib doc said so](https://github.com/rust-lang/rust/commit/a1e9c7fc2e4806fe72c84178bf1116f645d18c43#diff-01cdc7c78b710001e8b4ed10732d60ddL594). However, when I checked the actual source code, there is no `[u16]` and `OsStr` is [pretty much just utf-8](https://github.com/rust-lang/rust/blob/a8a2a887d0a65fff6c777f9bcd7b1c0bdfbbddc0/src/libstd/sys_common/wtf8.rs#L172).

> Overall, I don't think I understand what the intended semantics of this new `from_utf8` API should be, I don't understand what problem it's trying to solve and I don't understand how either of the two problems you've pointed out are addressed by your proposal. You might consider following up with a focus on the problem you're trying to solve and perhaps a couple of real examples.



---

_Comment by @BurntSushi on 2018-12-17 12:16_

I see. More succinctly, I think what you're saying is that, today, since the API requires a `&Path`, there _may_ be a cost associated with converting that `&Path` internally to something that can be matched by the glob rules, and you would like a way to avoid that cost. In particular, you have a specific use case where you are storing paths somewhere that are already valid UTF-8, and can therefore be matched directly and shouldn't need any platform specific code to handle them.

In principle, I'd be OK with an addition to the API that lets you circumvent this. Note, however, that the internal representation of a `Candidate` isn't just the path, but it also contains path components such as `basename` and `ext`, which are used for optimization purposes. See here: https://github.com/BurntSushi/ripgrep/blob/118b950085ff674c1c7142bc5e171dd2b4688934/globset/src/lib.rs#L495-L496

Personally, I would rather like to see a benchmark on a real workload that benefits from a change like this.

---

_Comment by @quark-zju on 2018-12-18 00:23_

Yep. I also noticed the overhead in `Candidate`. I think the `Path` conversion overhead is neglectable comparing to `file_name` overhead.

Ideally, `GlobSet::is_match` is nearly as fast as `BTreeMap` if there are only literal patterns. Currently, it's at least 3x slower.

----

Some more context. Not directly related to this issue.

I was wanting a `match_recursive(dir) -> None | Some(true) | Some(false)` interface, which returns `Some(true)` if all possible paths under `dir` will match; `Some(false)` if none can match; `None` if some paths can match. This can make certain tree walking logic fast, and the matcher can be negated without losing fast paths.

For example, for a single pattern `"a/b/**"`, `match_recursive("a")` returns `None`, `match_recursive("a/b/c")` returns `Some(true)`, and `match_recursive("b")` returns `Some(false)`.

If all patterns are just literal + `/**`, then things become simple. A tree struct like `struct TreeMatcher { subtrees: HashMap<String, TreeMatcher>, has_double_star: bool }` would answer questions easily.

Some patterns can have globs (ex. `a*/b*/**`). So it feels natural to just replace the above `HashMap` with `GlobSet`.

EDIT:

I guess another way to solve it is to just use a single `GlobSet`, then insert all parent patterns into the `GlobSet` to be able to tell "this directory" might have something that matches. To be able to get parent patterns, some logic to expand `{}` in glob patterns need to be written.

---

_Comment by @BurntSushi on 2019-01-24 14:58_

@quark-zju Could you please clarify whether this proposed new API would suit your use case?

```rust
    /// Create a new candidate for matching from the given path as a slice of
    /// arbitrary bytes.
    ///
    /// The purpose of using this method is that it may be able to circumvent
    /// some costly platform specific initialization steps.
    pub fn from_bytes<P: AsRef<[u8]> + ?Sized>(path: &'a P) -> Candidate<'a> {
        /// elided...
    }
```

If so, I'm not really convinced of this API. In particular, platform specific initialization steps are still required in order to handle path separators.

If you can:

1. Add this API.
2. Provide a convincing real world benchmark that shows a non-trivial improvement.

then I might be willing to reconsider this. Until then, I think this is just too complex and niche to justify. Yes, Windows requires a bit more processing here, but nothing is going to change that unless you can lobby Rust's standard library to expose an `OsStr`'s internal `WTF-8` representation, but thus far, they have remained ideologically opposed to such conveniences.

---

_Closed by @BurntSushi on 2019-01-24 14:58_

---
