```yaml
number: 3255
title: "printer: add Zed hyperlink alias"
type: pull_request
state: open
author: connorjsmith
labels: []
assignees: []
base: master
head: zed-hyperlink-format
created_at: 2026-01-03T20:34:48Z
updated_at: 2026-01-03T20:35:24Z
url: https://github.com/BurntSushi/ripgrep/pull/3255
synced_at: 2026-01-12T18:23:15Z
```

# printer: add Zed hyperlink alias

---

_@connorjsmith_

Similar to other editor aliases, main change is that this uses the `zed://` scheme

## Test plan
This results in line numbers and column numbers being emitted as expected
```sh
cargo run -- --hyperlink-format=zed "main"
```



---
