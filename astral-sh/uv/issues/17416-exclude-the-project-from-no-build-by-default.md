```yaml
number: 17416
title: "Exclude the project from `no-build` by default"
type: issue
state: open
author: NMertsch
labels:
  - enhancement
assignees: []
created_at: 2026-01-12T13:43:52Z
updated_at: 2026-01-12T16:50:13Z
url: https://github.com/astral-sh/uv/issues/17416
synced_at: 2026-01-12T18:23:49Z
```

# Exclude the project from `no-build` by default

---

_@NMertsch_

### Summary

I'd like to have the following:
1. External dependencies are not built, unless explicitly allowed with `no-binary-package` (basically: I want to install wheels, not sdists)
2. `uv sync` can create/update the venv
3. `uv build` can package my package (e.g. for `uv publish`)

However, that's not possible with packages (`uv init --package`):
1. `no-build = true` correctly prevents sdist installs, but then
2. `uv sync` only works after also configuring `no-binary-package = ["my-package"]`
3. `uv build` [only works with the `uv_build` backend](https://github.com/astral-sh/uv/issues/17416#issuecomment-3739349716). I have to remove `no-build = true` to make it work with other backends.

I think points 2 and 3 should work out of the box.

### How to reproduce
```bash
# create new package
uv init --package --build-backend setuptools my-package
cd my-package

# set `no-build = true`
echo '[tool.uv]' >> pyproject.toml
echo 'no-build = true' >> pyproject.toml

# interact with the package
uv sync  # error: Distribution `my-package==0.1.0 @ editable+.` can't be installed because it is marked as `--no-build` but has no binary distribution

# explicitly allow uv to build my own package
echo 'no-binary-package = ["my-package"]' >> pyproject.toml

# interact with the package
uv sync  # works

# build the package
uv build  # Failed to build, Building source distributions is disabled
```

---

_Label `enhancement` added by @NMertsch on 2026-01-12 13:43_

---

_Comment by @zanieb on 2026-01-12 15:09_

Can you explain (3)? Why does setuptools fail?

---

_Comment by @NMertsch on 2026-01-12 15:56_

> Can you explain (3)? Why does setuptools fail?

I don't think setuptools fails. To me, "Building source distributions is disabled" implies that uv didn't invoke setuptools at all due to `no-build = true`.

But when running `uv init --package my-package` (without `--build-backend setuptools`), `uv build` works just fine.

---

_Comment by @zanieb on 2026-01-12 15:58_

I'm confused that you said

> uv build doesn't work at all for setuptools projects

---

_Comment by @zanieb on 2026-01-12 15:59_

You're saying it works for the `uv_build` backend but not any other ones?

---

_Comment by @NMertsch on 2026-01-12 16:00_

Sorry for the confusion. What I meant was:
* `uv build` with backend `setuptools` works, unless `no-build = true`.
* `uv build` with backend `uv_build` works, independently of `no-build = true`.

I didn't test other backends. Give me a few minutes to try them all.

---

_Comment by @NMertsch on 2026-01-12 16:11_

I now tried steps from the original post on all supported build backends.

* `uv sync` behavior doesn't depend on the build backend. It always fails with `no-build = true` and always works without it.
* `uv build` with `-no-build = true` only works for `--build-backend uv` and fails for all other backends (hatch, flit, pdm, poetry, scikit, setuptools; I skipped maturin because I don't have Rust installed). The error message is always the same: "Building source distributions is disabled".

(Updated the original post: setuptools -> all backends besides uv_build)

---
