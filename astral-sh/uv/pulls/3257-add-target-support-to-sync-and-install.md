```yaml
number: 3257
title: "Add `--target` support to `sync` and `install`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - compatibility
assignees: []
merged: true
base: main
head: charlie/target
created_at: 2024-04-25T00:16:38Z
updated_at: 2024-08-02T17:33:09Z
url: https://github.com/astral-sh/uv/pull/3257
synced_at: 2026-01-12T16:05:31Z
```

# Add `--target` support to `sync` and `install`

---

_@charliermarsh_

## Summary

The approach taken here is to model `--target` as an install scheme in which all the directories are just subdirectories of the `--target`. From there, everything else... just works? Like, upgrade, uninstalls, editables, etc. all "just work".

Closes #1517.


---

_Label `enhancement` added by @charliermarsh on 2024-04-25 00:16_

---

_Label `compatibility` added by @charliermarsh on 2024-04-25 00:16_

---

_@zanieb reviewed on 2024-04-25 00:50_

---

_Review comment by @zanieb on `crates/uv/tests/pip_sync.rs`:4907 on 2024-04-25 00:50_

Can we include some sort of file system assertions here?

---

_@charliermarsh reviewed on 2024-04-25 00:54_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_sync.rs`:4907 on 2024-04-25 00:54_

E.g., that the packages are inside the target directory?

---

_Marked ready for review by @charliermarsh on 2024-04-25 02:01_

---

_Review requested from @zanieb by @charliermarsh on 2024-04-25 02:01_

---

_Review requested from @konstin by @charliermarsh on 2024-04-25 02:22_

---

_@zanieb reviewed on 2024-04-25 03:20_

---

_Review comment by @zanieb on `crates/uv/tests/pip_sync.rs`:4907 on 2024-04-25 03:20_

Yeah

---

_@konstin approved on 2024-04-25 07:56_

---

_Comment by @zanieb on 2024-04-25 14:28_

This is different than pip right? Want to note this in the compatibility doc?

---

_Comment by @charliermarsh on 2024-04-25 15:27_

Hmm, I don't think the _user experience_ actually differs from pip in significant ways, or at least in ways that would work in pip but wouldn't work in uv.

---

_Merged by @charliermarsh on 2024-04-25 23:15_

---

_Closed by @charliermarsh on 2024-04-25 23:15_

---

_Branch deleted on 2024-04-25 23:15_

---

_Comment by @henryiii on 2024-04-26 03:11_

Hmm, I tried this out:

```console
$ cargo run --release --bin uv -- pip install importlib_metadata --target tmpdir
$ ls
bin/                                importlib_metadata-7.1.0.dist-info/ zipp/
importlib_metadata/                 include/                            zipp-3.18.1.dist-info/
```

This looks close to what I'd expect, though the `bin` and `include` (empty) directories were unexpected. I would not expect to be able to `import bin` or `import include` :).

At least for the use case of building zipapps, which is what I use `--target` for, and seems to be a popular usage, you only want platlib/purelib (in the same folder) and don't want data/scripts/headers, which can't be used from a zipapp.

---

_Comment by @charliermarsh on 2024-04-26 03:13_

Yeah, but if you `pip install black --target tmpdir`, you will get a `tmpdir/bin` that contains `black` and `blackd` -- this follows pip. We're just creating those directories upfront rather than lazily. Does it have a material impact?

---

_Comment by @charliermarsh on 2024-04-26 03:14_

> At least for the use case of building zipapps, which is what I use --target for, and seems to be a popular usage, you only want platlib/purelib (in the same folder) and don't want data/scripts/headers, which can't be used from a zipapp.

Yes, but pip adds all of these, as far as I can tell? It just adds them to the top-level. If you `pip install jupyter --target foo`, you will see `foo/share` which contains a bunch of icons and such for Jupyter.

---

_Comment by @charliermarsh on 2024-04-26 03:15_

I understand that you don't need these things for a zipapp. What I'm trying to understand is why it's a problem to include them, if it matches pip's behavior.

---

_Comment by @henryiii on 2024-04-26 03:17_

I think it's just surprising that these were here; I've not bundled a library with scripts or headers in a zipapp before so have never seen them and didn't know pip would stick them there.

---

_Comment by @charliermarsh on 2024-04-26 03:17_

Perhaps I can change it to create those directories lazily so it's less surprising (since they typically would not be created at all in that case).

---

_Comment by @charliermarsh on 2024-04-26 03:29_

Okay, https://github.com/astral-sh/uv/pull/3274 should give you something closer to what you're used to :)

---

_Comment by @henryiii on 2024-04-26 03:33_

Nice, that feels a lot more pip-like. :)

---

_Comment by @Pixel-Minions on 2024-04-27 18:00_

Works great! Thank you.

---

_Comment by @mpderbec on 2024-04-27 22:23_

Just tried it out... With `--target` support, I wouldn't expect to have to create a virtual environment, correct?

(I'm getting this error:)

```
C:\WINDOWS\system32\cmd.exe /c "uv pip install --target="C:\GitHub\crab\site-packages\x64\prod" -r "C:\GitHub\crab\requirements.prod.txt" "
error: Failed to locate a virtualenv or Conda environment (checked: `VIRTUAL_ENV`, `CONDA_PREFIX`, and `.venv`). Run `uv venv` to create a virtualenv.
```

---

_Comment by @charliermarsh on 2024-04-27 22:42_

Mmm, I could see arguments either way given our stance on environments, but probably shouldn't be required, no. You can run with `--system` in the meantime.

---

_Comment by @Pixel-Minions on 2024-04-27 23:22_

You can use --python and pass thr path to the interpreter, it works great
even with embedded versions of python with no venv

On Sat, Apr 27, 2024, 6:42 p.m. Charlie Marsh ***@***.***>
wrote:

> Mmm, I could see arguments either way given our stance on environments,
> but probably shouldn't be required, no. You can run with --system in the
> meantime.
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/uv/pull/3257#issuecomment-2081220303>, or
> unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AAIXWF6KKNYFQXXREELY5CLY7QSVPAVCNFSM6AAAAABGX3QPZOVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZDAOBRGIZDAMZQGM>
> .
> You are receiving this because you commented.Message ID:
> ***@***.***>
>


---

_Comment by @mpderbec on 2024-04-28 00:14_

`--system` works great.

Success story: Our package install time went from 4m27s to 0m8s!

---
