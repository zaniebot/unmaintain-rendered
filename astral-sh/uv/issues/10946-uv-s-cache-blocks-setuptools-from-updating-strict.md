```yaml
number: 10946
title: "uv's cache blocks setuptools from updating strict-mode editable install sandbox symlinks"
type: issue
state: open
author: charlesnicholson
labels:
  - bug
  - compatibility
assignees: []
created_at: 2025-01-24T20:41:09Z
updated_at: 2025-02-16T13:59:29Z
url: https://github.com/astral-sh/uv/issues/10946
synced_at: 2026-01-12T16:00:24Z
```

# uv's cache blocks setuptools from updating strict-mode editable install sandbox symlinks

---

_@charlesnicholson_

### Summary

"strict" editable installations for source-layout packages behave as follows (roughly, I might be missing steps!):

1. An editable-install directory is created under the source package's `build` directory, with a name similar to `__editable__.package-0.0.1-py3-none-any`. Inside of this directory, 1 symlink is created per source file, that links to the source file contents. (This allows authors to edit source files rapidly without having to deal with the editable install process after the initial setup)
2. A .pth file is created in the venv's site packages library directory, that contains the absolute path to the source package's symlink directory under its `build` directory.

This structure and definition allows Python and tooling (LSPs etc) to resolve import statements directly into the source directory of a package being developed.

When programmers add new files to their package, or rename files in their package, the symlinks in this special directory must be updated. If a developer renames `foo.py` to `bar.py`, they must re-run the editable install process from their package. This allows Python code from _other_ packages to say `from package import bar`- under the hood, the import resolver lands in `package`'s symlink directory, and looks for `bar.py`.
If the symlinks in this directory are not updated, the directory will contain a now-dead symlink to `foo.py`, and no entry at all for `bar.py`.

`uv pip install -e` appears to not invoke setuptools to update this symlink directory when source files are renamed / added / deleted, but `pip install -e` does.

I've created a minimal repro case here: https://github.com/charlesnicholson/uv_editable_install_bug_repro

It defines two packages: `repro_p1` and `repro_p2`. `repro_p2` depends on `repro_p1`. 

There are two test scripts that do the same thing, one in pip and one in uv. The steps are:
1. Create a venv.
2. Editable-install `repro_p1` and `repro_p2`, with strict mode enabled.
3. Call a simple function in `repro_p2` that calls a simple function in `repro_p1`, to show that the packages work.
4. Print a new source file into the `repro_p1` directory, which has another simple function.
5. Re-run step 2, which should update the symlinks.
6. Call a simple function in `repro_p2` that calls the new function in the new `repro_p1` file

This process succeeds with pip (run `./test_with_pip.sh`) and fails with uv (run `./test_with_uv.sh`).

I've only tested this on macOS, but if it's helpful I can port this to windows or linux.

pip:
![Image](https://github.com/user-attachments/assets/5b023892-6b4c-48dd-a72d-8255b6e7b719)

uv:
![Image](https://github.com/user-attachments/assets/a133193f-ed38-475d-ad5f-bf32b7537f85)

See how the uv test fails because the `repro_p2` script is importing `repro_p1`'s newly-printed file, for which there exists no entry in the symlink directory.

Note that if you add `--force-reinstall` to step 5, then step 6 will succeed. But, force-reinstall does a whole lot more than just updating the symlinks, so it's a reasonable short-term workaround but this still feels like a bug.

Thanks for reading!

### Platform

macOS 15.2 arm64

### Version

uv 0.5.23

### Python version

Python 3.13.1

---

_Label `bug` added by @charlesnicholson on 2025-01-24 20:41_

---

_Comment by @charliermarsh on 2025-01-24 20:46_

I'm a bit confused. Adding a file should never require re-installing, assuming an editable install. But I'll have to look at your repro more closely when I have time.

---

_Comment by @charlesnicholson on 2025-01-24 20:50_

From https://setuptools.pypa.io/en/latest/userguide/development_mode.html#strict-editable-installs
![Image](https://github.com/user-attachments/assets/bded00af-58aa-47eb-80d9-a8217765969f)

"... new files *won't* be exposed and the editable installs will try to mimic as much as possible the behavior of a regular install."

I think your claim that adding a file not requiring re-installing is true for the normal "development" mode, but a bunch of tooling like pyright, which powers our in-editor LSPs, require "strict" mode to function, so we use it.


---

_Comment by @charlesnicholson on 2025-01-24 20:51_

There's a bit of discussion about the VSCode side of things here: https://github.com/mne-tools/mne-python/issues/12169

---

_Comment by @zanieb on 2025-01-24 20:52_

Ah, the controversial setuptools mode.

---

_Comment by @charlesnicholson on 2025-01-24 20:54_

@zanieb seriously :(

Is there an alternative / better way to do this? We loved the simplicity of "normal" development mode but then all of our LSP tooling stopped working!

---

_Comment by @zanieb on 2025-01-24 20:56_

Does the "compat" mode not work for you? I think returning to their previous behavior with that flag is a reasonable option. Otherwise, perhaps a different build backend? I wish they had not introduced a new default mode that's so antagonistic to static analysis.

See also https://github.com/pypa/setuptools/issues/3518

---

_Comment by @charlesnicholson on 2025-01-24 20:59_

Thanks for the suggestions! I'm happy to try it- we haven't re-assessed whether compat mode works since we moved to strict a few years ago. Maybe this is a free win :)

(Either way, it still feels like uv should update the symlinks in strict editable re-installs; simply doing it once and then ignoring the symlink directory forever more doesn't seem ideal...)

---

_Comment by @zanieb on 2025-01-24 21:01_

Yeah it seems like there might be something for us to look into here still

---

_Label `compatibility` added by @zanieb on 2025-01-24 21:01_

---

_Comment by @charliermarsh on 2025-01-24 21:04_

Hmm, I don't think we can improve anything here without completely reconsidering how we handle caching? We don't re-build on arbitrary code changes.

---

_Comment by @charlesnicholson on 2025-01-24 21:07_

If this can't change easily, maybe consider having `uv` print a warning when installing, that strict editable installs stop working as soon as you rename a file? That might save a future soul the ~2h that it cost me today :(


---

_Comment by @charlesnicholson on 2025-01-25 17:43_

I don't know the details of how the cache works, but would it be possible to just have uv always redo the symlink work whenever a strict editable install is requested? Maybe it's better that it works but is slow, vs not working at all...



---

_Comment by @zanieb on 2025-01-28 17:53_

I'm not enthused about adding behaviors that only apply to specific build backend settings. 

---

_Comment by @charlesnicholson on 2025-01-28 19:05_

Another thought- just revoke support entirely and explain to users who try to use `strict` mode that `compat` mode is probably what they want? 

It's kind of a bummer that it exists today but is broken, with no warnings.

---

_Comment by @eli-schwartz on 2025-02-16 10:09_

Since this is about a setuptools mode, not a uv mode, I wonder if the ticket should have its title changed.

Also not sure it's actually uv's place to warn you about *setuptools* behaviors, much less attempt to "fix" setuptools, except maybe to the extent that automatically applying some config settings may be apropos for known build backends.

---

_Comment by @charlesnicholson on 2025-02-16 13:45_

Re-running `python -m pip install -e mypackage --config-settings editable_mode=strict` re-invokes setuptools, which updates the sandbox with new symlinks.

Re-running `uv pip install -e mypackage --config-settings editable_mode=strict` does not re-invoke setuptools, so the sandbox is not updated, and additions/renames are not visible from the venv.

The difference here is `uv`; the behavior appears to be that it checks its cache and says "this is up to date" when, because of the sandbox symlinks contents being stale, it is not up to date.

This makes strict editable installs broken with `uv`, since `uv` is deciding that no work needs to be done. The bug here IMO is in `uv` not understanding the rules of strict editable install mode. When I suggest that `uv` warn the user, I'm suggesting that the warning should be about `uv`'s refusal to re-run the setuptools backend. That's not a setuptools problem, AFAICT.

If `uv` intercepted the editable_mode option and refused to run when "strict" was detected, that would be a clear signal to the user that the mode is not supported. If `uv` warned a la "Hey, uv's caching system is incompatible with strict editable installs, things will almost certainly break if you continue down this path but you're an adult so do what you will", that would be a clear signal to the user that they're in trouble. But `uv` is deciding that the backend simply doesn't need to be re-invoked. That's not a setuptools problem; that's `uv` incorrectly assuming that its cache represents reality, and in this case that's not correct.

@eli-schwartz - I'll change the issue title to "uv cache incorrectly skips critical setuptools strict-mode editable reinstalls"; that's clearer- thanks for the suggestion there.

Also: @zanieb - your suggestion that we use "compat" mode solved our problems around pyright / pylance support, so thanks a ton for that! :) I don't have a real-world use case for "strict" mode anymore, so this issue isn't hindering us. Maybe it's just academic?

---

_Renamed from "Strict `uv pip install -e` doesn't update symlinks when files change." to "uv's cache blocks setuptools from updating strict-mode editable install sandbox symlinks" by @charlesnicholson on 2025-02-16 13:45_

---

_Comment by @charlesnicholson on 2025-02-16 13:59_

(I made a minor edit to the top-level description to indicate that the issue is around re-running setuptools)

---
