```yaml
number: 9694
title: "Publish: Support --index <name>"
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
assignees: []
merged: true
base: main
head: konsti/upload-index
created_at: 2024-12-06T19:20:48Z
updated_at: 2024-12-10T21:17:49Z
url: https://github.com/astral-sh/uv/pull/9694
synced_at: 2026-01-10T12:00:01Z
```

# Publish: Support --index <name>

---

_Pull request opened by @konstin on 2024-12-06 19:20_

When publishing, we currently ask the user to set `--publish-url` to the upload URL and `--check-url` to the simple index URL, or the equivalent configuration keys. But that's redundant with the `[[tool.uv.index]]` declaration. Instead, we extend `[[tool.uv.index]]` with a `publish-url` entry and allow passing `uv publish --index <name>`.

`uv publish --index <name>` requires the `pyproject.toml` to be present when publishing, unlike using `--publish-url ... --check-url ...` which can be used e.g. in CI without a checkout step. `--index` also always uses the check URL feature to aid upload consistency.

The documentation tries to explain both approaches together, which overlap for the check URL feature.

Fixes #8864

---

_Label `enhancement` added by @konstin on 2024-12-06 19:20_

---

_Review requested from @charliermarsh by @konstin on 2024-12-06 19:20_

---

_Review requested from @zanieb by @konstin on 2024-12-06 19:20_

---

_Review comment by @zanieb on `crates/uv-static/src/env_vars.rs`:143 on 2024-12-10 14:52_

```suggestion
    pub const UV_PUBLISH_INDEX: &'static str = "UV_PUBLISH_INDEX";
```

---

_@zanieb reviewed on 2024-12-10 14:52_

---

_Review comment by @zanieb on `docs/reference/cli.md`:8133 on 2024-12-10 14:52_

```suggestion
<p>May also be set with the <code>UV_PUBLISH_INDEX</code> environment variable.</p>
```

---

_@zanieb reviewed on 2024-12-10 14:52_

---

_@zanieb approved on 2024-12-10 14:53_

Makes sense to me!

---

_@konstin reviewed on 2024-12-10 15:46_

---

_Review comment by @konstin on `crates/uv-static/src/env_vars.rs`:143 on 2024-12-10 15:46_

you have a good eye

---

_@charliermarsh approved on 2024-12-10 21:17_

---

_Merged by @konstin on 2024-12-10 21:17_

---

_Closed by @konstin on 2024-12-10 21:17_

---

_Branch deleted on 2024-12-10 21:17_

---
