```yaml
number: 2620
title: "ignore/types: Add *.sarif for SARIF format files"
type: pull_request
state: merged
author: rhysd
labels: []
assignees: []
merged: true
base: master
head: sarif
created_at: 2023-10-04T00:58:36Z
updated_at: 2023-10-05T17:23:30Z
url: https://github.com/BurntSushi/ripgrep/pull/2620
synced_at: 2026-01-12T18:23:14Z
```

# ignore/types: Add *.sarif for SARIF format files

---

_@rhysd_

[SARIF][] is a format for reporting static analysis results. It is [used by GitHub CodeQL][GH] for example.

Here are some samples from Microsoft's VSCode extension:

https://github.com/microsoft/sarif-vscode-extension/tree/main/samples

The SARIF format is built on top of JSON. This PR associates `*.sarif` files with JSON.

[SARIF]: https://docs.oasis-open.org/sarif/sarif/v2.1.0/csprd01/sarif-v2.1.0-csprd01.html
[GH]: https://docs.github.com/en/code-security/code-scanning/integrating-with-code-scanning/sarif-support-for-code-scanning

---

_@BurntSushi approved on 2023-10-05 17:22_

LGTM, thanks!

---

_Merged by @BurntSushi on 2023-10-05 17:23_

---

_Closed by @BurntSushi on 2023-10-05 17:23_

---
