```yaml
number: 513
title: Better tracing span
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: better-spans
created_at: 2023-11-29T10:30:36Z
updated_at: 2023-11-29T10:34:19Z
url: https://github.com/astral-sh/uv/pull/513
synced_at: 2026-01-12T16:04:00Z
```

# Better tracing span

---

_@konstin_

This will help us get better insight into what is happening and how long it takes. I'm particularly interested in how long the different source dist steps take (download, extract, build step(s)), to make better decisions about their caching, which i want to report through tracing.

Example output:

```console
$ RUST_LOG=puffin=info cargo run --bin puffin -q -- pip-compile -v --no-cache scripts/requirements/all-kinds.in > /dev/null
  puffin_distribution::source_dist::download_source_dist filename="werkzeug-3.0.1.tar.gz", source_dist=werkzeug @ https://files.pythonhosted.org/packages/0d/cc/ff1904eb5eb4b455e442834dabf9427331ac0fa02853bf83db817a7dd53d/werkzeug-3.0.1.tar.gz
  puffin_dispatch::build_source source_dist="werkzeug @ https://files.pythonhosted.org/packages/0d/cc/ff1904eb5eb4b455e442834dabf9427331ac0fa02853bf83db817a7dd53d/werkzeug-3.0.1.tar.gz", subdirectory=None
    puffin_build::extract_archive sdist="werkzeug-3.0.1.tar.gz"
    puffin_dispatch::resolve requirements="flit-core <4"
    puffin_dispatch::install requirements="flit-core ==3.9.0", venv="/tmp/.tmpgZAEAh/.venv"
    puffin_build::get_requires_for_build_wheel name="build_wheel", python_version=3.12
    puffin_build::build package_id="werkzeug @ https://files.pythonhosted.org/packages/0d/cc/ff1904eb5eb4b455e442834dabf9427331ac0fa02853bf83db817a7dd53d/werkzeug-3.0.1.tar.gz"
      puffin_build::run_python_script name="build_wheel", python_version=3.12
  puffin_dispatch::build_source source_dist="pydantic-extra-types @ git+https://github.com/pydantic/pydantic-extra-types.git@843b753e9e8cb74e83cac55598719b39a4d5ef1f", subdirectory=None
    puffin_dispatch::resolve requirements="hatchling"
    puffin_dispatch::install requirements="hatchling ==1.18.0, trove-classifiers ==2023.11.22, editables ==0.5, pathspec ==0.11.2, pluggy ==1.3.0, packaging ==23.2", venv="/tmp/.tmpJjweUn/.venv"
    puffin_build::get_requires_for_build_wheel name="build_wheel", python_version=3.12
    puffin_build::build package_id="pydantic-extra-types @ git+https://github.com/pydantic/pydantic-extra-types.git@843b753e9e8cb74e83cac55598719b39a4d5ef1f"
      puffin_build::run_python_script name="build_wheel", python_version=3.12
  puffin_distribution::source_dist::download_source_dist filename="django-allauth-0.51.0.tar.gz", source_dist=django-allauth==0.51.0
  puffin_dispatch::build_source source_dist="django-allauth==0.51.0", subdirectory=None
    puffin_build::extract_archive sdist="django-allauth-0.51.0.tar.gz"
    puffin_dispatch::resolve requirements="wheel, setuptools, pip"
    puffin_dispatch::install requirements="setuptools ==69.0.2, pip ==23.3.1, wheel ==0.42.0", venv="/tmp/.tmplSZisu/.venv"
    puffin_build::build package_id="django-allauth==0.51.0"
 Resolved 35 packages in 11.71s
```

---

_Merged by @konstin on 2023-11-29 10:34_

---

_Closed by @konstin on 2023-11-29 10:34_

---

_Branch deleted on 2023-11-29 10:34_

---
