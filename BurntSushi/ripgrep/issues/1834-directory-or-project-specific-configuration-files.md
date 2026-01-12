```yaml
number: 1834
title: directory or project specific configuration files
type: issue
state: closed
author: fidalgo
labels:
  - wontfix
assignees: []
created_at: 2021-03-23T06:40:23Z
updated_at: 2024-06-03T15:39:10Z
url: https://github.com/BurntSushi/ripgrep/issues/1834
synced_at: 2026-01-12T16:13:24Z
```

# directory or project specific configuration files

---

_@fidalgo_

#### Describe your feature request
My request is about to add support for ripgrep to lookup for a `.ripgreprc` file in the current directory and then look up in the user folder.

Software projects are very different by nature, and having a global configuration of ripgrep, is not always suitable for different projects. If I'm doing a `C` project, I would like to exclude the `build` directory, but if I'm using a ruby project that build could contain some scripts that will help me, let's say create an artefact for deploy.

Having the same configuration and especially use it for the backend of a search system in the editor (my case) will lead in unsatisfactory results.


---

_Comment by @BurntSushi on 2021-03-23 08:53_

Not happening, sorry. I elaborated more here: https://github.com/BurntSushi/ripgrep/issues/1373#issuecomment-753659388

Instead, I recommend sourcing a project specific shell script that sets the appropriate env var.

---

_Closed by @BurntSushi on 2021-03-23 08:53_

---

_Label `wontfix` added by @BurntSushi on 2021-03-23 08:53_

---

_Renamed from "Allow to set custom directory configuration" to "directory or project specific configuration files" by @BurntSushi on 2021-03-23 08:55_

---

_Comment by @salim-b on 2024-06-03 15:39_

Possibly useful bit of information for future visitors here: While ripgrep doesn't support project-specific *configuration* files, it does actually support [project-specific ***ignore*** files](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#automatic-filtering) incl. a ripgrep-specific **`.rgignore`** file. 😊

---
