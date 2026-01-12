```yaml
number: 6035
title: " WIP: Initial support for translation"
type: pull_request
state: closed
author: sylvestre
labels: []
assignees: []
draft: true
base: master
head: i18n
created_at: 2025-06-13T19:51:24Z
updated_at: 2025-07-02T17:49:08Z
url: https://github.com/clap-rs/clap/pull/6035
synced_at: 2026-01-12T16:14:17Z
```

#  WIP: Initial support for translation

---

_@sylvestre_

 WIP: Initial support for translation. It uses fluent and can be enabled with the feature  "i18n"
And incldues the French translation as poc.

If not built with this feature, it should not add any overhead as it will take the
english string directly in the code.

TODO:
* fix a better path for the locale files
* ship the resources in the binary
* fix some tests failing
* find the right versions of the new dependencies to preserve the MSRV
* Add tests to verify that it is indeed translated
* Update of the doc
    
closes #380

---
