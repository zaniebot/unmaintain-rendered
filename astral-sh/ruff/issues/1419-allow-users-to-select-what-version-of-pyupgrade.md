```yaml
number: 1419
title: Allow users to select what version of pyupgrade they want to use
type: issue
state: closed
author: colin99d
labels:
  - question
assignees: []
created_at: 2022-12-28T02:47:34Z
updated_at: 2022-12-28T12:25:16Z
url: https://github.com/astral-sh/ruff/issues/1419
synced_at: 2026-01-10T12:05:28Z
```

# Allow users to select what version of pyupgrade they want to use

---

_Issue opened by @colin99d on 2022-12-28 02:47_

Pyupgrade allows users to select which version of python to lint until. This functionality will be CRITICAL for anyone who wants to use Ruff, while keeping backwards compatibility. Would love to flesh out implementation fully, and the I can work on it once #827 is complete.

---

_Comment by @charliermarsh on 2022-12-28 02:58_

I believe we support this via the [`target-version`](https://github.com/charliermarsh/ruff#target-version) setting, but let me know if I'm misunderstanding!

---

_Label `question` added by @charliermarsh on 2022-12-28 02:58_

---

_Closed by @charliermarsh on 2022-12-28 12:14_

---

_Comment by @charliermarsh on 2022-12-28 12:14_

(Closing but just let me know if I've misunderstood.)

---

_Comment by @colin99d on 2022-12-28 12:17_

I know I havent been adding this functionality into my PRs? Do I need to just add a target_version requirement to them?

---

_Comment by @charliermarsh on 2022-12-28 12:20_

We don't support versions before 3.7, so anything that's marked as 3.7+ doesn't need this logic. But otherwise, you can look at the LRU cache (3.8+) case as an example:

```rust
if self.settings.enabled.contains(&CheckCode::UP011)
    && self.settings.target_version >= PythonVersion::Py38
{
    pyupgrade::plugins::unnecessary_lru_cache_params(self, decorator_list);
}
```

---

_Comment by @colin99d on 2022-12-28 12:25_

Oh, perfect! I definitely missed this. Thanks for explaining it to me.

---
