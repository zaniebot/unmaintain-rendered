---
number: 11205
title: uv --no-dev --group dev does not install dev group
type: issue
state: closed
author: gaborbernat
labels:
  - bug
assignees: []
created_at: 2025-02-03T23:12:31Z
updated_at: 2025-02-07T15:41:01Z
url: https://github.com/astral-sh/uv/issues/11205
synced_at: 2026-01-10T01:25:02Z
---

# uv --no-dev --group dev does not install dev group

---

_Issue opened by @gaborbernat on 2025-02-03 23:12_

### Summary

Reproducible:

https://github.com/koxudaxi/datamodel-code-generator/actions/runs/13119193488/job/36600851153
https://github.com/koxudaxi/datamodel-code-generator/blob/main/pyproject.toml#L59-L77

does not install pytest:
```
dev: uv-sync> uv sync --frozen --python-preference system --no-dev --group dev -p \
/home/runner/.local/share/uv/tools/tox/bin/python
```
### Platform

any

### Version

0.5.26

### Python version

any

---

_Label `bug` added by @gaborbernat on 2025-02-03 23:12_

---

_Assigned to @Gankra by @Gankra on 2025-02-03 23:15_

---

_Comment by @zanieb on 2025-02-03 23:17_

```
❯ uv init example
Initialized project `example` at `/Users/zb/workspace/uv/example`
❯ cd example
❯ uv add --dev anyio
Using CPython 3.12.6
Creating virtual environment at: .venv
Resolved 5 packages in 100ms
Installed 4 packages in 4ms
 + anyio==4.8.0
 + idna==3.10
 + sniffio==1.3.1
 + typing-extensions==4.12.2
❯ uv sync --no-dev --group dev
Resolved 5 packages in 0.79ms
Uninstalled 4 packages in 7ms
 - anyio==4.8.0
 - idna==3.10
 - sniffio==1.3.1
 - typing-extensions==4.12.2
```

---

_Comment by @charliermarsh on 2025-02-03 23:57_

I thought this was the intended behavior -- my understanding was that "excludes" always beat "includes".

---

_Comment by @zanieb on 2025-02-03 23:59_

I'm not sure that makes sense as a general behavior, like `--no-default-groups --group dev` should include `dev`, right?

I'm guessing it's painful to implement this as "last wins", unfortunately.

---

_Comment by @charliermarsh on 2025-02-04 00:02_

No, but if a group is explicitly excluded, it seems odd to ever include it IMO?

---

_Comment by @gaborbernat on 2025-02-04 00:02_

My understanding was the `--no-dev` only was related to the old uv dependency groups, not PEP-735. It's how it was working. If we want to change this, we should raise invalid flags when discovering this.

---

_Comment by @charliermarsh on 2025-02-04 00:03_

No, `--no-dev` has always applied equally to the non-PEP 735 group and a PEP 735 group named "dev".

---

_Comment by @zanieb on 2025-02-04 00:04_

Conceptually, `--no-dev` is just an alias for `--no-group dev`.

---

_Comment by @charliermarsh on 2025-02-04 00:04_

(At least, as far as I know. Maybe it changed accidentally at some point? But I don't believe so.)

---

_Comment by @zanieb on 2025-02-04 00:04_

> No, but if a group is explicitly excluded, it seems odd to ever include it IMO?

I agree with this, yeah. Unless we go the route of https://github.com/astral-sh/uv/pull/3259

---

_Comment by @gaborbernat on 2025-02-04 00:09_

> No, `--no-dev` has always applied equally to the non-PEP 735 group and a PEP 735 group named "dev".

So when both is present how does one choose to only install PEP or only non PEP?

---

_Comment by @zanieb on 2025-02-04 00:10_

>  only install PEP or only non PEP?

These groups are combined — they're not intended to be mixed or partially selected.

---

_Comment by @gaborbernat on 2025-02-04 00:14_

That is the two lists are concatanated, no matter what?

---

_Comment by @gaborbernat on 2025-02-04 00:19_

Help me understand then what this flag is supposed to do. `--no-dev` that is. Does it remove dev from the dependency group? That is if one extends the default, here https://docs.astral.sh/uv/concepts/projects/dependencies/#default-groups? Does only remove dev but keeps rest? Is it a flag to disable the default group, but oddly named?

---

_Comment by @Gankra on 2025-02-04 00:43_

--no-dev is Extremely hardcoded to *just* exclude a group called "dev", it doesn't effect the other default groups.

---

_Comment by @gaborbernat on 2025-02-04 01:04_

Perhaps then we need a flag of `--no-default-groups` 😆 

---

_Comment by @Gankra on 2025-02-04 01:10_

Good news for you: `--no-default-groups` already exists, it's just not surfaced as well in the high-level config docs as `--no-dev` is. It only shows up in the generated full CLI docs (e.g. [here](https://docs.astral.sh/uv/reference/cli/#uv-run)).

---

_Comment by @gaborbernat on 2025-02-04 01:12_

I am ok with this behaviour, however I still think if the CLI would hard fail when seeing `--no-dev` with `--group dev` would be a better behaviour than silently picking a winner.

---

_Comment by @gaborbernat on 2025-02-04 01:55_

> --no-default-groups

So question:

```
uv sync --no-default-groups --group dev
```

Will it install dev?

---

_Comment by @zanieb on 2025-02-04 02:00_

Yes.

---

_Referenced in [tox-dev/tox-uv#165](../../tox-dev/tox-uv/pulls/165.md) on 2025-02-04 02:05_

---

_Comment by @Gankra on 2025-02-05 20:45_

Now that I've properly overhauled this subsystem (in #11224), I can give some more concrete insight on how this is extra fun/weird.

We actually special-case `--no-dev` and `--dev` to positionally override eachother, so `--no-dev --dev` enables dev while `--dev --no-dev` disables it. This is almost certainly legacy behaviour from when these were the only flags. Meanwhile `--group dev` does not participate in this overriding (and I don't think it should, if anything I'd want to drop the special case for `--dev`, but that's a breaking change).

In the new impl we could definitely warn if you enable something only to disable it, but I'm not confident that this won't be yelling at people for doing something we explicitly support. At a minimum I'm going to open a PR to improve the docs a bit.

---

_Referenced in [astral-sh/uv#11284](../../astral-sh/uv/pulls/11284.md) on 2025-02-06 16:31_

---

_Closed by @Gankra on 2025-02-07 15:41_

---

_Closed by @Gankra on 2025-02-07 15:41_

---

_Referenced in [astral-sh/uv#3248](../../astral-sh/uv/issues/3248.md) on 2025-03-03 03:26_

---
