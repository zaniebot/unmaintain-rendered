```yaml
number: 1671
title: "crates/globset: (experimental) Add support for matching parent directories of a path"
type: pull_request
state: closed
author: andrewhickman
labels: []
assignees: []
draft: true
base: master
head: partial-match
created_at: 2020-08-29T18:43:52Z
updated_at: 2021-05-31T00:25:31Z
url: https://github.com/BurntSushi/ripgrep/pull/1671
synced_at: 2026-01-12T18:23:14Z
```

# crates/globset: (experimental) Add support for matching parent directories of a path

---

_@andrewhickman_

Hi,

I mostly wrote this code to suit my own need of an efficient way to visit all the paths matched by a glob. As such it might not be suitable for inclusion in this crate, so feel free to close this. However I thought it might be useful :slightly_smiling_face:

This adds `Glob::compile_parent_matcher`, which returns a regex that matches not just the globs paths, but all parent paths as well. For example the regex for the glob `a/b/*` would look something like `^a(?:b/(?:/.*)?)?$`. By using this regex as a filter in `walkdir` or similar, it will avoid traversing directories unnecessarily.

---

_Renamed from "Add support for matching parent directories of a path" to "crates/globset: (experimental) Add support for matching parent directories of a path" by @andrewhickman on 2020-08-29 18:44_

---

_Comment by @BurntSushi on 2021-05-31 00:25_

Thanks! I think this is an interesting idea, but I think I'd want to consider the feature a bit more holistically before adding public APIs. I might very well circle back around to this at some point (likely not soon though). But for now, I'm going to close this.

---

_Closed by @BurntSushi on 2021-05-31 00:25_

---
