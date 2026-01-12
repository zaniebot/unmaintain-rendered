```yaml
number: 6296
title: "Allow extras in `uvx` command"
type: issue
state: closed
author: dimaqq
labels:
  - good first issue
  - cli
assignees: []
created_at: 2024-08-21T04:26:43Z
updated_at: 2025-02-12T00:06:41Z
url: https://github.com/astral-sh/uv/issues/6296
synced_at: 2026-01-12T15:59:02Z
```

# Allow extras in `uvx` command

---

_@dimaqq_

How to specify package extras with `uvx`?

Or, in other words, how to run `httpx` with `uvx`?

```command
> uvx --version
uv-tool-uvx 0.3.0 (Homebrew 2024-08-20)
> uvx httpx
The httpx command line client could not run because the required dependencies were not installed.
Make sure you've installed everything with: pip install 'httpx[cli]'
> uvx httpx[cli]
error: Not a valid package or extra name: "httpx[cli]". Names must start and end with a letter or digit and may only contain -, _, ., and alphanumeric characters.
```

---

_Comment by @Eyal-Shalev on 2024-08-21 04:38_

Try `uvx 'httpx[cli]'`

---

_Comment by @konstin on 2024-08-21 06:23_

`uvx --with 'httpx[cli]' httpx` works, but this is something we should support better.

---

_Renamed from "uvx extras, or how to httpx?" to "Extras in uvx" by @konstin on 2024-08-21 07:49_

---

_Comment by @charliermarsh on 2024-08-21 13:17_

Yeah I think `httpx[cli]` _could_ be supported (install `httpx[cli]`, then run the `httpx` entrypoint).

---

_Label `wish` added by @charliermarsh on 2024-08-21 13:17_

---

_Label `cli` added by @charliermarsh on 2024-08-21 13:17_

---

_Renamed from "Extras in uvx" to "Allow extras in `uvx` command" by @charliermarsh on 2024-08-21 13:17_

---

_Comment by @henryiii on 2024-08-23 20:21_

pipx run does this; you can specify version specifiers and extras. When making the package name, it runs them through packaging and just picks the package name out.

---

_Label `good first issue` added by @zanieb on 2024-08-23 20:28_

---

_Label `wish` removed by @zanieb on 2024-08-23 20:28_

---

_Comment by @zanieb on 2024-08-23 20:29_

I'm not opposed to supporting this in the short-term. If there's a reason `uvx httpx[cli]` shouldn't work lmk.

---

_Comment by @charliermarsh on 2024-09-16 01:34_

We need to decide on the following cases:

- `uvx httpx==0.6.0`
- `uvx httpx[cli]`
- `uvx httpx[cli]==0.6.0`
- `uvx httpx[cli]@0.6.0`

---

_Comment by @zanieb on 2024-09-16 02:54_

I think including the extra is arguably just an extension of the name, whereas the `==` syntax is in another category. So I'd basically say just the following are allowed:

- `uvx httpx[cli]`
- `uvx httpx[cli]@0.6.0`

Alternatively, we could support `--extra`, e.g., `uvx --extra cli httpx`? 


---

_Comment by @henryiii on 2024-09-16 03:21_

I know pipx supports the first three. Haven’t seen the forth.

---

_Comment by @henryiii on 2024-09-16 03:29_

Note, the benefits of just parsing the version and extra syntax like normal, and passing on the package name, is that people don't have to learn a new syntax (it works just like pip), and you can do things like `uvx cmake~=3.23.0`, `uvx "https[cli]<0.6"`, `uvx "ruff>=0.6"`,  etc. It's literally just what you'd pass to pip. That's how pipx works, anyway, and I don't think anyone's complained (though I don't follow too closely).

Might make sense to support the extras first, then decide on version stuff later, though.

---

_Comment by @dimaqq on 2024-11-12 04:27_

I think one more usage pattern belongs here:

I wanted to do:

```command
> uvx 'git+https://github.com/tox-dev/tox.git@main#egg=tox' --version
error: Not a valid package or extra name: "git+https://github.com/tox-dev/tox.git@main#egg=tox". Names must start and end with a letter or digit and may only contain -, _, ., and alphanumeric characters.
```

I can work around that with:

```
> uvx --with 'git+https://github.com/tox-dev/tox.git@main#egg=tox' tox --version
 Updated https://github.com/tox-dev/tox.git (343fe92)
4.23.3.dev10+g343fe92 from /Users/dima/Library/Caches/uv/archive-v0/guRzVYUj3CZf_BUckU7HB/lib/python3.11/site-packages/tox/__init__.py
```

---

_Comment by @charliermarsh on 2024-11-23 03:30_

I'm personally okay with supporting `uvx httpx==0.6.0` (and any valid specifier).

---

_Comment by @jaheba on 2025-01-08 09:18_

Looking at this issue, I'm a bit confused why `--from` is not recommended here, as it is the way of specifying what package to actually install?

From my understanding, the specified command of `uvx` has some "magic" to it:

On one hand there is the command to run and then there is the package to install it from.

Usually, these are the same -- hence one can just do `uvx ruff`. However, that is just a shorthand for `uvx --from ruff ruff`.

Similarly, `uvx ruff@0.8` will expand to something akin to `uvx --from ruff~=0.8 ruff`.

Note that if `--from` is specified, there is no attempt to decode the command:

```sh
$ uvx --from "ruff~=0.8" ruff@0.8                                                                                                                                       
The executable `ruff@0.8` was not found.
warning: An executable named `ruff@0.8` is not provided by package `ruff`.
```

---

With that established, the question becomes how to populate `--from`, if only the `command` is specified.

What I understand from the docs, the current allowed formats for `command` are:

* `<package>` (e.g., `ruff`) and
* `<package>@<version>` (e.g., `ruff@latest`).

Now, we might want to add:

* `<package>[<extras>]`, e.g. (`httpx[cli]`)
* `<package>==<version>`, e.g. `httpx==0.6.0`, `httpx~=0.6.0` 
* `<git-url>, e.g. `git+https://github.com/tox-dev/tox.git@main#egg=tox`


The implementation for the first two should be relatively simple

* `uvx <package>[<extras>]` becomes `uvx --from <package>[<extras>] package`
* `uvx <package>==<version>` becomes `uvx --from <package>==<version> package`
* `uvx <package>[<extras>]==<version>` becomes `uvx --from <package>[<extras>]==<version> package`

In the case of the git repository we can't assume the package name (and thus command) from the specified url, rather we would need to install the package first to receive the package (and command) name.

I think there is also an argument to support mixing extras with uv's version requirements (`uvx httpx[cli]@0.6.0`). But in that case, one should not be able to also specify version pip-like requirements (`uvx httpx[cli]==0.6.0@0.6.0`).

---

In my view, `uv` should at least support extras and `==1.2.3` like version specifiers to be as compatible with `pipx` as possible.

Mixing `@<version>` and `==<version>` specifiers should not be allowed.

Support for path-resources could also be nice, but is less important in my eyes. One can argue that is more explicit to require the command name, if it can't be derived from the package-path.


---

_Closed by @charliermarsh on 2025-02-12 00:06_

---

_Closed by @charliermarsh on 2025-02-12 00:06_

---
