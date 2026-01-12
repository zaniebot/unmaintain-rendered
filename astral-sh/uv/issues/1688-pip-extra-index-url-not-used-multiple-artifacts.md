```yaml
number: 1688
title: PIP_EXTRA_INDEX_URL not used. Multiple artifacts feeds not parsed correctly.
type: issue
state: closed
author: MarcSkovMadsen
labels:
  - duplicate
assignees: []
created_at: 2024-02-19T12:31:14Z
updated_at: 2024-02-19T16:01:22Z
url: https://github.com/astral-sh/uv/issues/1688
synced_at: 2026-01-12T15:58:31Z
```

# PIP_EXTRA_INDEX_URL not used. Multiple artifacts feeds not parsed correctly.

---

_@MarcSkovMadsen_

My team uses a `PIP_EXTRA_INDEX_URL` environment variable to point to multiple azure artifacts feeds.

If I try to use `uv` 

- the `PIP_EXTRA_INDEX_URL` variable seems not to be used and
- `--extra-index-url` does not work with a space separated list of artifact feeds.

## Reproduce Basic Issue

- `export PIP_EXTRA_INDEX_URL=https://USER:PAT@pkgs.dev.azure.com/ORGANISATION/_packaging/FEED%40Local/pypi/simple/`
- `uv pip install CUSTOM_PACKAGE`

```bash
  × No solution found when resolving dependencies:
  ╰─▶ Because CUSTOM_PACKAGE was not found in the package registry and you require CUSTOM_PACKAGE, we can conclude that the requirements are unsatisfiable.
```

If I run 

- `pip install CUSTOM_PACKAGE` I get it installed

## Issue with `--extra-index-url`

If I try

```bash
uv pip install CUSTOM_PACKAGE --extra-index-url=$PIP_EXTRA_INDEX_URL
```

I get

```bash
error: Failed to download: CUSTOM_PACKAGE==0.1.1110624
  Caused by: HTTP status client error (405 Method Not Allowed) for url (https://pkgs.dev.azure.com/ORGANISATION/_packaging/SOME_ID@ANOTHER_ID/pypi/download/CUSTOM_PACKAGE/0.1.1110624/CUSTOM_PACKAGE-0.1.1110624-py3-none-any.whl#sha256=SOME_SHA256_CODE)
```

This is the same issue as https://github.com/astral-sh/uv/issues/1371.

## Issue with multiple artifacts feeds

In practice we use multiple azure artifacts feeds. We set `PIP_EXTRA_INDEX_URL` to a space separated list of the urls.

```bash
export PIP_EXTRA_INDEX_URL="https://USER:PASSWORD@pkgs.dev.azure.com/ORGANISATION/_packaging/FEED_1%40Local/pypi/simple/ https://USER:PASSWORD@pkgs.dev.azure.com/ORGANISATION/_packaging/FEED_2/pypi/simple/"
```

The `PIP_EXTRA_INDEX_URL` is again not taken into account

```bash
uv pip install CUSTOM_PACKAGE
  × No solution found when resolving dependencies:
  ╰─▶ Because CUSTOM_PACKAGE was not found in the package registry and you require CUSTOM_PACKAGE, we can conclude that the requirements are unsatisfiable.
```

If I try to work around it using `--extra-index-url` i see

```bash
 uv pip install CUSTOM_PACKAGE --extra-index-url="$PIP_EXTRA_INDEX_URL"
error: HTTP status client error (400 Bad Request) for url (https://pkgs.dev.azure.com/ORGANISATION/_packaging/FEED_1%40Local/pypi/simple/%20https://USER:PASSWORD@pkgs.dev.azure.com/ORGANISATION/_packaging/FEED_2/pypi/simple/CUSTOM_PACKAGE/)
```

Thus `uv` does not handle multiple feeds correctly.

Using `pip` works

```bash
pip install CUSTOM_PACKAGE --extra-index-url="$PIP_EXTRA_INDEX_URL"
...
Successfully installed CUSTOM_PACKAGE-0.1.1110624.
```


---

_Comment by @notatallshaw on 2024-02-19 13:52_

FYI, for any use case other than a mirror, pip extra index url is inheriently insecure, as pip does not guarantee ordering of which index url it will read first an attacker can place a package on the other index with the same name as your internal package. This isn't theoretical, malicious packages were uploaded to PyPI that mirrored the names of packages on the pytorch index.

The attempt to solve this is PEP 708, but it hasn't been implemented by PyPI yet. But I would strongly advise no one use extra index url with pip unless they very specifically are hosting multiple index mirrors.

For uv see related discussion here: https://github.com/astral-sh/uv/issues/171

---

_Comment by @charliermarsh on 2024-02-19 14:28_

Thanks! The env var is a duplicate of https://github.com/astral-sh/uv/issues/1535. The 401 is a duplicate of https://github.com/astral-sh/uv/issues/1458.

---

_Closed by @charliermarsh on 2024-02-19 14:28_

---

_Comment by @notatallshaw on 2024-02-19 14:34_

> The env var is a duplicate of #1535.

FYI pip index url and pip extra index url are two different features of Pip. The latter allowing to query multiple indexes (and as I've said is inherently insecure).

You seem to have linked to the former and this request is about the latter.

---

_Comment by @charliermarsh on 2024-02-19 14:39_

@notatallshaw - Ah sorry, I just consider it "the same" underlying issue, since if we add support for `PIP_INDEX_URL` via an environment variable, we'll naturally do the same for `PIP_EXTRA_INDEX_URL`.

---

_Label `duplicate` added by @zanieb on 2024-02-19 15:42_

---

_Comment by @MarcSkovMadsen on 2024-02-19 15:54_

Thanks @charliermarsh . I don't think the two mentioned existing issues covers the "Issue with multiple artifacts feeds". Would you consider and reopen if you agree. Thanks.

---

_Comment by @zanieb on 2024-02-19 15:59_

I can open an issue to track that — although I think the solution here is to pass `--extra-index-url` multiple times.

---

_Comment by @zanieb on 2024-02-19 16:01_

- https://github.com/astral-sh/uv/issues/1702

---
