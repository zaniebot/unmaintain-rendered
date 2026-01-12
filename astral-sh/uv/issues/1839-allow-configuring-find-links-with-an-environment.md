```yaml
number: 1839
title: Allow configuring FIND_LINKS with an environment variable
type: issue
state: closed
author: alexandre-fabra
labels:
  - configuration
assignees: []
created_at: 2024-02-21T22:37:43Z
updated_at: 2024-12-20T15:35:58Z
url: https://github.com/astral-sh/uv/issues/1839
synced_at: 2026-01-12T15:58:32Z
```

# Allow configuring FIND_LINKS with an environment variable

---

_@alexandre-fabra_

I would like to be able to configure the `find-links` URL with an environment variable similar to how `PIP_FIND_LINKS` works.

To verify the current behaviour, I used the following snippet:

```bash
export UV_INDEX_URL=https://index.url \
&& export UV_EXTRA_INDEX_URL=https://extra.index.url \
&& export UV_FIND_LINKS=https://find.links \
&& uv pip install --help
```

and then verified that the env values for `INDEX_URL` and `EXTRA_INDEX_URL` are honored, but not for `FIND_LINKS`:

```
  -i, --index-url <INDEX_URL>
          The URL of the Python package index (by default: https://pypi.org/simple)
          
          [env: UV_INDEX_URL=https://index.url]

      --extra-index-url <EXTRA_INDEX_URL>
          Extra URLs of package indexes to use, in addition to `--index-url`
          
          [env: UV_EXTRA_INDEX_URL=https://extra.index.url]

  -f, --find-links <FIND_LINKS>
          Locations to search for candidate distributions, beyond those found in the indexes.
          
          If a path, the target must be a directory that contains package as wheel files (`.whl`) or source distributions (`.tar.gz` or `.zip`) at the top level.
          
          If a URL, the page must contain a flat list of links to package files.

```

---

_Comment by @hauntsaninja on 2024-02-21 22:39_

This is maybe a duplicate of the more general https://github.com/astral-sh/uv/issues/1789

---

_Closed by @charliermarsh on 2024-02-22 00:48_

---

_Comment by @charliermarsh on 2024-02-22 00:48_

Agree, want to add these.

---

_Comment by @garthk on 2024-09-06 00:19_

I regret #1789 was closed without a UV_FIND_LINKS:

> Closing for now as we have many of these.
> ―  @charliermarsh in https://github.com/astral-sh/uv/issues/1789#issuecomment-2066674786

---

_Reopened by @zanieb on 2024-09-06 00:50_

---

_Comment by @zanieb on 2024-09-06 00:50_

Thanks for noting.

---

_Label `configuration` added by @zanieb on 2024-09-06 00:51_

---

_Comment by @konstin on 2024-12-20 15:35_

This was fixed in #7912, adding support for `UV_FIND_LINKS`.

---

_Closed by @konstin on 2024-12-20 15:35_

---
