```yaml
number: 2107
title: "ignore/gitignore: Support quoted INI values"
type: pull_request
state: closed
author: Enzime
labels: []
assignees: []
base: master
head: fix/quoted-excludes-file
created_at: 2021-12-20T03:12:19Z
updated_at: 2023-07-08T22:26:02Z
url: https://github.com/BurntSushi/ripgrep/pull/2107
synced_at: 2026-01-12T18:23:14Z
```

# ignore/gitignore: Support quoted INI values

---

_@Enzime_

Fix parsing `excludesFile` values that are quoted to match Git and [other INI parsers](https://en.wikipedia.org/wiki/INI_file#Quoted_values).

---

_Comment by @Enzime on 2022-08-29 10:18_

@BurntSushi any thoughts on this PR?

---

_Comment by @BurntSushi on 2023-07-08 22:26_

I think I'm going to pass on this. It permits mixed quotes and doesn't handle escaping. Maybe it's still better than the status quo, but I'd rather just keep things simple instead of trying to hackily improve it. Probably we should spend more effort trying to parse the INI file, but it isn't a priority for me right now.

---

_Closed by @BurntSushi on 2023-07-08 22:26_

---
