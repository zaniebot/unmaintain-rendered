```yaml
number: 8030
title: "Bug: `uv publish` missing `METADATA` file"
type: issue
state: closed
author: jamesbraza
labels:
  - bug
  - external
assignees: []
created_at: 2024-10-09T01:28:56Z
updated_at: 2024-10-15T15:37:35Z
url: https://github.com/astral-sh/uv/issues/8030
synced_at: 2026-01-12T15:59:18Z
```

# Bug: `uv publish` missing `METADATA` file

---

_@jamesbraza_

I am using `uv==0.4.20` and just migrated a publish GitHub Action in a repo using [this workspace layout](https://docs.astral.sh/uv/concepts/workspaces/#workspace-layouts) for several packages to `uv publish`.

```diff
    steps:
      - uses: actions/checkout@v4
      - uses: astral-sh/setup-uv@v3
        with:
          enable-cache: true
      - run: uv sync
      - name: Build a binary wheel and a source tarball
        run: |
          uv build --sdist --wheel --out-dir dist/ .
          uv build --sdist --wheel --out-dir dist/ packages/bird-feeder
          uv build --sdist --wheel --out-dir dist/ packages/seeds
---      - uses: pypa/gh-action-pypi-publish@release/v1
---        with:
---          password: ${{ secrets.PYPI_API_TOKEN }}
+++      - run: uv publish
+++        env:
+++          UV_PUBLISH_TOKEN: ${{ secrets.PYPI_API_TOKEN }}
```

Before, publish worked correctly, but with `uv publish` I am getting:

```none
warning: `uv publish` is experimental and may change without warning
Publishing 6 files https://upload.pypi.org/legacy/
Uploading seeds-0.7.6-py3-none-any.whl (5.0KiB)
error: Failed to publish `dist/seeds-0.7.6-py3-none-any.whl` to https://upload.pypi.org/legacy/
  Caused by: Upload failed with status code 400 Bad Request. Server says: 400 Wheel 'seeds-0.7.6-py3-none-any.whl' does not contain the required METADATA file: seeds-0.7.6.dist-info/METADATA
```

Any idea what's going on here? I think this is a `uv publish` bug given my change was an atomic change, and I was previously using `uv build` successfully

---

_Assigned to @konstin by @charliermarsh on 2024-10-09 09:33_

---

_Comment by @konstin on 2024-10-09 13:02_

What build backend are you using, and could you share more details about your project and your setup? Additionally, `seeds-0.7.6-py3-none-any.whl` is a zip file, could you look inside and check what `seeds-0.7.6.dist-info` in there contains?

---

_Label `needs-mre` added by @konstin on 2024-10-09 13:02_

---

_Comment by @Avasam on 2024-10-09 19:11_

Since I just got the same issue, here's a project: https://github.com/Avasam/typed-D3DShot/tree/24954b8122448926b9afddc1c245796b5e9165a0
And here's the wheel file from `uv build`: [typed_D3DShot-1.0.0-py3-none-any.whl.zip](https://github.com/user-attachments/files/17317480/typed_D3DShot-1.0.0-py3-none-any.whl.zip)
FWIW, dist-info does contain this metadata file (I added the .txt extention to upload to GitHub: 
[METADATA](https://github.com/user-attachments/files/17317489/METADATA.txt)

~~A PyPI issue maybe ? Since I doubt `uv publish` touches the wheel file's content.~~

Backend: setuptools
uv version: uv 0.4.20 (0e1b25a53 2024-10-08)


---

_Comment by @Avasam on 2024-10-09 19:40_

Thanks stackoverflow https://stackoverflow.com/a/76757413
It's case sensitive. So `uv build` issue when naming the folder in the wheel file

I renamed the dist-info folder and it worked. I kept the `.whl` itself with capital letters.

---

_Comment by @jamesbraza on 2024-10-10 03:43_

I just took a closer look, and in my case it's seemingly a confusion with `.` vs `_` in the package name. In my attempts earlier in this issue to remove my repo's specifics, I also clouded the issue.

The package name is [`aviary.gsm8k`](https://pypi.org/project/aviary.gsm8k/), and in the `uv publish` logs:

```none
Uploading aviary_gsm8k-0.7.6-py3-none-any.whl (5.0KiB)
error: Failed to publish `dist/aviary.gsm8k-0.7.6-py3-none-any.whl` to https://upload.pypi.org/legacy/
  Caused by: Upload failed with status code 400 Bad Request. Server says: 400 Wheel 'aviary_gsm8k-0.7.6-py3-none-any.whl' does not contain the required METADATA file: aviary_gsm8k-0.7.6.dist-info/METADATA
```

We see the issue is a mixup with `aviary.gsm8k` and `aviary_gsm8k`. Running `ls dist`:

```none
.gitignore
aviary.gsm8k-0.7.6-py3-none-any.whl
aviary_gsm8k-0.7.6.tar.gz
```

---

_Label `needs-mre` removed by @konstin on 2024-10-15 08:24_

---

_Comment by @konstin on 2024-10-15 09:02_

The problem here is that the wheel filename is invalid: All `.` and `-` need to replaced with `_`, so the valid name would be `aviary_gsm8k-0.7.6-py3-none-any.whl` (https://packaging.python.org/en/latest/specifications/binary-distribution-format/#escaping-and-unicode). This is a setuptools bug: https://github.com/pypa/setuptools/issues/3777.

We're doing two things about this, one is adding a warning for invalid filenames (#8203), the other is adding a workaround specifically for setuptools (#8204), so this wheel can be uploaded, as it can with twine.

---

_Label `bug` added by @konstin on 2024-10-15 11:35_

---

_Label `upstream` added by @konstin on 2024-10-15 11:35_

---

_Closed by @konstin on 2024-10-15 12:22_

---

_Comment by @jamesbraza on 2024-10-15 15:37_

Oh thank you @konstin, nice! Just seeking one point of clarification on your above comment, for my understanding:

>  `.` and `-` need to replaced with `_`, so the valid name would be `aviary.gsm8k-0.7.6-py3-none-any.whl`

Did you mean to say `aviary_gsm8k-0.7.6-py3-none-any.whl`?

---

_Comment by @konstin on 2024-10-15 15:37_

yes, sorry

---
