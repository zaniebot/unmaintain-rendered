```yaml
number: 7976
title: Add docs for the uv build backend
type: pull_request
state: closed
author: konstin
labels:
  - documentation
  - preview
assignees: []
draft: true
base: main
head: konsti/build-backend3
created_at: 2024-10-07T16:04:47Z
updated_at: 2025-04-25T14:37:04Z
url: https://github.com/astral-sh/uv/pull/7976
synced_at: 2026-01-12T16:08:06Z
```

# Add docs for the uv build backend

---

_@konstin_

Documentation for the uv build backend. To be merged last since we can't hide it in preview.

There are no reference docs yet since there aren't any specific options yet (TBD).

CC @zanieb the guide vs. concept split is odd here, it requires repeating things in the concept section while eliding a lot in the guide section.

---

_Label `documentation` added by @konstin on 2024-10-07 16:04_

---

_Label `preview` added by @konstin on 2024-10-07 16:04_

---

_Renamed from "Add docs for pyproject.toml" to "Add docs for the uv build backend" by @konstin on 2024-10-07 16:04_

---

_@pantheraleo-7 reviewed on 2024-10-16 02:06_

---

_Review comment by @pantheraleo-7 on `docs/concepts/projects.md`:643 on 2024-10-16 02:06_

```suggestion
requires = ["uv>=0.4.18,<0.5"]
```

also, is a minor version bumb be breaking change? maybe it can `requires = ["uv>=0.4.18,<1"]`

---

_@pantheraleo-7 reviewed on 2024-10-16 02:11_

---

_Review comment by @pantheraleo-7 on `docs/guides/integration/alternative-indexes.md`:106 on 2024-10-16 02:11_

```suggestion
described in the [publishing guide](../build_and_publish.md). You will need to set `UV_PUBLISH_URL`
```

---

_@pantheraleo-7 reviewed on 2024-10-16 02:14_

---

_Review comment by @pantheraleo-7 on `docs/reference/pyproject_toml.md`:30 on 2024-10-16 02:14_

why is the lower bound different at different places?

---

_@pantheraleo-7 reviewed on 2024-10-16 02:17_

---

_Review comment by @pantheraleo-7 on `docs/reference/pyproject_toml.md`:199 on 2024-10-16 02:17_

```suggestion
Dynamic metadata is not supported. Please specify all metadata statically.
```

---

_@pantheraleo-7 reviewed on 2024-10-16 02:21_

---

_Review comment by @pantheraleo-7 on `docs/reference/pyproject_toml.md`:210 on 2024-10-16 02:21_

```suggestion
foo = "foo.cli:main"
```

naming the main function as `__main__` will be confusing (with the `__name__=='__main__'` idiom) and there's no precedent for naming the main function as a dunder afaik

---

_Review comment by @pantheraleo-7 on `docs/reference/pyproject_toml.md`:233 on 2024-10-16 02:25_

```suggestion
foo-gui = "foo.gui:main"
```

---

_@pantheraleo-7 reviewed on 2024-10-16 02:25_

---

_@henryiii reviewed on 2024-10-22 04:25_

---

_Review comment by @henryiii on `docs/concepts/projects.md`:78 on 2024-10-22 04:25_

This is a really large range. :)

---

_@henryiii reviewed on 2024-10-22 04:28_

---

_Review comment by @henryiii on `docs/concepts/projects.md`:78 on 2024-10-22 04:28_

Also, you only need an upper bound if there's custom configuration that might change. `[project]` is fine, it's just if there are `tool.uv.*` options that might change in the future. I don't see any custom configuration explained at all below?

---

_@henryiii reviewed on 2024-10-22 04:30_

---

_Review comment by @henryiii on `docs/concepts/projects.md`:649 on 2024-10-22 04:30_

This is only true if you use something that might change in a future version. While setting an upper bound means your package is using an unsupported build backend the second a new release comes out (and that's pretty frequent with uv!).

---

_Review comment by @henryiii on `docs/concepts/projects.md`:78 on 2024-10-22 04:33_

FWIW, scikit-build-core handles this by having the minimum version be something a user can specify, and then the any changed behaviors will keep the same behavior as the minimum version specified. (CMake works the same way)

So if we change a default, or a name, we respect the old way of doing things if the minimum version is set and below the value where it changed.

---

_@henryiii reviewed on 2024-10-22 04:33_

---

_@konstin reviewed on 2024-10-22 09:30_

---

_Review comment by @konstin on `docs/concepts/projects.md`:78 on 2024-10-22 09:30_

We will add custom options, we need them for inclusions and exclusions, and from my experience in maturin and ruff we need to be able to evolve them.

---

_Comment by @konstin on 2024-10-22 09:32_

Note that this PR is in draft state, it is still missing many parts as is the build backend itself.

---

_Review comment by @johncf on `docs/guides/build_and_publish.md`:4 on 2024-10-31 11:50_

I think the original text sounded better. The new text makes it sound like `uv` supports building _only_ via the `uv` build backend (even though "only" is not written).

---

_Review comment by @johncf on `docs/guides/build_and_publish.md`:12 on 2024-10-31 11:52_

I think it's worth mentioning here that `uv` supports other build backends as well.

---

_@johncf reviewed on 2024-10-31 11:52_

---

_@chrisrodrigue reviewed on 2024-10-31 21:34_

---

_Review comment by @chrisrodrigue on `docs/reference/pyproject_toml.md`:190 on 2024-10-31 21:34_

nit: usually stylized as `README.md` and `LICENSE` (no file extension).

If you think flat is better than nested, license can also be expressed as `license.file = "LICENSE"`

---

_@chrisrodrigue reviewed on 2024-10-31 21:38_

---

_Review comment by @chrisrodrigue on `docs/reference/pyproject_toml.md`:197 on 2024-10-31 21:38_

`"Private :: Do Not Upload"` is a handy classifier which causes PyPI to reject a package. It's handy for packages that you don't want to accidentally publish, so maybe it could be used in the full example?

---

_@jiri-bajer reviewed on 2024-11-12 12:57_

---

_Review comment by @jiri-bajer on `docs/reference/pyproject_toml.md`:25 on 2024-11-12 12:57_

There are typos -- `The` -> `There` and `so you constrain` -> `so you should constrain`.

---

_Comment by @konstin on 2025-04-25 14:37_

Closing in favor of #12804

---

_Closed by @konstin on 2025-04-25 14:37_

---
