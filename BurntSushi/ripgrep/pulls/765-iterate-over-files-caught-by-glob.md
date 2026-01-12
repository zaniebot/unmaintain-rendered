```yaml
number: 765
title: Iterate over files caught by Glob
type: pull_request
state: closed
author: Gilnaa
labels: []
assignees: []
base: master
head: globset_iter_at
created_at: 2018-01-29T21:25:06Z
updated_at: 2018-02-20T12:14:30Z
url: https://github.com/BurntSushi/ripgrep/pull/765
synced_at: 2026-01-12T18:23:13Z
```

# Iterate over files caught by Glob

---

_@Gilnaa_

Example at the tests section.

Question:
- Is this API acceptable?
- Should `iter_at` consume GlobSet
- Should this feature be behind a feature-gate (because of the added dependency)?

---

_Comment by @BurntSushi on 2018-01-29 21:35_

Thanks for this PR! Honestly, I'm not sure if this is right or not. I have a lot of concerns:

1. Is the implementation here actually correct? I think we need a written specification (that should turn into public API docs) that describes how this works. As one example, it looks like this will iterate over every path in the directory where as I think it should probably not descend into directories if there is no possibility for a match. This may be a hard optimization to implement, so I am in principle OK punting on it, but we need to be sure that the optimization won't change behavior.
2. I'm not sure what the public API should look like. We probably want to support both `Glob` and `GlobSet`s for this, in addition to a top-level convenience function.
3. The walker itself using `walkdir` seems fine, but `walkdir` exposes many options that permit configuring how walking works. For example, walkdir permits following symlinks. We probably want to re-expose the walkdir options via a custom builder *without* making `walkdir` a public dependency. (So that means writing a little boiler plate.)
4. I wonder whether this belongs in a separate crate, perhaps, `globset-walker`. I think I would rather that than having to maintain a feature. The possible roadblock to this is if the walker for some reason needs access to glob internals for the optimization I mentioned in (1). I'm not sure, but it needs to be thought through.

That's all I can think of at the moment. Thanks for taking a crack at this, but I think this probably needs a bit more work to be carried over the finish line.

---

_Comment by @killercup on 2018-01-29 22:53_

Thanks, @Gilnaa! For context: [I asked for this on Twitter](https://twitter.com/killercup/status/957691964694188033) :)

We talked about where to put this as well. Adding this to walkdir (e.g., `Walkdir::new_glob()`) is an option. As a new crate would directly compete with the glob crate (which, right now, is quite simple and does not have any dependencies), maybe another option is to add a "globset" feature there? (This would also give us the existing tests from that crate which I would otherwise suggest we copy.)

---

_Comment by @BurntSushi on 2018-01-29 23:10_

The `glob` crate currently has a lot of deficiencies. AFAIK, the only advantage that the `glob` crate has over `globset` is the existence of the directory walker and the fact that it doesn't depend on `regex`, so its compile times are better. If neither of those things are deal breakers to you, then I think I'd strongly recommend `globset` over `glob` at this point. As a library team member, if we could cover at least the directory walking use case, then I kind of feel like we could get a consensus to coalesce around `globset` and either deprecate `glob` or replace it.

With that said, I kind of feel like making `globset` a dependency of `glob` would be... a little strange. :-) I also think making `globset` a dependency of `walkdir` is a little weird too. But honestly... I don't feel that strongly. I think my preference is still for this to be a separate crate (perhaps in this repo) unless that isn't feasible for performance reasons.

But yeah... I definitely would like to have this feature (walker based on walkdir and globset), but I think it needs some deep thinking on the specification first. Alternatively, an enterprising individual could start their own crate with this and develop it on their own terms!

---

_Comment by @Gilnaa on 2018-02-03 07:01_

Alright :), I think I'll play with it further in a separate repo for a while

---

_Comment by @BurntSushi on 2018-02-20 12:14_

@Gilnaa Thanks for working on this! I'm going to close out this PR. Please reach out if you'd like any help or guidance!

---

_Closed by @BurntSushi on 2018-02-20 12:14_

---
