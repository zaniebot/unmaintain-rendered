```yaml
number: 2703
title: "feat(globset): allow custom component separator characters?"
type: issue
state: open
author: akx
labels:
  - question
assignees: []
created_at: 2024-01-05T13:30:59Z
updated_at: 2024-01-05T18:52:59Z
url: https://github.com/BurntSushi/ripgrep/issues/2703
synced_at: 2026-01-12T16:13:24Z
```

# feat(globset): allow custom component separator characters?

---

_@akx_

#### Describe your feature request

As discussed on https://github.com/astral-sh/ruff/discussions/8403.

It could be useful if `globset` allowed a custom component separator character instead of the default `/` or `\` path separator, to make it possible to use `globset` for non-filename patterns such as Python qualified names.

In other words, if (mildly mis)using the current `globset` as-is with `{'foo.x.bar', 'foo.y.bar', 'foo.x.y.bar'}` as potential targets, both `foo.*.bar` and `foo.**.bar` will match all of them; if `globset` could be told to interpret `.` as the separator character instead, `foo.*.bar` wouldn't match `foo.x.y.bar` (as it wouldn't match a filesystem path `foo/x/y/bar`).

One alternative, as discussed on the Ruff issue, is to pre-mangle the globset contents and the strings to be matched to replace dots with slashes, but that would entail additional allocation and copying at some point, which sounds unfortunate.

---

_Comment by @BurntSushi on 2024-01-05 14:57_

I'll think about this. You caught me at a good time because I am in the process of rewriting `globset`. So now is a pretty good time for me to experiment with a setting like this. But I'm skeptical.

Here are some concerns though:

* Globs are pretty tightly coupled to files/directories. For example, they at least have things like, "a trailing `/` means that it will only match directories." Such a thing doesn't really make sense for your use case I think. It doesn't necessarily present a showstopping problem, but it is an oddity.
* Similarly, the "require literal leading dot" option that I plan to add also doesn't make a ton of sense if a glob is being used for something other than a file path. Namely, that setting (which will be enabled by default) really only exists because it's a Unix convention to prefix a file with a `.` if you want it to be hidden. If you changed the separator to `.` for path matching but didn't disable the "require literal leading dot" setting, then I suppose nothing terrible would happen in your case because the `.` is incidentally the separator. But if you used something else like `:`, then it might be a little odd.
* I'm also planning on adding support for walking a directory tree to `globset`. Its input will be a `Glob`. What happens if you build a `Glob` with a separator that isn't a path separator and use it to walk a directory tree? I suppose it could return an error. It's a little ham-fisted.
* While I haven't implemented this yet, globs will also likely need to support detecting absolute paths and handle them specially. File paths have a concept of this, but it doesn't necessarily generalize.
* The `globset` crate currently (and likely will continue to) implements optimizations based around the assumption is it matching file paths. For example, looking at things like `*.{foo,bar,quux}` and recognizing that all it needs to do is find the extension of the path and match it.
* In the status quo, it's possible to assume that `/` is _always_ a separator. I can't think off hand how this might be helpful, but my instincts say it's important. This feature request removes the ability to make that assumption.
* This seems likely to be a feature that has limited utility. In particular, I'd expect that in many cases, separators may be multiple characters. For example, `::` in Rust. That is quite a bit harder to support. Perhaps implausible.

As I created that list, I'm afraid I've gotten more and more skeptical of functionality like this unfortunately.

> One alternative, as discussed on the Ruff issue, is to pre-mangle the globset contents and the strings to be matched to replace dots with slashes, but that would entail additional allocation and copying at some point, which sounds unfortunate.

Possibly unfortunate, but not necessarily. It really depends. I think it would be prudent to start here. And if it ends up being a perf concern, then I actually think the next best step would be to build your own pattern language specifically for this sort of thing. The part of `globset` that translates a glob to a regex string is rather small and not hard copy and modify. Then you just need to feed the resulting regexes to `regex::RegexSet`. It's true that `globset` has other optimizations, but I think at least some of them are no longer relevant, and others are coupled with the notion of searching file paths anyway.

---

_Label `question` added by @BurntSushi on 2024-01-05 14:57_

---

_Comment by @akx on 2024-01-05 18:43_

Thank you for putting so much thought into this suggestion!

For Ruff's use case, I think some of the optimizations `globset` knows for, well, glob sets would be useful – not necessarily the extension matching mentioned above, but matching the entire path by prefix or suffix only is a likely common use case, for e.g. "ban all of `package1.legacy.*`". It's of course not an issue to reimplement them however this'll end up getting done, but would be nice to be able to use existing, tested code :)

But we'll see – if you don't think it's a great idea to allow custom separators (and I don't blame you!), I'll explore other ways of implementing this first (if I have the time and bandwidth, that is...).

---

_Comment by @BurntSushi on 2024-01-05 18:52_

Aye. Ideally, we could push those optimizations down into the regex engine. It's in theory more amenable to such things than it was when I wrote `globset`.

I do think it would be wise to get a "macro" benchmark here. If real use cases are dominated by something other than filtering out Python paths, then it might not be worth optimizing it too much.

---
