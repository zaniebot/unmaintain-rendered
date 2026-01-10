```yaml
number: 11133
title: Duplicate packages with conflicting extras
type: issue
state: closed
author: ElliottKasoar
labels:
  - bug
assignees: []
created_at: 2025-01-31T13:30:43Z
updated_at: 2025-02-10T14:17:33Z
url: https://github.com/astral-sh/uv/issues/11133
synced_at: 2026-01-10T03:50:31Z
```

# Duplicate packages with conflicting extras

---

_Issue opened by @ElliottKasoar on 2025-01-31 13:30_

### Summary

We have three dependencies, which all require PyTorch:

- `mace-torch`, which requires `torch>=1.12` in their [tagged release](https://github.com/ACEsuit/mace/blob/v0.3.9/setup.cfg#L17)
- `chgnet`, which requires `torch>=2.4.1` in their [tagged release](https://github.com/CederGroupHub/chgnet/blob/v0.4.0/pyproject.toml#L15)
- `matgl` (labelled `m3nget`), which requires any `torch` in their [tagged release](https://github.com/materialsvirtuallab/matgl/blob/v1.1.3/pyproject.toml#L56), but in most cases we need to require `torch<=2.2.1` (and `"dgl==2.1"`)

A (relatively) minimal pyproject.toml for this, with the conflict between `chgnet` and `m3gnet` specified:

```
[project]
name = "test"
version = "0.0.1"
requires-python = ">=3.10"
dependencies = [
    "mace-torch==0.3.9",
]

[project.optional-dependencies]
chgnet = [
    "chgnet == 0.4.0",
]

m3gnet = [
    "matgl == 1.1.3",
    "torch == 2.2.1",
    "torchdata == 0.7.1",
]

[tool.uv]
constraint-dependencies = [
    "dgl==2.1",
    "torch<2.6",
]
conflicts = [
    [
      { extra = "chgnet" },
      { extra = "m3gnet" },
    ],
]
```

However, if I run `uv sync -p 312` with this, followed by `uv pip list`, two versions of `torch` are installed:

```
torch                2.2.1
torch                2.5.1
```

Strangely, we swap out `matgl` for `alignn` (which only explicitly depends on `torch>=2.0.0` in their [tagged release](https://github.com/usnistgov/alignn/blob/v2024.5.27/setup.py#L21)):

```
[project]
name = "test"
version = "0.0.1"
requires-python = ">=3.10"
dependencies = [
    "mace-torch==0.3.9",
]

[project.optional-dependencies]
chgnet = [
    "chgnet == 0.4.0",
]

alignn = [
    "alignn == 2024.5.27",
    "torch == 2.2.1",
    "torchdata == 0.7.1",
]

[tool.uv]
constraint-dependencies = [
    "dgl==2.1",
    "torch<2.6",
]
conflicts = [
    [
      { extra = "chgnet" },
      { extra = "alignn" },
    ],
]
```

then, when running `uv sync -p 312`, only torch 2.5.1 is installed.

May be related to https://github.com/astral-sh/uv/issues/10985.

### Platform

macOS 15.2 arm64

### Version

0.5.26

### Python version

3.12.8

---

_Label `bug` added by @ElliottKasoar on 2025-01-31 13:30_

---

_Assigned to @BurntSushi by @konstin on 2025-01-31 14:11_

---

_Comment by @BurntSushi on 2025-01-31 14:33_

I think the intended behavior here is that only `torch==2.5.1` is installed, right?

If I look at the lock file, I see this for `torchmetrics`:

```toml
[[package]]
name = "torchmetrics"
version = "1.6.1"
source = { registry = "https://pypi.org/simple" }
dependencies = [
    { name = "lightning-utilities" },
    { name = "numpy" },
    { name = "packaging" },
    { name = "torch", version = "2.2.1", source = { registry = "https://pypi.org/simple" } },
    { name = "torch", version = "2.5.1", source = { registry = "https://pypi.org/simple" }, marker = "extra == 'extra-4-test-chgnet' or extra != 'extra-4-test-m3gnet'" },
]
```

Notice that both `2.2.1` and `2.5.1` will be used when there are no extras enabled (or even just when `chgnet` is enabled). Other dependencies on `torch` in the lock file look like this:

```toml
dependencies = [
    { name = "torch", version = "2.2.1", source = { registry = "https://pypi.org/simple" }, marker = "extra == 'extra-4-test-m3gnet'" },
    { name = "torch", version = "2.5.1", source = { registry = "https://pypi.org/simple" }, marker = "extra == 'extra-4-test-chgnet' or extra != 'extra-4-test-m3gnet'" },
]
```

Which is correct. Those markers are disjoint.

So I think this is a bug in resolution or possibly conflict marker simplification. This requires more investigation.

---

_Comment by @ElliottKasoar on 2025-01-31 15:01_

Thanks for the quick response!

> I think the intended behavior here is that only `torch==2.5.1` is installed, right?

Yes, that was my expectation.

This isn't directly related to the bug, but can I also ask a related question: is there a convenient way of specifying conflicting sets of extras?

I know conflict groups are possible, but in this case, I'd like to be able to `m3gnet` and `alignn` both conflict with `chgnet` and potentially others, while still being install each individually as an extra.

Is it currently necessary to specify each conflicting pair?

---

_Comment by @BurntSushi on 2025-01-31 15:45_

Your sets of conflicts can be of arbitrary length. If I'm understanding you correctly, then I think you just want this:

```toml
conflicts = [
    [
      { extra = "chgnet" },
      { extra = "alignn" },
      { extra = "m3gnet" },
    ],
]
```

And that will make all of them conflict with one another.

---

_Comment by @ElliottKasoar on 2025-01-31 15:58_

> Your sets of conflicts can be of arbitrary length. If I'm understanding you correctly, then I think you just want this:
> 
> conflicts = [
>     [
>       { extra = "chgnet" },
>       { extra = "alignn" },
>       { extra = "m3gnet" },
>     ],
> ]
> And that will make all of them conflict with one another.

Sorry, no I wasn't quite clear enough. For the above three, I would like to say that `chgnet` conflicts with both `alignn` and `m3gnet`, but `alignn` and `m3gnet` do not conflict with each other (we may want to install both).

More generally, I would like one set of non-conflicting extras that are all able to use something like `torch==2.5.1`, and another non-conflicting set which still require `torch==2.2.1`, while retaining the ability to install each individually.

---

_Comment by @BurntSushi on 2025-01-31 16:07_

Yeah you have to explicitly specify the conflicts. Like this:

```toml
conflicts = [
    [
      { extra = "chgnet" },
      { extra = "alignn" },
    ],
    [
      { extra = "chgnet" },
      { extra = "m3gnet" },
    ],
]
```

---

_Comment by @ElliottKasoar on 2025-01-31 16:17_

> Yeah you have to explicitly specify the conflicts. Like this:
> 
> conflicts = [
>     [
>       { extra = "chgnet" },
>       { extra = "alignn" },
>     ],
>     [
>       { extra = "chgnet" },
>       { extra = "m3gnet" },
>     ],
> ]

Ok yep, thanks for clarifying! The number of pairs grows quite quickly as the two sets grow, but it's also quite a niche problem.

---

_Comment by @BurntSushi on 2025-01-31 16:20_

Yeah if you want to open a new issue with a more complete view of what you're trying to do, and also what your full `pyproject.toml` looks like with all the conflicts, then I think that would let us consider the UX here a bit more holistically. There is definitely somewhat of an assumption that the number of conflicts is overall small, and I think that influenced the current UX. Your specific issue might be niche, but it might be common enough to consider improving the UX. I'm honestly not sure.

One thing to consider here is that every conflict you define leads to a fork in the resolver. So like, if you have a crazy number of conflicts, there is definitely a potential there for slower resolutions.

---

_Closed by @BurntSushi on 2025-02-10 14:17_

---
