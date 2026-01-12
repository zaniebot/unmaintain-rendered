```yaml
number: 1222
title: Remove extra new-line after Clap output
type: pull_request
state: merged
author: okdana
labels: []
assignees: []
merged: true
base: master
head: dana/newline
created_at: 2019-03-13T17:39:54Z
updated_at: 2019-04-06T11:59:37Z
url: https://github.com/BurntSushi/ripgrep/pull/1222
synced_at: 2026-01-12T18:23:13Z
```

# Remove extra new-line after Clap output

---

_@okdana_

31d3e241306f305c1cb94e1882511da2b48dcd36 added this wrapper around the invocation of Clap which manually writes whatever output it produces. But it uses `writeln!()`, and Clap's output is already new-line-delimited, so `--help` and `--version` now have an extra new-line at the end that inexplicably upsets me. This just makes it go away again

---

_@BurntSushi approved on 2019-04-06 11:59_

I like it! Thanks!

---

_Merged by @BurntSushi on 2019-04-06 11:59_

---

_Closed by @BurntSushi on 2019-04-06 11:59_

---
