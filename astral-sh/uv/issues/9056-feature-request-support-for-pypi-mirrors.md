```yaml
number: 9056
title: "[Feature Request] Support for PyPI Mirrors"
type: issue
state: closed
author: liblaf
labels:
  - configuration
  - needs-design
assignees: []
created_at: 2024-11-12T13:08:49Z
updated_at: 2024-11-13T05:16:14Z
url: https://github.com/astral-sh/uv/issues/9056
synced_at: 2026-01-12T15:59:41Z
```

# [Feature Request] Support for PyPI Mirrors

---

_@liblaf_

#### Description

In order to improve download speeds and reliability, it would be beneficial to support configurable PyPI mirrors within the `uv.toml` configuration file. This feature would allow users to specify alternative mirrors for PyPI packages and files, while still retaining the default PyPI link in the `uv.lock` file.

#### Proposed Solution

Inspired by the configuration options in [pixi](https://pixi.sh/latest/reference/pixi_configuration/#mirrors), I propose adding a `[mirrors]` section to the `uv.toml` file. This section would allow users to redirect requests for specific PyPI endpoints to alternative mirrors.

Example configuration:

```toml
[mirrors]
# Redirect all requests for PyPI to the mirror
"https://pypi.org/simple" = [
    "https://mirrors.cernet.edu.cn/pypi/web/simple"
]

# Redirect all requests for files to one of the listed mirrors
"https://files.pythonhosted.org/packages" = [
    "https://mirrors.tuna.tsinghua.edu.cn/pypi/web/packages",
    "https://mirrors.cernet.edu.cn/pypi/web/packages",
]
```

#### Benefits

- **Improved Download Speeds**: Users can leverage faster mirrors located closer to their geographical region.
- **Reliability**: Redundancy in mirror options can help mitigate issues with a single mirror being down.
- **Consistency**: The default PyPI link remains in the `uv.lock` file, ensuring compatibility and clarity.

#### Additional Considerations

- **Fallback Mechanism**: If a specified mirror is unavailable, uv could automatically fall back to the next listed mirror or the default PyPI endpoint.

#### Related Issue

- [Original Issue #9053](https://github.com/astral-sh/uv/issues/9053)

#### Conclusion

Supporting configurable PyPI mirrors in `uv.toml` would enhance the user experience by providing faster and more reliable package downloads. I kindly request the implementation of this feature or further discussion on alternative solutions.

---

_Comment by @charliermarsh on 2024-11-12 14:55_

I think this could be thought of as similar to #6349.

---

_Label `resolver` added by @charliermarsh on 2024-11-12 17:47_

---

_Label `needs-design` added by @charliermarsh on 2024-11-12 17:47_

---

_Label `resolver` removed by @charliermarsh on 2024-11-12 17:47_

---

_Label `configuration` added by @charliermarsh on 2024-11-12 17:47_

---

_Comment by @liblaf on 2024-11-13 05:16_

@charliermarsh Thanks! I didn't notice that issue before.

I think URL redirecting is more flexible than setting proxy URL for index, as URL redirecting can handle cases when `sdist` and `wheels` URL differ from registry URL (e.g. <https://pypi.org> and <https://files.pythonhosted.org>).

Closing in favor of [#6349](https://github.com/astral-sh/uv/issues/6349).

---

_Closed by @liblaf on 2024-11-13 05:16_

---
