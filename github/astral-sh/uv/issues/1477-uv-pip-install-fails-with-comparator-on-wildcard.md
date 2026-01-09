---
number: 1477
title: uv pip install fails with comparator on wildcard version specifier
type: issue
state: closed
author: fennovj
labels:
  - bug
assignees: []
created_at: 2024-02-16T11:04:19Z
updated_at: 2024-03-19T17:24:54Z
url: https://github.com/astral-sh/uv/issues/1477
synced_at: 2026-01-07T13:12:16-06:00
---

# uv pip install fails with comparator on wildcard version specifier

---

_Issue opened by @fennovj on 2024-02-16 11:04_

Minimal example:

requirements.txt with content:

```
requests>=2.30.*
```
or
```
requests<=2.30.*
```

`pip install -r requirements.txt` will succeed, but `uv pip install -r requirements.txt` will fail.

```
error: Couldn't parse requirement in `requirements.txt` at position 0
  Caused by: Operator >= cannot be used with a wildcard version specifier
requests>=2.30.*
        ^^^^^^^^
```

For an example in the wild (how I encountered this issue):

`pip install polars==0.14.0` succeeds, but `uv pip install polars==0.14.0` fails with

```
error: Failed to read metadata for: polars==0.14.0
  Caused by: Failed to parse METADATA file at: /Users/fenno/scratch/.env/lib/python3.10/site-packages/polars-0.14.0.dist-info/METADATA
  Caused by: Operator >= cannot be used with a wildcard version specifier
pyarrow>=4.0.*; extra == 'pyarrow'
       ^^^^^^^
```

---

_Label `bug` added by @charliermarsh on 2024-02-16 14:57_

---

_Comment by @charliermarsh on 2024-02-16 14:57_

Thanks! That's actually not a "valid" specifier, but we have some fixups in the code for packages that use invalid specifiers. Will make sure this is covered.

---

_Assigned to @BurntSushi by @BurntSushi on 2024-02-16 14:58_

---

_Referenced in [astral-sh/uv#1529](../../astral-sh/uv/pulls/1529.md) on 2024-02-16 19:03_

---

_Closed by @BurntSushi on 2024-02-16 20:52_

---

_Comment by @webcoderz on 2024-03-19 17:23_

still running into this problem

`37.90 stderr: error: Failed to download: torchsde==0.2.5
37.90   Caused by: Couldn't parse metadata of torchsde-0.2.5-py3-none-any.whl from https://files.pythonhosted.org/packages/73/8d/efd3e7b31ea854d0bd6886aa3cf44914adce113a6d460850af41ac1dd4dd/torchsde-0.2.5-py3-none-any.whl.metadata
37.90   Caused by: Operator >= cannot be used with a wildcard version specifier
37.90 numpy (>=1.19.*) ; python_version >= "3.7"
37.90        ^^^^^^^^`

---

_Comment by @charliermarsh on 2024-03-19 17:24_

Can you open a new issue please?

---

_Referenced in [astral-sh/uv#2546](../../astral-sh/uv/issues/2546.md) on 2024-03-19 18:42_

---
