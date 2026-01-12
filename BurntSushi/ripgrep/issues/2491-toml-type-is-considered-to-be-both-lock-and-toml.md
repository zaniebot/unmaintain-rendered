```yaml
number: 2491
title: toml type is considered to be both .lock and .toml files
type: issue
state: closed
author: lesleyrs
labels: []
assignees: []
created_at: 2023-04-16T14:26:25Z
updated_at: 2023-04-16T14:50:31Z
url: https://github.com/BurntSushi/ripgrep/issues/2491
synced_at: 2026-01-12T16:13:24Z
```

# toml type is considered to be both .lock and .toml files

---

_@lesleyrs_

#### What version of ripgrep are you using?

ripgrep 13.0.0

#### How did you install ripgrep?

cargo install ripgrep

#### What operating system are you using ripgrep on?

Windows 10

#### Describe your bug.

`rg a . --type lock` - shows only cargo.lock output
`rg a . --type toml` - shows both cargo.lock and cargo.toml

#### What is the expected behavior?

Just match toml files with the toml type, but if this is intended behavior you can close this. Just wanted to mention.

---

_Comment by @lesleyrs on 2023-04-16 14:50_

Actually I'll close this, just found `--type-list` and I think it's fine this way when you use a better search term.

---

_Closed by @lesleyrs on 2023-04-16 14:50_

---
