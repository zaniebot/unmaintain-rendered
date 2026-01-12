```yaml
number: 2766
title: "[feature request] Context up to a matching pattern instead of by fixed number of lines"
type: issue
state: closed
author: amosshapira
labels:
  - duplicate
assignees: []
created_at: 2024-03-25T22:22:06Z
updated_at: 2024-03-25T23:24:37Z
url: https://github.com/BurntSushi/ripgrep/issues/2766
synced_at: 2026-01-12T16:13:24Z
```

# [feature request] Context up to a matching pattern instead of by fixed number of lines

---

_@amosshapira_

I'd like to be able to print all lines starting/surrounding (aka "context") a matching line up to another matching pattern, rather than a fixed number of lines.

For instance, given I have a tree with [docker-compose.yml](https://docs.docker.com/compose/compose-file/compose-file-v3/) files and I'm looking for an example standza, e.g.:
```

  envvars:
    image: dkr.ecr.amazonaws.com/envvars:0.1.2
    env_file: .env
    working_dir: /work
    volumes:
      - .:/work

```
I can search through the tree with:
```
rg -A 10 -g docker-compose\* envvars:
```
And get up to 10 lines after the matching `envvars:` line but instead I'd like `rg` to output only up to the next empty line.

A possible suggestion for the flag: to avoid adding more flags, perhaps make the line number parameter of the existing `-A`/`-B`/`-C` flags accept a string e.g. '-A /\n/' to indicate that the context is defined by a pattern rather than a fixed number of lines.

---

_Renamed from "Context up to a matching pattern instead of by line numbers" to "[feature request] Context up to a matching pattern instead of by fixed number of lines" by @amosshapira on 2024-03-25 22:23_

---

_Comment by @BurntSushi on 2024-03-25 23:24_

Duplicate of #2211.

---

_Closed by @BurntSushi on 2024-03-25 23:24_

---

_Label `duplicate` added by @BurntSushi on 2024-03-25 23:24_

---
