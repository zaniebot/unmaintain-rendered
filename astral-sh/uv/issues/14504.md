```yaml
number: 14504
title: Support Mercurial like pip
type: issue
state: closed
author: paugier
labels:
  - enhancement
  - wish
assignees: []
created_at: 2025-07-08T10:25:46Z
updated_at: 2025-07-09T10:50:17Z
url: https://github.com/astral-sh/uv/issues/14504
synced_at: 2026-01-10T03:32:45Z
```

# Support Mercurial like pip

---

_Issue opened by @paugier on 2025-07-08 10:25_

### Summary

With pip, one can do:

```sh
$ python3 -m venv tmp_venv_fluiddyn
$ . tmp_venv_fluiddyn/bin/activate
$ pip install fluiddyn@hg+https://foss.heptapod.net/fluiddyn/fluiddyn
Collecting fluiddyn@ hg+https://foss.heptapod.net/fluiddyn/fluiddyn
  Cloning hg https://foss.heptapod.net/fluiddyn/fluiddyn to ./pip-install-zvd68wo9/fluiddyn_02c3874ec9954bfe9db47e720d68c9fa
  Running command hg clone --noupdate --quiet https://foss.heptapod.net/fluiddyn/fluiddyn /tmp/pip-install-zvd68wo9/fluiddyn_02c3874ec9954bfe9db47e720d68c9fa
  Running command hg update --quiet
[...]
```

Unfortunately, this gives with UV:

```sh
$ uv venv tmp_venv_fluiddyn_uv
Using CPython 3.13.2
Creating virtual environment at: tmp_venv_fluiddyn_uv
Activate with: source tmp_venv_fluiddyn_uv/bin/activate
$ . tmp_venv_fluiddyn_uv/bin/activate
$ uv pip install fluiddyn@hg+https://foss.heptapod.net/fluiddyn/fluiddyn
error: Failed to parse: `fluiddyn@hg+https://foss.heptapod.net/fluiddyn/fluiddyn`
  Caused by: Unsupported URL prefix `hg` in URL: `hg+https://foss.heptapod.net/fluiddyn/fluiddyn` (Mercurial is not supported)
fluiddyn@hg+https://foss.heptapod.net/fluiddyn/fluiddyn
         ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
```

I was wondering if Mercurial could be supported which would be convenient for few Python projects.

[Mercurial](https://www.mercurial-scm.org) is still active with a growing part written in Rust (https://foss.heptapod.net/mercurial/mercurial-devel/-/tree/branch/default/rust).

I realize that it's easier from the UV point of view to just say "just use Git" however, 

- Diversity can also be useful for software.
- It could be quite easy to support Mercurial (just following what is done in Pip), with very stable code so very little maintenance (the needed Mercurial commands will never change).
- Since Mercurial is partly implemented in Rust, I guess Mercurial developers (cc @Alphare) could propose a PR.

Therefore, I'd like to ask if such PR implementing Mercurial support could be considered?

### Example

_No response_

---

_Label `enhancement` added by @paugier on 2025-07-08 10:25_

---

_Label `wish` added by @charliermarsh on 2025-07-08 11:40_

---

_Comment by @charliermarsh on 2025-07-08 11:41_

It could make sense to support it, it's just a matter of prioritization and I'd want to see more user demand before investing in it. Beyond the initial implementation, it's also a lot of work to maintain support over time.


---

_Comment by @edmorley on 2025-07-08 19:37_

If it helps as as data point, we (Heroku) removed Mercurial from our build environments (when adding our Ubuntu 24.04 based images last year) due to low usage, and have currently had zero requests to add it back.

Whilst there will still be the occasional Python library using Mercurial, most of our users will consume the sdist or wheels from an index rather than installing from a VCS URL.

As a workaround, for most hosted VCS providers, they typically make the source available via a tarball of the branch too, eg in this case:
`https://foss.heptapod.net/fluiddyn/fluiddyn/-/archive/branch/default/fluiddyn-branch-default.tar.gz`

---

_Comment by @paugier on 2025-07-09 10:50_

I didn't realize that 

```sh
uv pip install fluiddyn@https://foss.heptapod.net/fluiddyn/fluiddyn/-/archive/branch/default/fluiddyn-branch-default.tar.gz
```

would work, which is a very good alternative.

Note that it also works with Mercurial topic (feature branches), for example `https://foss.heptapod.net/fluiddyn/fluiddyn/-/archive/topic/default/dct-dst/fluiddyn-topic-default-dct-dst.tar.gz` or tags, like `https://foss.heptapod.net/fluiddyn/fluiddyn/-/archive/0.8.0/fluiddyn-0.8.0.tar.gz`.

I guess then there is no need for this feature.

Aside note: in the long term, having just one solution for versioning (only Git) is very bad for user experience. A bit the same as if Python users have no other solution than `pip`.

---

_Closed by @paugier on 2025-07-09 10:50_

---
