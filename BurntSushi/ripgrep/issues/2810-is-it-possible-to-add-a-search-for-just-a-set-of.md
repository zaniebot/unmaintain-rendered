```yaml
number: 2810
title: Is it possible to add a search for just a set of directories as a type?
type: issue
state: closed
author: aronchick
labels: []
assignees: []
created_at: 2024-05-16T16:33:00Z
updated_at: 2025-10-11T01:34:45Z
url: https://github.com/BurntSushi/ripgrep/issues/2810
synced_at: 2026-01-12T16:13:24Z
```

# Is it possible to add a search for just a set of directories as a type?

---

_@aronchick_

#### Describe your feature request

Please describe the behavior you want and the motivation. Please also provide
examples of how ripgrep would be used if your feature request were added.

If you're not sure what to write here, then try imagining what the ideal
documentation of your new feature would look like in ripgrep's man page. Then
try to write it.

If you're requesting the addition or change of default file types, please open
a PR. We can discuss it there if necessary.

---------

In my .ripgreprc:
```
--type-add
bac:{bacalhau,bacalhau-examples}/*
```

but when i type:
```
❯ rg -t bac lower
unrecognized file type: bac
```

Did i screw it up?

---

_Comment by @BurntSushi on 2024-05-16 16:46_

You haven't provided enough detail to know whether you screwed it up. For example, you don't say anywhere whether you've set the `RIPGREP_CONFIG_PATH` environment variable, [as documented](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#configuration-file).

---

_Closed by @BurntSushi on 2025-10-11 01:34_

---
