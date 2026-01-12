```yaml
number: 1666
title: Expand variables in ripgrep.rc
type: issue
state: closed
author: rpdelaney
labels:
  - duplicate
assignees: []
created_at: 2020-08-26T18:46:32Z
updated_at: 2020-08-26T19:06:30Z
url: https://github.com/BurntSushi/ripgrep/issues/1666
synced_at: 2026-01-12T16:13:24Z
```

# Expand variables in ripgrep.rc

---

_@rpdelaney_

#### Describe your feature request

In bash, I can invoke ripgrep with a custom ignores file like so:

```console
rg --ignore-file "$XDG_CONFIG_HOME/ripgrep.ignore"
```

However, the analogous setting in the ripgrep config does not resolve the path, as variable substitutions are interpreted literally. Only absolute paths allow ripgrep to succeed in finding the file:

```dosini
--ignore-file=$XDG_CONFIG_HOME/ripgrep.ignore     # bad
--ignore-file=~/.config/ripgrep.ignore            # bad
--ignore-file=/home/ryan/.config/ripgrep.ignore   # good
```

Being able to dynamically set configuration options in the rg config would enable me to use the same  config file on different platforms where configuration files are found in different places, as with macOS for example. This tempts me to alias `rg` to `rg --ignore-file...`, which feels like an anti-pattern when a configuration file is available.

---

_Comment by @BurntSushi on 2020-08-26 19:06_

Duplicate of #931 and #1548.

---

_Closed by @BurntSushi on 2020-08-26 19:06_

---

_Label `duplicate` added by @BurntSushi on 2020-08-26 19:06_

---
