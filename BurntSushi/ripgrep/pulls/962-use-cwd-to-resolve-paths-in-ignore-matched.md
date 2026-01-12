```yaml
number: 962
title: "Use cwd to resolve paths in Ignore::matched"
type: pull_request
state: closed
author: tmccombs
labels: []
assignees: []
base: master
head: fix-ignore-paths
created_at: 2018-06-24T06:56:59Z
updated_at: 2019-04-06T12:08:39Z
url: https://github.com/BurntSushi/ripgrep/pull/962
synced_at: 2026-01-12T18:23:13Z
```

# Use cwd to resolve paths in Ignore::matched

---

_@tmccombs_

Fixes #829 and #278.

This does change the Ignore struct to depend on the working directory at
the time of creation, I _think_ this is fine, since Ignore isn't
publicly accessible and the Walk structs already depend on the current
working directory implicitly.

I tried to make a minimal change to fix the issue. An alternative
implementation could be to call `current_dir` in the `matched_ignore`
method instead of in `add_parents`, but I'm not sure of the performance
implications of doing that.

Another possible solution would be to change the places that we call
`Ignore::matched` to change the path to be relative to the absolute_base
of the `Ignore`.

---

_Comment by @BurntSushi on 2019-04-06 12:08_

https://github.com/BurntSushi/ripgrep/pull/963#issuecomment-480499071

---

_Closed by @BurntSushi on 2019-04-06 12:08_

---
