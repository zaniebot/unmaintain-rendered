```yaml
number: 8752
title: "uv-client: switch to RFC 9110 compatible format"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/if-modified-since-format-fix
created_at: 2024-11-01T12:34:38Z
updated_at: 2024-11-01T13:46:27Z
url: https://github.com/astral-sh/uv/pull/8752
synced_at: 2026-01-10T12:08:45Z
```

# uv-client: switch to RFC 9110 compatible format

---

_Pull request opened by @BurntSushi on 2024-11-01 12:34_

This still utilizes the RFC 2822 datetime formatter, but utilizes new
methods [added in jiff 0.1.14] to emit timestamps in a format strictly
compatible with RFC 9110.

It seems like most HTTP servers were pretty flexible and supported RFC
2822 datetime formats, but #8747 shows at least one case where that
isn't true. Given that the [MDN docs prescribe RFC 9110], we defer to
them.

Fixes #8747

[added in jiff 0.1.14]: https://github.com/BurntSushi/jiff/pull/154
[MDN docs prescribe RFC 9110]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/If-Modified-Since


---

_Review requested from @charliermarsh by @BurntSushi on 2024-11-01 12:34_

---

_@charliermarsh approved on 2024-11-01 13:43_

---

_Merged by @BurntSushi on 2024-11-01 13:46_

---

_Closed by @BurntSushi on 2024-11-01 13:46_

---

_Branch deleted on 2024-11-01 13:46_

---
