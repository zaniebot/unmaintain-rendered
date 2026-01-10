---
number: 3879
title: Using build as tool fails while pipx suceeds
type: issue
state: closed
author: konstin
labels: []
assignees: []
created_at: 2024-05-28T13:36:06Z
updated_at: 2025-01-14T18:57:16Z
url: https://github.com/astral-sh/uv/issues/3879
synced_at: 2026-01-10T01:23:31Z
---

# Using build as tool fails while pipx suceeds

---

_Issue opened by @konstin on 2024-05-28 13:36_

When trying to build https://github.com/konstin/transformers/tree/konsti/repr-for-uv (4b33866235c546f4cd602c6e11efe3cc167b289b), `pipx run build --installer uv .` passes, while `uv tool run --from build python -m build --installer uv .` fails with:

```
Resolved 3 packages in 1ms
Installed 3 packages in 2ms
 + build==1.2.1
 + packaging==24.0
 + pyproject-hooks==1.1.0
* Creating isolated environment: venv+uv...
* Using external uv from /home/konsti/.cargo/bin/uv
* Installing packages in isolated environment:
  - hatchling
* Getting build dependencies for sdist...
* Building sdist...

Traceback (most recent call last):
  File "/home/konsti/projects/transformers/.uv/.tmpyXHcNs/lib/python3.12/site-packages/build/__main__.py", line 178, in _handle_build_error
    yield
  File "/home/konsti/projects/transformers/.uv/.tmpyXHcNs/lib/python3.12/site-packages/build/__main__.py", line 429, in main
    built = build_call(
            ^^^^^^^^^^^
  File "/home/konsti/projects/transformers/.uv/.tmpyXHcNs/lib/python3.12/site-packages/build/__main__.py", line 276, in build_package_via_sdist
    t.extractall(sdist_out)
  File "/home/konsti/.pyenv/versions/3.12.3/lib/python3.12/tarfile.py", line 2261, in extractall
    tarinfo = self._get_extract_tarinfo(member, filter_function, path)
              ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/konsti/.pyenv/versions/3.12.3/lib/python3.12/tarfile.py", line 2315, in _get_extract_tarinfo
    self._handle_fatal_error(e)
  File "/home/konsti/.pyenv/versions/3.12.3/lib/python3.12/tarfile.py", line 2313, in _get_extract_tarinfo
    tarinfo = filter_function(tarinfo, path)
              ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/konsti/.pyenv/versions/3.12.3/lib/python3.12/tarfile.py", line 828, in data_filter
    new_attrs = _get_filtered_attrs(member, dest_path, True)
                ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/konsti/.pyenv/versions/3.12.3/lib/python3.12/tarfile.py", line 805, in _get_filtered_attrs
    raise AbsoluteLinkError(member)
tarfile.AbsoluteLinkError: 'transformers-4.39.0.dev0/.uv/.tmpyXHcNs/bin/python' is a link to an absolute path

ERROR 'transformers-4.39.0.dev0/.uv/.tmpyXHcNs/bin/python' is a link to an absolute path
```

---

_Label `preview` added by @konstin on 2024-05-28 13:36_

---

_Comment by @njzjz on 2024-06-10 21:12_

For my build backend, this can be resolved by adding `.uv` into `.gitignore`. I think uv should add a `gitignore` file like ruff: https://github.com/astral-sh/ruff/pull/208

---

_Comment by @charliermarsh on 2024-06-10 21:16_

Yes I can fix that separately. Can you file another issue? Can be very brief / short.

---

_Referenced in [astral-sh/uv#4219](../../astral-sh/uv/issues/4219.md) on 2024-06-10 21:32_

---

_Label `preview` removed by @zanieb on 2024-08-20 18:23_

---

_Comment by @konstin on 2025-01-14 18:57_

Can't reproduce this anymore, and we changed python symlink path handling that has probably fixed it.

---

_Closed by @konstin on 2025-01-14 18:57_

---
