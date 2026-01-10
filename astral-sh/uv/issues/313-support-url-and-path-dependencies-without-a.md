```yaml
number: 313
title: Support URL and path dependencies without a package name
type: issue
state: closed
author: charliermarsh
labels:
  - enhancement
assignees: []
created_at: 2023-11-03T18:38:23Z
updated_at: 2024-03-22T11:58:08Z
url: https://github.com/astral-sh/uv/issues/313
synced_at: 2026-01-10T05:40:31Z
```

# Support URL and path dependencies without a package name

---

_Issue opened by @charliermarsh on 2023-11-03 18:38_

Apparently this works with pip?

```
git+https://github.com/pallets/flask.git@refs/pull/5313/head
```

As opposed to:
```
flask @ git+https://github.com/pallets/flask.git@refs/pull/5313/head
```

We could decide not to support these. I don't know if they're spec-compliant.


---

_Label `enhancement` added by @charliermarsh on 2023-11-03 18:38_

---

_Comment by @charliermarsh on 2023-11-03 18:38_

\cc @konstin - do you know if these are spec-compliant? Or legacy?

---

_Comment by @konstin on 2023-11-03 18:44_

They are iirc not part of PEP 621 and PEP 508, but they are supported in many places (`pip install https://...` makes sense)

---

_Comment by @charliermarsh on 2023-11-03 18:47_

Sadly, adds a lot of complexity.

---

_Comment by @zanieb on 2023-11-04 04:07_

Oh this is the syntax I use every time I install a package from `git` using `pip` 😬

I'm not sure if we really need to support it in a requirements file? Unless it's common in the wild. Generally it seems reasonable to have a nice error suggesting inclusion of the package name.

---

_Comment by @charliermarsh on 2023-11-04 13:04_

If we support it _at all_, then it’s not too hard to support it in a requirements file too.

---

_Comment by @charliermarsh on 2023-11-04 13:23_

Although, I guess it lets us skip supporting it in the _parser_. Anyway, we probably do need to support this long-term if it’s allowed in pyproject.toml.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-04 13:33_

---

_Added to milestone `Feature complete` by @charliermarsh on 2023-11-04 13:33_

---

_Comment by @charliermarsh on 2023-11-04 13:42_

But yeah we should support them :)

---

_Renamed from "Support (or decide not to support) URL dependencies without a package name" to "Support URL dependencies without a package name" by @charliermarsh on 2023-11-04 13:42_

---

_Comment by @charliermarsh on 2023-11-05 15:02_

@konstin - Do you think we should make `name` optional on `Requirement`, or convert `Requirement` to an enum? Names can only be omitted when the version specific is a URL. Similarly, you can't have extras without a name. Feels like all of this could be encoded in the type system.

---

_Comment by @konstin on 2023-11-06 09:20_

We should add a `RequirementsTxtRequirement` which allows not having a name (or where name can be a url), i'd keep PEP 508 `Requirement` as-is.

---

_Unassigned @charliermarsh by @charliermarsh on 2023-11-06 21:06_

---

_Comment by @charliermarsh on 2023-12-13 06:32_

I'll give this another try.

---

_Comment by @konstin on 2023-12-13 14:16_

Would you mind waiting until #587 is merged? Editables will profit from this, but doing it now would clash badly.

---

_Comment by @charliermarsh on 2023-12-13 14:43_

Will do!

---

_Comment by @sfc-gh-jcarroll on 2024-02-16 21:10_

It seems like this is also an issue for installing a local whl file?

```sh
% uv pip install ./streamlit-1.31.1-py2.py3-none-any.whl 
error: Failed to parse `./streamlit-1.31.1-py2.py3-none-any.whl`
  Caused by: Expected package name starting with an alphanumeric character, found '.'
./streamlit-1.31.1-py2.py3-none-any.whl
^

% uv pip install "streamlit @ ./streamlit-1.31.1-py2.py3-none-any.whl"
Resolved 40 packages in 680ms
Downloaded 40 packages in 1.74s
Installed 40 packages in 97ms
...
```

BTW `uv` seems awesome!! Thank you!!!

---

_Comment by @charliermarsh on 2024-02-16 21:12_

Yup, all the same issue! Although we can probably support local wheels "trivially" since the file name is required to be encoded in the wheel.

---

_Comment by @edgarrmondragon on 2024-02-17 05:43_

This is probably the biggest blocker to experimenting with uv [in Meltano](https://github.com/meltano/meltano/blob/c47653b75be5be03441285f64a8d23d7fddd7a41/src/meltano/core/venv_service.py) since dependencies of the type `git+https://...` are very widely used by plugins.

---

_Renamed from "Support URL dependencies without a package name" to "Support URL and path dependencies without a package name" by @charliermarsh on 2024-02-17 14:19_

---

_Comment by @charliermarsh on 2024-02-17 14:23_

👍 I want to support this, requires a bunch of internal refactors but definitely doable.

---

_Comment by @yyolk on 2024-02-21 01:30_

Hopefully not to add to the confusion - I've been running scenarios against integrating with [`pdm`](https://github.com/pdm-project/pdm).

While investigating my own feature request, pdm-project/pdm#2641 I discovered `pdm` is handling `requirements.txt`-style exports in a way that falls back to pip's behavior...and works with `uv` if I remove the `#egg=` fragments. 

Here's some quirks I was able to discover. Being able to do `uv v --seed` for running through was very helpful, I use the `.venv/bin/pip` for wherever `pip install` is used. I believe these quirks would be universal, anywhere there's a python package with a `pyproject.toml`. 

I'm using the [pdm-example-monorepo](https://github.com/pdm-project/pdm-example-monorepo) below in the latest Python Docker image under a freshly made user in their home directory.


**When in a git directory:**

`pip` will end up resolving an editable install (`pip install -e 'file://'$PWD'relative/path'`) of a local path that's inside a git directory differently than one that is not. 

When inside a git directory, pip will include an `#egg=` fragment along with another fragment param for its relative directory, `subdirectory`.

e.g.;
```bash
# replace .venv
$ uv v --seed
# install the package as editable with relative path
$ .venv/bin/pip install -e 'file://'$PWD'packages/pkg-second'

# ...

# show pip installation
$ .venv/bin/pip freeze | grep pkg-core

-e git+https://github.com/pdm-project/pdm-example-monorepo@b85261555ea063e68eaf744e225804149c27e64f#egg=pkg_core&subdirectory=packages/pkg-core
```

**When it's not a git directory, and it's still an editable install:**

```bash
# replace .venv
$ uv v --seed
# get rid of .git, so it's not detected as a git repository
$ mv .git .gitold
# install the package as editable with relative installation, again
$ .venv/bin/pip install -e 'file://'$PWD'/packages/pkg-core'

# ...

# show pip installation
$ .venv/bin/pip freeze | grep pkg-core

# Editable install with no version control (pkg-core==0.1.0+editable)
-e /home/python/pdm-example-monorepo/packages/pkg-core
```


* * *
**Non-editable installs for comparison**

There's also this way that will lend itself to the next example - `pkg-name @ file:///...`, which cannot be installed as editable, regardless of git directory or not:

```bash
# replace .venv
$ uv v --seed
# install relative package, non-editable, pkg @ file://... way
$ .venv/bin/pip install 'pkg-core @ file://'$PWD'/packages/pkg-core'

# ...

# freezes the same way whether in a git repository or not
$ venv/bin/pip freeze | grep pkg-core

pkg-core @ file:///home/python/pdm-example-monorepo/packages/pkg-core
```

And lastly - `pip install 'file://...` will result in the same `package-name @ file://...` expansion regardless if it's in a git directory or not 😅 

```bash
# replace .venv
$ uv v --seed
# install non-editable, just 'file://...' 
$ .venv/bin/pip install 'file://'$PWD'/packages/pkg-core'

# ...

# freezes the same way whether in git repository or not
$ .venv/bin/pip freeze | grep pkg-core
pkg-core @ file:///home/python/pdm-example-monorepo/packages/pkg-core
```

---

_Comment by @kkpattern on 2024-02-22 07:30_

We have a similar use case; we want to install a package in a local directory without `editable.`

We have several Python packages in a mono repo. Sometimes we need to create a python `venv`, install those packages into the `venv`, then pack the `venv` and distribute it to CI or production environment.

So we want to be able to install a local package like this:

```bash
uv pip install ./path/to/package/dir
```

We don't want to use `-e` here because the venv will be redistributed.



---

_Comment by @charliermarsh on 2024-02-22 13:35_

@kkpattern - that _is_ supported, but you need to include the package name, like “ uv pip install ‘foo @ ./path/to/package/dir’”. We’ll lift this restriction in time, I’m mostly focused on correctness issues right now.

---

_Comment by @kkpattern on 2024-02-22 13:41_

@charliermarsh  Yes. We figured that out. We have already switched to uv on our main branch. Massive speed up. Thanks for this awesome tool. (And ruff too!)

---

_Comment by @shekharidentv on 2024-02-22 15:40_

I could able to install the package as mentioned above ways. but 
```uv pip sync requirements.txt``` is failing.  I am running `uv pip freeze > requirements.txt ` to generate the txt file.

```
error: Couldn't parse requirement in `requirements.txt` at position 384
  Caused by: Trailing `(from https://<url>/<package>-0.7.1-py3-none-any.whl)` is not allowed
```

---

_Comment by @charliermarsh on 2024-02-22 16:12_

@shekharidentv - do you mind opening a new issue for this? We may want to strip those URLs from freeze.

---

_Comment by @jkgenser on 2024-03-09 18:29_

👍  for this feature. I'm liking UV so much that I'm using `pip install path/to/my.whl` (which is slow) and then `uv pip install` after.

---

_Comment by @zanieb on 2024-03-09 19:06_

@jkgenser note you _can_ just provide a package name e.g. `uv pip install "foo@path/to/my.whl"` in the meantime.


---

_Comment by @jkgenser on 2024-03-10 14:46_

> @jkgenser note you _can_ just provide a package name e.g. `uv pip install "foo@path/to/my.whl"` in the meantime.

Wow amazing thank you that worked! I was having trouble because I saw other examples in github issues that referred to `"package @ path/to/my.whl"` and it wasn't working me. However removing the spaces was what was needed.

---

_Comment by @aliencaocao on 2024-03-10 16:44_

How to install optional deps?
```uv pip install xxx@./xxx[yy]``` dont work and says Distribution not found

---

_Comment by @akx on 2024-03-10 16:45_

> How to install optional deps? 
> `uv pip install xxx@./xxx[yy]` dont work 

I'd guess `uv pip install package[extra]@location`.


---

_Comment by @jeremander on 2024-03-15 21:46_

Loving `uv` so far!

Will supporting this also allow the very basic `uv pip install .` (non-editable) to work?

---

_Comment by @charliermarsh on 2024-03-16 13:17_

Yup!

---

_Comment by @charliermarsh on 2024-03-18 18:16_

Publicly committing to this one.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-18 18:16_

---

_Comment by @dadokkio on 2024-03-18 18:21_

I've a similar issue with a requirements.txt file that contains a package installed as:

`-e git+https://github.com/xxx/package.git@commit_id#egg=package package`

at the moment `uv pip install` fails because -e seems to work only for local files 
```error: Unsupported URL (expected a `file://` scheme) in requirements.txt ```

Will this syntax be supported? There is an alternative way to install a specific package in requirements in current folder from a github repo?

Thanks



---

_Comment by @charliermarsh on 2024-03-18 18:27_

You can do `package @ git+https://github.com/xxx/package.git@commit_id`, right?

---

_Comment by @dadokkio on 2024-03-18 19:05_

Yes, but in that case it'll be installed under python path and not under current folder.. also with standard pip the behavior is different 

---

_Comment by @charliermarsh on 2024-03-18 19:08_

We don't support editable installs for Git and URL dependencies. I'd suggest just cloning the repo and installing it as a file-based editable install (`-e ./package`).

---

_Comment by @charliermarsh on 2024-03-20 23:44_

Okay, this is now in review.

---

_Comment by @charliermarsh on 2024-03-22 03:10_

These have all merged. Will be supported in the next release.

---

_Closed by @charliermarsh on 2024-03-22 03:10_

---

_Comment by @franciscoafonsoo on 2024-03-22 11:58_

Thank you so much for making this, makes a world of a difference !

---
