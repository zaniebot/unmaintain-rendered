```yaml
number: 6970
title: "[Feature Request] allow exporting virtual environments"
type: issue
state: open
author: danielgafni
labels: []
assignees: []
created_at: 2024-09-03T14:45:15Z
updated_at: 2025-11-11T21:37:49Z
url: https://github.com/astral-sh/uv/issues/6970
synced_at: 2026-01-12T15:59:09Z
```

# [Feature Request] allow exporting virtual environments

---

_@danielgafni_

It would be great if `uv` provided a way to export virtual environments to some formats like `.tar.gz`. 

This is useful when preparing environments for [PySpark](https://spark.apache.org/docs/latest/api/python/user_guide/python_packaging.html#using-virtualenv). 

[venv-pack](https://github.com/jcrist/venv-pack) is a tool which does exactly that, but it's painfully slow. I'm sure `uv` could do the same task in a fraction of what it takes for `venv-pack` :crab:  

---

_Comment by @paveldikov on 2024-09-06 15:40_

That's exactly the use case that made me contribute `uv venv --relocatable` :-) all that is missing is the archival/compression which in your case is a simple `tar czf` of the finished venv.

---

_Comment by @danielgafni on 2024-09-06 18:32_

I did notice the relocatable flag and used it. That's really nice! I guess all what's missing is a bunch of ergonomics and docs. 

---

_Comment by @danielgafni on 2024-10-02 11:44_

Hey, getting back to this... 
It doesn't seem like the `--relocatable ` flag is working as expected. 

```shell
❯ uv venv --relocatable
Using CPython 3.11.9
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
❯ cd ./.venv/bin         
❯ ls -lh
total 44K
-rw-r--r-- 1 dan dan 3.7K Oct  2 13:42 activate
-rw-r--r-- 1 dan dan 2.2K Oct  2 13:42 activate.bat
-rw-r--r-- 1 dan dan 2.6K Oct  2 13:42 activate.csh
-rw-r--r-- 1 dan dan 4.2K Oct  2 13:42 activate.fish
-rw-r--r-- 1 dan dan 3.8K Oct  2 13:42 activate.nu
-rw-r--r-- 1 dan dan 2.8K Oct  2 13:42 activate.ps1
-rw-r--r-- 1 dan dan 2.4K Oct  2 13:42 activate_this.py
-rw-r--r-- 1 dan dan 1.7K Oct  2 13:42 deactivate.bat
-rw-r--r-- 1 dan dan 1.2K Oct  2 13:42 pydoc.bat
lrwxrwxrwx 1 dan dan   79 Oct  2 13:42 python -> /home/dan/.local/share/uv/python/cpython-3.11.9-linux-x86_64-gnu/bin/python3.11
lrwxrwxrwx 1 dan dan    6 Oct  2 13:42 python3 -> python
lrwxrwxrwx 1 dan dan    6 Oct  2 13:42 python3.11 -> python
```

Looks like the `python` executable is a symlink (which is not exactly relocatable). 

Is there anything I'm missing here? 

---

_Comment by @bluss on 2024-10-02 15:27_

I think relocatable currently covers this use case: you rename the project folder, like if project1 is now project1.1. I don't think it does more. Without --relocatable, even that breaks (absolute paths in .venv/bin/ executable stubs break)

---

_Comment by @zanieb on 2024-10-02 15:32_

Yeah we don't support bundling the interpreter in the environment yet, but want to.

---

_Comment by @paveldikov on 2024-10-03 10:58_

Yes, that's right, this was not part of the initial use case that drove me to add `--relocatable` -- I was happy to take the base interpreter installation for granted.

This works very well for distribution formats like RPM or DEB (where you can register a dependency on a base Python package) but not at all end-user/desktop installs (whereby a system-wide Python installation is not expected or even desirable).

Some more thoughts on how one could solve the end-user-redistributability use case from first principles:

**On terminology/semantics**: by the time you bundle an interpreter with the virtual environment it's arguably no longer _'virtual'_. Rather what you have is a fully fledged Python installation (could indeed be one coming from `uv python install`), that you are augmenting with further packages installed _at system-site-packages-level_.

So, this is mostly already achievable using stock `uv` functionality -- it's `uv python install` followed by `uv pip install --system`. The only problem here is the relocatable entrypoints. Currently the only user-facing way to generate relocatable entrypoint scripts is by using `uv venv --relocatable`... but in this case there is no venv at all! So there is no way to propagate the `relocatable: true` argument all the way down to the wheel installer.

A few options seem to naturally come out of this:
* `uv package` command that does a fully opinionated one-stop-shop thing.
    - Pros: simplest UX.
    - Cons: this would probably be strongly-coupled to `uv python install`, which is currently really enterprise-unfriendly (in a lot of regulated enterprises, downloading arbitrary Python binaries direct from GitHub is a fireable offence)
* `uv pip install --system` could imply `relocatable: true`.
    - Pros: this lines up with Python binary distributions already being relocatable -- if `python{,.exe}` is relocatable then it makes a lot of sense its sibling/dependent scripts to also be relocatable.
    - Cons: is this an implicit behaviour we are comfortable with? Relocatable entrypoints do have a few corner cases e.g. #6489 which means that there is a minor loss of compatibility.
* Add `--relocatable` param to `uv pip install`
    - Pros: low level control
    - Cons: fragmented UX?
* Deliberately ignore the 'virtual' semantics and implement a mode of `uv venv` that actually completely clones the interpreter in its entirety.
    - Pros: UX remains consistent. And we maintain a decent level of low-level control for power users.
    - Cons: can we live with this breach of semantics?

---

_Comment by @chrisrodrigue on 2024-10-03 22:37_

Thanks for brainstorming this @paveldikov. I think this would be a killer feature especially for offline environment deployments. I expect `--relocatable` allows one to move a venv anywhere on the file system and still function, is that correct?

 Symlinks are somewhat tricky on Windows since they can require elevation to make them (unlike hardlinks) which isn't usually available in enterprise environments, so hopefully `--relocatable` doesn't depend on symlinks for the Windows implementation. This would have implications for Windows because hardlinks are only valid within the same file system, unlike symlinks which can span file systems since they just point to the other file.

I think `--with-python` and `--with-uv` or `--with uv,python` or `--with uv --with python` would be awesome options to add for `uv venv` since these flags would let someone move the venv onto another computer that might not have python or uv installed at all. This is somewhat the inverse of options that `virtualenv` provides such as `--no-pip`, `--no-wheel` and `--no-setuptools`. [See here](https://virtualenv.pypa.io/en/latest/cli_interface.html).

Maybe there could also be another flag so that one can target additional platforms that the venv should function in. By default, it targets the current platform, but it would be nice to create a venv that also grabs the wheels necessary for a different platform so that the venv could be moved there and still function. I think it would be a powerful feature, but I don't know what the API would look like, maybe `--platform current,win_amd64,win32,macosx_10_12_x86_64,...` or `--platform win --platform macosx` with `--platform current` being the implied default? If many platforms are specified, I would also expect it to apply to other options such as `--with-python` and `--with-uv` (i.e., the `python` and `uv` binaries for all specified platforms should be in the venv too). 

 `--platform all` would be nice too, for a universal (and gigantic) venv. Cosmopolitan python anyone?


---

_Comment by @chrisrodrigue on 2024-10-03 22:50_

> Cons: this would probably be strongly-coupled to uv python install, which is currently really enterprise-unfriendly (in a lot of regulated enterprises, downloading arbitrary Python binaries direct from GitHub is a fireable offence)

I think it would be cool if uv just bundled the latest python interpreter inside the respective `uv` platform binary for bootstrapping purposes, but not sure how the Astral team would feel about this or if it makes sense without having the rest of the standard library.

> Deliberately ignore the 'virtual' semantics and implement a mode of uv venv that actually completely clones the interpreter in its entirety.

I think this option makes the most sense to me.




---

_Comment by @zanieb on 2024-10-03 23:50_

> downloading arbitrary Python binaries direct from GitHub is a fireable offence

For what it's worth, we're maintaining those binaries — they're not arbitrary. How's uv being installed? Isn't that a binary from GitHub? Note there's a `UV_PYTHON_INSTALL_MIRROR` which allows you to download from an internal mirror of the binaries.

---

_Comment by @zanieb on 2024-10-03 23:51_

I also think we can only create environments with a bundled Python interpreter if the interpreter itself is relocatable, i.e., it would need to be one of our managed Python interpreters not an arbitrary system interpreter.

---

_Comment by @paveldikov on 2024-10-04 08:07_

> > downloading arbitrary Python binaries direct from GitHub is a fireable offence
> 
> For what it's worth, we're maintaining those binaries — they're not arbitrary. How's uv being installed? Isn't that a binary from GitHub? Note there's a `UV_PYTHON_INSTALL_MIRROR` which allows you to download from an internal mirror of the binaries.

Oh, by arbitrary I meant 'unapproved' -- no slight intended, it's just that it doesn't fall under a use case that is known and expressly permitted.

It is quite common for enterprises to deploy scanning proxies of various kinds (cf. #5260) -- downloading direct from github.com or pypi.org is considered a breach of policy as you'd be bypassing said proxy. In fact in most cases you wouldn't even have direct connectivity to github.com or pypi.org as these would be blocked at firewall level.

Actually I was not aware of `UV_PYTHON_INSTALL_MIRROR` -- I am curious, is this also exposed in `uv.toml`? It does actually make a significant difference -- enterprises could use it to benefit from `uv python` in a way that still falls under existing/well-understood control implementations.

---

_Comment by @jlevy on 2025-06-16 21:47_

I only now saw this issue. But I looked at this a few weeks ago as I wanted to make a self-contained, relocatable install directory with a given set of dependencies. It's a bit like a uv-powered PyInstaller:

https://github.com/jlevy/py-app-standalone

I think this covers much this use case (see the readme) so am curious if anyone tries it or finds it useful. I didn't try to handle alternate or enterprise-approved mirrors but am sure it could be adapted to support that better.

---

_Comment by @jlevy on 2025-06-16 21:55_

Thinking a little more, @paveldikov you may find it of interest as it takes the last approach you mention above. By eliminating the symlinks, using relocatable shebangs for all scripts, and also rewriting the macos dylib, you get a fully relocatable standalone directory. If you make the source of the Python install pluggable (e.g. accept a general uv version or a link to an enterprise-approved version) and the package mirror settable, I think it could give you everything you want?

And @zanieb perhaps you already know this, but it seemed like it was only a small patch on macos to make the uv python install relocatable.

---

_Comment by @jeffmelville on 2025-11-11 21:37_

To hop on this thread, a Python-only uv variant of [pixi-pack](https://pixi.sh/v0.45.0/deployment/pixi_pack/) (or the old conda-pack) would be a great model for the offline deployments I do

---
