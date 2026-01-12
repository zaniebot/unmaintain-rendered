```yaml
number: 3049
title: "Add `can_skip_directory` to `globset`"
type: issue
state: closed
author: lukesandberg
labels: []
assignees: []
created_at: 2025-05-13T16:49:11Z
updated_at: 2025-05-16T01:53:50Z
url: https://github.com/BurntSushi/ripgrep/issues/3049
synced_at: 2026-01-12T16:13:25Z
```

# Add `can_skip_directory` to `globset`

---

_@lukesandberg_

#### Describe your feature request

When walking a directory tree to find files to match a glob, it would be useful to know that we do not need to recurse on a given directory.

```
project
   src
        foo.js
   node_modules
         ..infinite files...
```

so if someone specifies `src/**/*.js` we know no file under `node_modules` will ever match.

similarly for slightly more complex patterns like `{src,lib,test}/**/*.js`

`globset` is super fast of course, but the issue here is avoiding the filesystem operations of traversing the subdirectories, and in my specific usecase we are also setting up file watchers for directories that match the pattern.   Thus it would be very useful to have a 'negative prefix match' like `can_skip_directory` where if i pass it `node_modules/` it would return `true` for either of the above patterns.

For simple rooted patterns this is easy to land, but of course we could be dealing with other kinds of patterns:

`*.js` wouldn't match any file in a subdirectory so `can_skip_directory` should always return true for a pattern like that.

`*/*/*.js` wouldn't match anything 3 or more levels deep.


### Implementation

This is actually tricky.  
For the special cased 'literal' implementations in globset, this is straightforward (e.g. a `MatchStrategy::Suffix` would always return `false` from `can_skip_directory`).  However, the real implementation issue is the regex engine.

For the various automata implementations it seems like it should be possible to simply ask the NFA 'were there any active states when the input was exhausted?' but i don't see a straightforward way to do this with the exposed APIs (is this possible with the `search_slots` api? it is unclear).

Perhaps there is a way to modify the generated patterns or automata with extra 'terminal' states to support this? 

Perhaps we could consider this as a kind of a 'streaming match' where we are only providing part of the input and asking if there is any suffix that could lead to a match?

### Uses
Would this be useful for `ripgrep` itself? it appears not since ripgrep just uses globs to skip certain files whereas we are using them to select files to watch.

### Alternatives
I did implement a version of this with my own Thompson style NFA implementation, but the regex/globset crates are _much_ more mature and so it makes more sense for this functionality to live upstream. Thanks!

---

_Comment by @lukesandberg on 2025-05-15 17:02_

I spent a while looking at the `PikeVm` implementation and I don't think it is reasonable to really implement this at that level since it would require threading new APIs through the whole stack.

Adding some kind of additional `wildcard_suffix` bit to the `Input` type is perhaps plausible where if we exhaust the input and the bit is set we could always match if that is set..., but again very invasive.

So perhaps the better idea is to change regex construction, taking my examples from above:

 - `src/**/*.js` -> `src/.*`

- `{src,lib,test}/**/*.js` -> `(src|lib|test)/.*`

- `*.js` -> `[^/]*\.js`

- `*/*/*.js` -> `[^/]*/(?:[^/]*/(?:[^/]*/)?)?` 

so i think the algorithm would be similar to the current regex construction but

tokens like `**` would just short circuit
literal separators `/` would induce an optional match `(?:../)?`

I think that would cover it?  not entirely sure, luckily globs are pretty simple.

The final question would be about API

This could be a parallel API to `GlobMatcher` and `GlobSet` altogether or it could be new functionality on those types.

As new functionality this would increase memory overhead, but could perhaps be `feature` guarded.

As parallel APIs we get new naming problems `GlobDirFilter?` but avoid configuration surfaces. 

I think i will try to put together a PR using a `feature` flag idea but would of course be happy to accept feedback on this.

---

_Comment by @BurntSushi on 2025-05-15 17:08_

I think I am probably pretty unlikely to accept big changes to `globset` at this point. My plan (for a few years now, with middling progress) is to revisit it from first principles. That doesn't help you much. But realistically speaking, I'm not going to have the bandwidth to review a change like this. So you're probably better off forking for now and going your own way.

---

_Comment by @lukesandberg on 2025-05-15 21:35_

Thanks for the feedback, a bit disappointing of course but I appreciate the clarity.  I do believe my implementation will not be very large (~600 locs with tests if that makes a difference), but I also understand the hesitation to support a feature you don't have a usecase for.

I am curious what issues you see in the current implementation that you hope to address?

---

_Closed by @lukesandberg on 2025-05-16 01:53_

---
