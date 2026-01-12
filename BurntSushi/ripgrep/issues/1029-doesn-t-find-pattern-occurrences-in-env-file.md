```yaml
number: 1029
title: "Doesn't find pattern occurrences in .env file"
type: issue
state: closed
author: Boscop
labels: []
assignees: []
created_at: 2018-08-27T05:36:05Z
updated_at: 2018-08-27T06:04:51Z
url: https://github.com/BurntSushi/ripgrep/issues/1029
synced_at: 2026-01-12T16:13:22Z
```

# Doesn't find pattern occurrences in .env file

---

_@Boscop_

#### What version of ripgrep are you using?
ripgrep 0.9.0

#### What operating system are you using ripgrep on?

arch

#### Describe your question, feature request, or bug.

It doesn't find occurrences in .env file.

#### If this is a bug, what are the steps to reproduce the behavior?

> $ rg --hidden DATABASE_URL

#### If this is a bug, what is the actual behavior?

Only finds results in other files.

#### If this is a bug, what is the expected behavior?

Should show the occurrence in the .env file `foo/bar/.env`:
> DATABASE_URL=...


---

_Comment by @okdana on 2018-08-27 05:41_

You didn't include the `--debug` output like the issue template says, but given the purpose of `.env` files i suppose you've probably added it to your `.gitignore`, which `rg` respects by default

`rg -uu` will search hidden files without respecting `.gitignore`

---

_Comment by @Boscop on 2018-08-27 06:04_

Ah, thanks :)

---

_Closed by @Boscop on 2018-08-27 06:04_

---
