```yaml
number: 3796
title: "`uv` is caching installs based on config, even if build-backend is in-tree"
type: issue
state: open
author: thejcannon
labels:
  - needs-decision
assignees: []
created_at: 2024-05-23T14:48:50Z
updated_at: 2024-05-24T21:01:55Z
url: https://github.com/astral-sh/uv/issues/3796
synced_at: 2026-01-12T15:58:45Z
```

# `uv` is caching installs based on config, even if build-backend is in-tree

---

_@thejcannon_

👋 Ok this one took me a while to figure out, lol

```console
$ echo "First make package \`foo\`" > /dev/null
$ mkdir foo
$ cat << EOF > foo/pyproject.toml
[build-system]
requires = []
build-backend = "bar"
backend-path = ["."]

[project]
name = "foo"
dynamic = ["version", "dependencies"]
EOF
$ echo "Now make hooks \`bar\`" > /dev/null
$ cat << EOF > foo/bar.py
def build_wheel(*args) -> str:
    import setuptools.build_meta

    return setuptools.build_meta.build_wheel(*args)


def prepare_metadata_for_build_wheel(*args) -> str:
    import setuptools.build_meta

    return setuptools.build_meta.prepare_metadata_for_build_wheel(*args)
EOF
$ echo "Great! Lets make a venv and install \`foo\`" > /dev/null
$ uv venv .venv --seed
$ uv pip install --python .venv/bin/python ./foo --no-build-isolation
$ echo "Ok, but I think theres a bug in \`bar\` it actually should error." > /dev/null
$ cat << EOF > foo/bar.py
def build_wheel(*args) -> str:
    raise Exception("DEPRECATED")


def prepare_metadata_for_build_wheel(*args) -> str:
    raise Exception("DEPRECATED")
EOF
$ echo "Lets reinstall foo" > /dev/null
$ uv pip uninstall --python .venv/bin/python foo
$ uv pip install --python .venv/bin/python ./foo --no-build-isolation
$ echo "Hey! That didn't error!" > /dev/null
```

The smoking gun is:

```console
$ touch foo/pyproject.toml
$ uv pip uninstall --python .venv/bin/python foo
$ uv pip install --python .venv/bin/python ./foo
```

Which errors because we touched the timestamp of `pyproject.toml`.

---

_Comment by @thejcannon on 2024-05-23 14:49_

In the real-deal, what's changing is the dynamic `dependencies` METADATA (which isn't being picked up)

---

_Renamed from "`uv` is caching installs based on config, even if build-backend is in-repo" to "`uv` is caching installs based on config, even if build-backend is in-tree" by @thejcannon on 2024-05-23 14:56_

---

_Comment by @charliermarsh on 2024-05-24 00:05_

In general we don't rebuild local packages unless `pyproject.toml` or similar changes. There's some discussion of it elsewhere -- you should be able to pass with `--refresh` to avoid this. However, we should be rebuilding when `dynamic` is present? And we seem to do so in my own testing?

---

_Comment by @thejcannon on 2024-05-24 14:46_

> And we seem to do so in my own testing?

Did you try the reproducer above? (FWIW I'm on `uv 0.1.44 (d417daad7 2024-05-14)`)

---

_Comment by @thejcannon on 2024-05-24 14:46_

But `--refresh` is good advice! 🙌 

---

_Comment by @charliermarsh on 2024-05-24 18:44_

Oh, the thing that's changing here is literally the build backend itself? No, we definitely don't support that right now. I'm not sure that we will -- how can we reliably detect that it has changed? Any ideas? `--refresh` feels like the right answer here.

---

_Label `needs-decision` added by @charliermarsh on 2024-05-24 18:44_

---

_Comment by @thejcannon on 2024-05-24 18:58_

I think your choices are:
- If `pyproject.toml` uses `backend-path` be a coward and always run (pun intended)
  - If you wanted you could scope to "only if it has `dynamic` attributes", but that's still leaving room for error
- \<this bug\>, which admittedly did make me spin my wheels for an hour or two (pun also intended)

---

_Comment by @charliermarsh on 2024-05-24 21:01_

Yeah, we _could_ treat these as if they're dynamic.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-24 21:01_

---
