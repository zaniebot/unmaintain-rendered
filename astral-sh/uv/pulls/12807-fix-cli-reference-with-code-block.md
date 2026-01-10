```yaml
number: 12807
title: Fix CLI reference with code block
type: pull_request
state: merged
author: konstin
labels:
  - documentation
assignees: []
merged: true
base: main
head: konsti/fix-publish-index-doc
created_at: 2025-04-10T13:24:27Z
updated_at: 2025-04-10T15:59:42Z
url: https://github.com/astral-sh/uv/pull/12807
synced_at: 2026-01-10T11:10:40Z
```

# Fix CLI reference with code block

---

_Pull request opened by @konstin on 2025-04-10 13:24_

Due to https://github.com/clap-rs/clap/issues/5900, clap folds docstring code blocks in a way that breaks the rendering of the `uv publish --index` option to html. As a workaround, `verbatim_doc_comment` prevents this. 


Release:
![image](https://github.com/user-attachments/assets/66d9af51-ac23-47f6-a859-7b20a4f1f4a2)

PR:
![image](https://github.com/user-attachments/assets/6a32a5a6-1dd8-49ff-a853-9df02f0141ad)


Release:
```
      --index <INDEX>
          The name of an index in the configuration to use for publishing.
          
          The index must have a `publish-url` setting, for example:
          
          ```toml [[tool.uv.index]] name = "pypi" url =
          "https://pypi.org/simple" publish-url =
          "https://upload.pypi.org/legacy/" ```
          
          The index `url` will be used to check for existing files to skip
          duplicate uploads.
          
          With these settings, the following two calls are equivalent:
          
          ``` uv publish --index pypi uv publish --publish-url
          https://upload.pypi.org/legacy/ --check-url https://pypi.org/simple
          ```
          
          [env: UV_PUBLISH_INDEX=]
```

PR:
```
      --index <INDEX>
          The name of an index in the configuration to use for publishing.
          
          The index must have a `publish-url` setting, for example:
          
          ```toml
          [[tool.uv.index]]
          name = "pypi"
          url = "https://pypi.org/simple"
          publish-url = "https://upload.pypi.org/legacy/"
          ```
          
          The index `url` will be used to check for existing files to skip
          duplicate uploads.
          
          With these settings, the following two calls are equivalent:
          
          ```shell
          uv publish --index pypi
          uv publish --publish-url https://upload.pypi.org/legacy/
          --check-url https://pypi.org/simple
          ```
          
          [env: UV_PUBLISH_INDEX=]
```	

Fixes #12652

---

_Label `documentation` added by @konstin on 2025-04-10 13:24_

---

_@jtfmumm approved on 2025-04-10 13:36_

---

_Merged by @zanieb on 2025-04-10 15:59_

---

_Closed by @zanieb on 2025-04-10 15:59_

---

_Branch deleted on 2025-04-10 15:59_

---
