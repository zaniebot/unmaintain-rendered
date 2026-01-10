```yaml
number: 16489
title: Ignore markers on requirements.txt options
type: pull_request
state: open
author: charliermarsh
labels:
  - compatibility
assignees: []
base: main
head: charlie/opt
created_at: 2025-10-29T00:04:33Z
updated_at: 2025-11-09T14:59:49Z
url: https://github.com/astral-sh/uv/pull/16489
synced_at: 2026-01-10T06:28:12Z
```

# Ignore markers on requirements.txt options

---

_Pull request opened by @charliermarsh on 2025-10-29 00:04_

## Summary

This matches pip's behavior. Given:

```
--extra-index-url=https://download.pytorch.org/whl/cu129 ; sys_platform  != 'darwin'
```

We just ignore the marker. Previously, it was treated as part of the URL.

There's one difference here, which is that in pip, if you quote the entire value, they don't strip the marker -- so:

```
--extra-index-url="https://download.pytorch.org/whl/cu129 ; sys_platform  != 'darwin'"
```

...is treated as a verbatim URL that includes the space, semicolon, etc. For simplicity, we also strip those.

Closes https://github.com/astral-sh/uv/issues/16488.


---

_Review requested from @zanieb by @charliermarsh on 2025-10-29 00:04_

---

_Review requested from @konstin by @charliermarsh on 2025-10-29 00:04_

---

_Label `compatibility` added by @charliermarsh on 2025-10-29 00:04_

---

_Marked ready for review by @charliermarsh on 2025-10-29 00:04_

---

_Review comment by @konstin on `crates/uv-requirements-txt/src/lib.rs`:825 on 2025-10-29 10:26_

```suggestion
/// For example, given `--index-url https://pypi.org/simple ; python_version < "3.8"`,
```

---

_Review comment by @konstin on `crates/uv-requirements-txt/src/lib.rs`:826 on 2025-10-29 10:26_

```suggestion
/// remove the trailing marker and return `https://pypi.org/simple`.
```

---

_Review comment by @konstin on `crates/uv-requirements-txt/src/lib.rs`:841 on 2025-10-29 10:30_

In PEP 508, the whitespace needs to be between URL and semicolon (to check that the semicolon is not part of the URL), instead of after the semicolon.

---

_Comment by @konstin on 2025-10-29 10:41_

I see the pip-compatibility argument, but it does seem confusing that in URLs with markers the markers are silently ignored, instead of applying them conditionally them same way e.g. conflicts do.

```
--extra-index-url=https://download.pytorch.org/whl/nightly/cpu ; sys_platform  == 'darwin'
--extra-index-url=https://download.pytorch.org/whl/cu129 ; sys_platform  != 'darwin'
```

Can we add a warning to indicate to the user that their requirements file doesn't do what they think it does?

---

_@konstin reviewed on 2025-10-29 10:41_

---

_Comment by @zanieb on 2025-10-29 14:04_

I think I'd expect this to be an error outside of the `uv pip` interface and a warning in `uv pip`.

---

_Comment by @charliermarsh on 2025-10-29 16:35_

Okay, I'm going to make this a warning (but not an error). An error would be breaking, and it would also require making the parsing more context-aware which doesn't seem worth the trouble right now IMO.

---

_Comment by @charliermarsh on 2025-10-30 19:26_

Okay, I rewrote the option parsing to unify the behavior rather than split it into post-processing steps.

---

_Comment by @zanieb on 2025-11-09 14:59_

I'm afraid of the parser rewrite but otherwise a +1

@konstin do you want to own reviewing that?

---
