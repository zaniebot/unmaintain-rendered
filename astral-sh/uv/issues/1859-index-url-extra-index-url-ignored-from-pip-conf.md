---
number: 1859
title: index-url/extra-index-url ignored from pip.conf
type: issue
state: closed
author: jtanx
labels: []
assignees: []
created_at: 2024-02-22T08:59:42Z
updated_at: 2024-02-22T09:01:26Z
url: https://github.com/astral-sh/uv/issues/1859
synced_at: 2026-01-10T01:23:09Z
---

# index-url/extra-index-url ignored from pip.conf

---

_Issue opened by @jtanx on 2024-02-22 08:59_

```
(jeremy) [jeremy@machine ~]$ pip config debug
env_var:
env:
global:
  /etc/xdg/pip/pip.conf, exists: False
  /etc/pip.conf, exists: True
    global.index-url: https://example.com
site:
  /usr/pip.conf, exists: False
user:
  /home/jeremy/.pip/pip.conf, exists: False
  /home/jeremy/.config/pip/pip.conf, exists: False
```

```
(jeremy) [jeremy@machine ~]$ cat /etc/pip.conf
[global]
index-url = https://example.com
```

With pip

```
(jeremy) [jeremy@machine ~]$ python -m pip install ruff
Looking in indexes: https://example.com
ERROR: Could not find a version that satisfies the requirement ruff (from versio                                                                                        ns: none)
ERROR: No matching distribution found for ruff
```

With uv

```
(jeremy) [jeremy@machine ~]$ uv pip install ruff --verbose
 uv::requirements::from_source source=ruff
    0.006767s DEBUG uv_interpreter::virtual_env Found a virtualenv through VIRTU                                                                                        AL_ENV at: /home/jeremy/.venv
    0.006908s DEBUG uv_interpreter::interpreter Detecting markers for: /home/jer                                                                                        emy/.venv/bin/python
    0.074349s DEBUG uv::commands::pip_install Using Python 3.11.7 environment at                                                                                         /home/jeremy/.venv/bin/python
    0.074932s DEBUG uv_client::registry_client Using registry request timeout of                                                                                         300s
 uv_client::flat_index::from_entries
 uv_resolver::resolver::solve
      0.077356s   0ms DEBUG uv_resolver::resolver Solving with target Python ver                                                                                        sion 3.11.7
   uv_resolver::resolver::choose_version package=root
   uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
        0.077520s   0ms DEBUG uv_resolver::resolver Adding direct dependency: ru                                                                                        ff*
   uv_resolver::resolver::choose_version package=ruff
     uv_resolver::resolver::package_wait package_name=ruff
 uv_resolver::resolver::process_request request=Versions ruff
   uv_client::registry_client::simple_api package=ruff
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/home/jeremy/.cache/u                                                                                        v/simple-v1/pypi/ruff.rkyv
 uv_resolver::resolver::process_request request=Prefetch ruff *
          0.080773s   2ms DEBUG uv_client::cached_client No cache entry for: htt                                                                                        ps://pypi.org/simple/ruff/
       uv_client::cached_client::fresh_request url="https://pypi.org/simple/ruff                                                                                        /"
       uv_client::cached_client::new_cache file=/home/jeremy/.cache/uv/simple-v1                                                                                        /pypi/ruff.rkyv
       uv_client::registry_client::parse_simple_api package=ruff
 uv_resolver::version_map::from_metadata
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=ruff                                                                                        ==0.2.2
     uv_client::registry_client::wheel_metadata built_dist=ruff==0.2.2
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/home/jeremy/.cac                                                                                        he/uv/wheels-v0/pypi/ruff/ruff-0.2.2-py3-none-manylinux_2_17_x86_64.manylinux201                                                                                        4_x86_64.msgpack
        0.471720s 394ms DEBUG uv_resolver::resolver Searching for a compatible v                                                                                        ersion of ruff (*)
        0.471750s 394ms DEBUG uv_resolver::resolver Selecting: ruff==0.2.2 (ruff                                                                                        -0.2.2-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl)
   uv_resolver::resolver::get_dependencies package=ruff, version=0.2.2
     uv_resolver::resolver::distributions_wait package_id=ruff-0.2.2
              0.472024s   0ms DEBUG uv_client::cached_client No cache entry for:                                                                                         https://files.pythonhosted.org/packages/27/f1/3bf230a048561fd03bc779f2a3e5b05d8                                                                                        ea8cb1c91456a4246c5673b8ef5/ruff-0.2.2-py3-none-manylinux_2_17_x86_64.manylinux2                                                                                        014_x86_64.whl
           uv_client::cached_client::fresh_request url="https://files.pythonhost                                                                                        ed.org/packages/27/f1/3bf230a048561fd03bc779f2a3e5b05d8ea8cb1c91456a4246c5673b8e                                                                                        f5/ruff-0.2.2-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl"
           uv_client::cached_client::new_cache file=/home/jeremy/.cache/uv/wheel                                                                                        s-v0/pypi/ruff/ruff-0.2.2-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.ms                                                                                        gpack
           uv_client::registry_client::read_metadata_range_request wheel=ruff-0.                                                                                        2.2-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl
Resolved 1 package in 643ms
    0.720413s DEBUG uv_installer::plan Identified uncached requirement: ruff ==0                                                                                        .2.2
    0.720465s DEBUG uv_installer::plan Unnecessary package: setuptools==69.1.0
    0.720475s DEBUG uv_installer::plan Unnecessary package: wheel==0.42.0
    0.720483s DEBUG uv_installer::plan Unnecessary package: pip==24.0
 uv_installer::downloader::download total=1
   uv_installer::downloader::get_wheel name=ruff==0.2.2, size=Some(7817875), url                                                                                        ="https://files.pythonhosted.org/packages/27/f1/3bf230a048561fd03bc779f2a3e5b05d                                                                                        8ea8cb1c91456a4246c5673b8ef5/ruff-0.2.2-py3-none-manylinux_2_17_x86_64.manylinux                                                                                        2014_x86_64.whl"
     uv_distribution::distribution_database::get_or_build_wheel dist=Built(Regis                                                                                        try(RegistryBuiltDist { filename: WheelFilename { name: PackageName("ruff"), ver                                                                                        sion: "0.2.2", python_tag: ["py3"], abi_tag: ["none"], platform_tag: ["manylinux                                                                                        _2_17_x86_64", "manylinux2014_x86_64"] }, file: File { dist_info_metadata: None,                                                                                         filename: "ruff-0.2.2-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl",                                                                                         hashes: Hashes { md5: None, sha256: Some("5e1439c8f407e4f356470e54cdecdca1bd543                                                                                        9a0673792dbe34a2b0a551a2fe3") }, requires_python: Some(VersionSpecifiers([Versio                                                                                        nSpecifier { operator: GreaterThanEqual, version: "3.7" }])), size: Some(7817875                                                                                        ), upload_time_utc_ms: Some(1708209375006), url: AbsoluteUrl("https://files.pyth                                                                                        onhosted.org/packages/27/f1/3bf230a048561fd03bc779f2a3e5b05d8ea8cb1c91456a4246c5                                                                                        673b8ef5/ruff-0.2.2-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl"), y                                                                                        anked: Some(Bool(false)) }, index: Pypi }))
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=/home/jeremy/.cac                                                                                        he/uv/wheels-v0/pypi/ruff/ruff-0.2.2-py3-none-manylinux_2_17_x86_64.manylinux201                                                                                        4_x86_64.http
              0.722079s   1ms DEBUG uv_client::cached_client No cache entry for:                                                                                         https://files.pythonhosted.org/packages/27/f1/3bf230a048561fd03bc779f2a3e5b05d8                                                                                        ea8cb1c91456a4246c5673b8ef5/ruff-0.2.2-py3-none-manylinux_2_17_x86_64.manylinux2                                                                                        014_x86_64.whl
           uv_client::cached_client::fresh_request url="https://files.pythonhost                                                                                        ed.org/packages/27/f1/3bf230a048561fd03bc779f2a3e5b05d8ea8cb1c91456a4246c5673b8e                                                                                        f5/ruff-0.2.2-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl"
           uv_client::cached_client::new_cache file=/home/jeremy/.cache/uv/wheel                                                                                        s-v0/pypi/ruff/ruff-0.2.2-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.ht                                                                                        tp
           uv_distribution::distribution_database::download wheel=ruff==0.2.2
Downloaded 1 package in 2.45s
 uv_installer::installer::install num_wheels=1
Installed 1 package in 8ms
 + ruff==0.2.2
```

---

_Comment by @jtanx on 2024-02-22 09:01_

Ah wait dupe of #1404

---

_Closed by @jtanx on 2024-02-22 09:01_

---
