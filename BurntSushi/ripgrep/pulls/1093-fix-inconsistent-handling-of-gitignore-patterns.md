```yaml
number: 1093
title: "Fix inconsistent handling of gitignore patterns like foo*bar"
type: pull_request
state: merged
author: okdana
labels: []
assignees: []
merged: true
base: master
head: dana/literal-separator
created_at: 2018-10-28T06:45:10Z
updated_at: 2019-09-05T12:46:03Z
url: https://github.com/BurntSushi/ripgrep/pull/1093
synced_at: 2026-01-12T18:23:13Z
```

# Fix inconsistent handling of gitignore patterns like foo*bar

---

_@okdana_

(This is kind of related to #762, but not exactly. The issue was present before that was merged. I don't think anyone's ever complained, though, because there's no test for it.)

In the `ignore` crate, there's a check for a literal `/` in `gitignore` patterns. If there is one, `literal_separator` is set to `true`, preventing `*` from matching slashes; if there's not, `*` is allowed to match slashes.

Whether the `*` matches slashes doesn't really matter either way for patterns like `*foo` and `foo*`, but it's pretty important for `foo*bar`. In that case (without `literal_separator`), the `gitignore` matcher will return a successful match for both `foobazbar` *and* `foo/baz/bar`.

Git itself does not seem to treat `gitignore` patterns like this (tested 2.17.1 and 2.19.1).

You can run the following in the root of the ripgrep repo to demonstrate this:

```
% \rg --no-ignore-global --files -g 's*.rs' | sort
globset/src/glob.rs
globset/src/lib.rs
globset/src/pathutil.rs
grep-cli/src/decompress.rs
grep-cli/src/escape.rs
... # and so on
% \git ls-files --ignored -x 's*.rs' | sort
grep-printer/src/standard.rs
grep-printer/src/stats.rs
grep-printer/src/summary.rs
grep-regex/src/strip.rs
grep-searcher/examples/search-stdin.rs
grep-searcher/src/sink.rs
grep/examples/simplegrep.rs
src/search.rs
src/subject.rs
```

The `rg` command lists almost all of the Rust source files in the repo, because `s*.rs` without `literal_separator` matches `src/whatever.rs`. The `git` command lists only Rust source files that actually start with an `s`.

With this change, both commands produce the same output.

---

I think `ignore`'s current behaviour is the result of attempting to account for the apparent distinction being made by these two bullet points in the `gitignore` documentation:

>If the pattern does not contain a slash /, Git treats it as a shell glob pattern and checks for a match against the pathname relative to the location of the .gitignore file (relative to the toplevel of the work tree if not from a .gitignore file).
>
>Otherwise, Git treats the pattern as a shell glob: "*" matches anything except "/", "?" matches any one character except "/" and "[]" matches one character in a selected range. See fnmatch(3) and the FNM_PATHNAME flag for a more detailed description.

Someone obviously felt the need to split these two scenarios out here, suggesting there's a difference, but whatever that might be isn't really communicated at all.

The first point kind of suggests, with the 'match against the pathname' bit, that it should be matched against the full relative path, but that's actually the *opposite* of how it behaves, which is evidenced in the `ignore` crate by the way it (correctly) treats patterns without a slash as having `**/` prepended to them.

Both points also use the phrase 'treats ... as a shell glob', and the second one clarifies that, in a shell glob, `*` doesn't match `/`.

So, despite their weird attempt at a distinction, i'm not sure that the existing behaviour is actually supported by the documentation. Or at least the alternative isn't *not* supported...?

Does that make sense, or is this another case where you'd like to see the up-stream documentation made more explicit before changing things? (I should probably send them another e-mail about this either way, but i haven't yet.)

---

_Comment by @okdana on 2018-10-28 10:08_

I also found that the pattern `**` is not handled correctly by `ignore`. It turns it into `**/**/*`, which `globset` transforms into `(?-u)^(?:/|/.*/).*$`, which doesn't match anything at all unless you give an absolute path on the command line.

That's an even easier fix, but it touches the same code and i don't think you can make a PR based on another one, so i'll hold off until i know how you feel about this

Edit: Well, it's easy to fix the problem in `ignore`. But actually i think the fact that `globset` doesn't handle `**/**/*` properly is a *third* bug — Git seems to treat it the same as `**/*`. Maybe the fix for that one should be part of the changes required to support `**` fall-back to `*`, assuming the change being discussed on the Git mailing list goes through

---

_@BurntSushi approved on 2019-01-23 22:53_

Excellent, thank you. Sorry it took me so long to get to this, but I wanted to carefully read your argument and change, and I think I buy it! Your diagnosis of why the code was the way it was because of how `man gitignore` was written is exactly right I believe. I definitely inferred a distinction there where in turns out that in practice there isn't one!

---

_Merged by @BurntSushi on 2019-01-23 22:54_

---

_Closed by @BurntSushi on 2019-01-23 22:54_

---

_Comment by @BurntSushi on 2019-09-05 12:46_

FWIW, as of at least `2.23.0`, `man gitignore` has much clearer language now than it did. (And re-affirming that this fix is indeed correct!)

---
