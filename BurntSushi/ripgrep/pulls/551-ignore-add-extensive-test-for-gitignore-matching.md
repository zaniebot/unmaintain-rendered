```yaml
number: 551
title: "[ignore] Add extensive test for gitignore matching"
type: pull_request
state: merged
author: behnam
labels: []
assignees: []
merged: true
base: master
head: ignore
created_at: 2017-07-11T07:59:20Z
updated_at: 2017-07-13T02:33:10Z
url: https://github.com/BurntSushi/ripgrep/pull/551
synced_at: 2026-01-12T18:23:13Z
```

# [ignore] Add extensive test for gitignore matching

---

_@behnam_

The test data (gitignore rules and expected result) is based on the test
repo at <https://github.com/behnam/gitignore-test>.

This reveals a bug in gitignore matching:

* Rules of form `<dir>/*` result in ignoring only first-level files, but
no deep files. This is not correct, as `<dir>/*` matches the first-level
directories under `<dir>`, resulting all to be ignored.

(UPDATE: Turns out the inline comments are not supported in gitignore, so I dropped the related tests.)

---

_Comment by @behnam on 2017-07-11 09:42_

I expanded the tests to 64 rules and 158 assertions, out of which 30 is failing (marked with `FIXME`).

Most of these are blockers for migrating Cargo's include/exclude rules to `ignore` crate. (https://github.com/rust-lang/cargo/pull/4270)

I'm not sure if I'll find the time this week to dig deep into `ignore` crate to fix these, so please feel free to pick up the rest of the work here.

---

_Comment by @BurntSushi on 2017-07-11 11:14_

> Rules of form `<dir>/*` result in ignoring only first-level files, but
no deep files. This is not correct, as `<dir>/*` matches the first-level
directories under `<dir>`, resulting all to be ignored.

This is an interesting case. The bug doesn't manifest in ripgrep because the ignore rules are applied iteratively at each directory level. That is, if `dir` is ignored, then `dir/foo` is also ignored because `dir/foo` is never actually visited.

More generally, it's not actually clear what the right behavior is here. The fundamental issue is that ignoring files is based on globbing, and `dir/*` simply does not match `dir/foo/bar` because `*` does not match path separators when a literal path separator is present in the glob. (If you read `man gitignore` carefully, you'll note that `git` defines its semantics in terms of `fnmatch`. In particular, if the glob contains a literal `/`, then `fnmatch` with `FNM_PATHNAME` is set.)

I think there's probably an impedance mismatch here. The implementation for resolving ignore rules fundamentally works via glob matching, but by doing this, it sacrifices correctness for some inputs because its primary use case doesn't need correctness for those inputs. A trivial resolution to this would be to match each ancestor of a path against the ignore rules, and if any one is ignored, then all children are subsequently ignored. This cannot be done by default because performance would suffer considerably.

---

_Comment by @BurntSushi on 2017-07-11 11:15_

To drive this point home, if you use `ignore`'s directory walker, then all of these ignore rules should be resolved correctly.

---

_Comment by @behnam on 2017-07-11 18:36_

Thanks @BurntSushi for looking into this. Right, now that I think about the API as how the walker calls into it, it all makes sense.

This problem of matching globs with a path AND its parents is actually how I got here. I initially submitted a patch to Cargo to apply the include/exclude patterns (the current glob-based rules) to both a file and its parents (https://github.com/rust-lang/cargo/pull/4256), then we realized that we may be better off to not get into the details of matching in Cargo and use a library that supports matching a file path with a set of rules. (And, in this transition, make it better by also switching to gitignore rules instead of bare globs.)

That said, I really like to add API to allow matching one path (usually file, but also could be a directory) with a gitignore ruleset. So that's what I did in the new commit here, which also allows all the tests to pass.

I think it's a good thing to have this method here, because it's a common use case (when not using the built-in walker) and because we can use the helper methods here and the logic is pretty straightforward: start from the path and walk to the parents, return the first non-`Match::None` result, or `Match::None` otherwise.

What do you think?

---

_Comment by @behnam on 2017-07-11 18:37_

By the way, the implementation is broken for `rustc 1.12.0` and I'm working on a fix for it. Everything else should be fine.

**UPDATE**: It now works in `rustc 1.12.0`, as well.

---

_Review comment by @BurntSushi on `ignore/src/gitignore.rs`:196 on 2017-07-12 10:47_

After the initial description, I think we should briefly explain what we mean by `parent`. That is, it is a transformation on the path itself and doesn't try to walk up the file system. I'm not sure how to say that succinctly and clearly though.

---

_Review comment by @BurntSushi on `ignore/src/gitignore.rs`:209 on 2017-07-12 10:49_

Can we think of another name? I feel like `matched_recursive`  doesn't really signal to the user what this is actually doing. But this is a super subtle method, so I'm not sure what to call it either. Maybe `matched_any_parent` or something?

---

_@BurntSushi reviewed on 2017-07-12 10:53_

I agree we should add this method. I left a few comments mostly around the docs/naming. The implementation looks OK to me.

My only remaining concern is that this might produce matches when there shouldn't be a match (i.e., a false positive). I can't actually think of a specific test case for it though, which maybe means we're OK. I think the fact that we assume the given path is relative to the directory containing the corresponding `.gitignore` file is what allows this to work. It's a very subtle point though.

---

_@behnam reviewed on 2017-07-12 19:17_

---

_Review comment by @behnam on `ignore/src/gitignore.rs`:196 on 2017-07-12 19:17_

I know what you mean. Added some more text here. Please see how that look?

---

_@behnam reviewed on 2017-07-12 19:18_

---

_Review comment by @behnam on `ignore/src/gitignore.rs`:209 on 2017-07-12 19:18_

Right. Renamed to `matched_path_or_any_parents()`. What do you think?

---

_Comment by @behnam on 2017-07-12 19:19_

Thanks, @BurntSushi.

Addressed the comments in the last commit. Also, updated the fn implementation to be more flat and easier to read.

---

_Comment by @behnam on 2017-07-12 19:23_

And, I have added more text about the path being expected to be under the root, and put a `debug_assert!()` for it.

I think `gitignore` has undefined behavior otherwise, as it has an early assumption that `root == dirname(gitignore_path)` and only walks down from there.

So, I think it's a fair assumption for this API, and we can always extend it later, if we need to.

What do you think?

---

_Comment by @behnam on 2017-07-12 22:45_

Looks like the appveyor test is failing because of different path handling on windows systems. I'm looking into that part.

---

_Comment by @BurntSushi on 2017-07-12 23:06_

This looks great. I think it's good to go once you figure out Windows. Thank you! If you get stuck let me know and I can take a look (but it might take me a while to get to it).

---

_Comment by @behnam on 2017-07-13 01:58_

Using `!path.has_root()` instead of `path.is_relative()` solved the problem for windows and now all the tests are passing.

I also made the walking loop even simpler. Please take another look, if you can: https://github.com/BurntSushi/ripgrep/pull/551/files#diff-6d0dbe450a74521a529608d3e56606b7R223

And, we're going to need a crate release to use this in Cargo. Want me to add a commit to bump the version?

---

_Comment by @BurntSushi on 2017-07-13 02:03_

@behnam Great, thank you! LGTM.

No need to add a version. My scripts do that! :)

---

_Merged by @BurntSushi on 2017-07-13 02:06_

---

_Closed by @BurntSushi on 2017-07-13 02:06_

---

_Branch deleted on 2017-07-13 02:06_

---

_Comment by @BurntSushi on 2017-07-13 02:09_

`ignore 0.2.1` should now be on crates.io!

---

_Comment by @behnam on 2017-07-13 02:33_

Thanks for the quick help here, @BurntSushi!

---
