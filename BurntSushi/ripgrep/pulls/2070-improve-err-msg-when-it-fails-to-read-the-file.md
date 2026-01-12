```yaml
number: 2070
title: Improve err msg when it fails to read the file specified in RIPGREP_CONFIG_PATH
type: pull_request
state: merged
author: mi-wada
labels: []
assignees: []
merged: true
base: master
head: improve-errmsg-around-ripgrep_config_path
created_at: 2021-11-15T10:50:27Z
updated_at: 2024-08-12T15:16:00Z
url: https://github.com/BurntSushi/ripgrep/pull/2070
synced_at: 2026-01-12T18:23:14Z
```

# Improve err msg when it fails to read the file specified in RIPGREP_CONFIG_PATH

---

_@mi-wada_

close: #1990

## Behavior before this change
```sh
$ RIPGREP_CONFIG_PATH=no_exist_path rg 'search regex'
no_exist_path: No such file or directory (os error 2)
```

## Behavior after this change
```sh
$ RIPGREP_CONFIG_PATH=no_exist_path rg 'search regex'
failed to read the file specified in RIPGREP_CONFIG_PATH: no_exist_path: No such file or directory (os error 2)
```

---

_@BurntSushi approved on 2021-11-15 15:03_

I buy it, thanks!

---

_Merged by @BurntSushi on 2021-11-15 15:29_

---

_Closed by @BurntSushi on 2021-11-15 15:29_

---

_Branch deleted on 2024-08-12 15:16_

---
