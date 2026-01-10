```yaml
number: 1997
title: "permission denied on installed CLI's "
type: issue
state: closed
author: Tomperez98
labels:
  - needs-mre
assignees: []
created_at: 2024-02-26T22:43:03Z
updated_at: 2024-03-01T12:49:46Z
url: https://github.com/astral-sh/uv/issues/1997
synced_at: 2026-01-10T05:40:32Z
```

# permission denied on installed CLI's 

---

_Issue opened by @Tomperez98 on 2024-02-26 22:43_

**uv installed via `pip`**

command `uv pip sync requirements/dev.txt --verbose`

```bash
uv::requirements::from_source source=requirements/dev.txt
    0.000819s DEBUG uv_interpreter::virtual_env Found a virtualenv through VIRTUAL_ENV at: /Users/tomasperez/Documents Local/customer-engine/backend/.venv
    0.001037s DEBUG uv_interpreter::interpreter Cached interpreter info for Python 3.12.2, skipping probing: /Users/tomasperez/Documents Local/customer-engine/backend/.venv/bin/python
    0.001052s DEBUG uv::commands::pip_sync Using Python 3.12.2 environment at /Users/tomasperez/Documents Local/customer-engine/backend/.venv/bin/python`
```

problem: None of the CLIs installed have sufficient permissions
```bash
zsh: permission denied: alembic
zsh: permission denied: pyrgo
zsh: permission denied: uvicorn
```

version:
- `uv 0.1.11 (32e5cacdd 2024-02-26)`
- `zsh 5.9 (x86_64-apple-darwin23.0)`


---

_Comment by @Tomperez98 on 2024-02-26 22:48_

<img width="541" alt="image" src="https://github.com/astral-sh/uv/assets/72174660/0f71900c-8620-4329-a584-3509d5d57495">


---

_Comment by @charliermarsh on 2024-02-26 23:07_

Hm, it works just fine for me:

```
❯ uv pip install uvicorn
Resolved 3 packages in 142ms
Downloaded 2 packages in 212ms
Installed 3 packages in 6ms
 + click==8.1.7
 + h11==0.14.0
 + uvicorn==0.27.1
uv on  charlie/py:main [$⇡] is 📦 v0.1.11 via 🐍 v3.11.7 (uv) via 🦀 v1.76.0
❯ uvicorn
Usage: uvicorn [OPTIONS] APP
Try 'uvicorn --help' for help.

Error: Missing argument 'APP'.
```

---

_Comment by @charliermarsh on 2024-02-26 23:07_

Can you try clearing the cache first (`uv cache clean`), and re-installing into a new environment?

---

_Comment by @Tomperez98 on 2024-02-26 23:11_

Does not work 😞 


---

_Comment by @charliermarsh on 2024-02-26 23:14_

Do you mind posting the full output `uv pip install --verbose` after `uv cache clean`?

---

_Label `needs-mre` added by @charliermarsh on 2024-02-26 23:16_

---

_Comment by @Tomperez98 on 2024-02-26 23:20_

```zsh
backend git:(main) ✗ uv pip install alembic --verbose
 uv::requirements::from_source source=alembic
    0.001647s DEBUG uv_interpreter::virtual_env Found a virtualenv through VIRTUAL_ENV at: /Users/tomasperez/Documents Local/customer-engine/backend/.venv
    0.001841s DEBUG uv_interpreter::interpreter Probing interpreter info for: /Users/tomasperez/Documents Local/customer-engine/backend/.venv/bin/python
    0.030354s DEBUG uv_interpreter::interpreter Found Python 3.12.2 for: /Users/tomasperez/Documents Local/customer-engine/backend/.venv/bin/python
    0.030720s DEBUG uv::commands::pip_install Using Python 3.12.2 environment at /Users/tomasperez/Documents Local/customer-engine/backend/.venv/bin/python
    0.031172s DEBUG uv_client::registry_client Using registry request timeout of 300s
 uv_client::flat_index::from_entries 
 uv_resolver::resolver::solve 
      0.176330s   0ms DEBUG uv_resolver::resolver Solving with target Python version 3.12.2
   uv_resolver::resolver::choose_version package=root
   uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
        0.176494s   0ms DEBUG uv_resolver::resolver Adding direct dependency: alembic*
   uv_resolver::resolver::choose_version package=alembic
     uv_resolver::resolver::package_wait package_name=alembic
 uv_resolver::resolver::process_request request=Versions alembic
   uv_client::registry_client::simple_api package=alembic
     uv_client::cached_client::get_cacheable 
       uv_client::cached_client::read_and_parse_cache file=/Users/tomasperez/Library/Caches/uv/simple-v3/pypi/alembic.rkyv
 uv_resolver::resolver::process_request request=Prefetch alembic *
 uv_client::cached_client::from_path_sync path="/Users/tomasperez/Library/Caches/uv/simple-v3/pypi/alembic.rkyv"
          0.177063s   0ms DEBUG uv_client::cached_client No cache entry for: https://pypi.org/simple/alembic/
       uv_client::cached_client::fresh_request url="https://pypi.org/simple/alembic/"
       uv_client::cached_client::new_cache file=/Users/tomasperez/Library/Caches/uv/simple-v3/pypi/alembic.rkyv
       uv_client::registry_client::parse_simple_api package=alembic
   uv_resolver::version_map::from_metadata 
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=alembic==1.13.1
     uv_client::registry_client::wheel_metadata built_dist=alembic==1.13.1
       uv_client::cached_client::get_serde 
         uv_client::cached_client::get_cacheable 
           uv_client::cached_client::read_and_parse_cache file=/Users/tomasperez/Library/Caches/uv/wheels-v0/pypi/alembic/alembic-1.13.1-py3-none-any.msgpack
        0.243403s  66ms DEBUG uv_resolver::resolver Searching for a compatible version of alembic (*)
 uv_client::cached_client::from_path_sync path="/Users/tomasperez/Library/Caches/uv/wheels-v0/pypi/alembic/alembic-1.13.1-py3-none-any.msgpack"
        0.243422s  66ms DEBUG uv_resolver::resolver Selecting: alembic==1.13.1 (alembic-1.13.1-py3-none-any.whl)
   uv_resolver::resolver::get_dependencies package=alembic, version=1.13.1
     uv_resolver::resolver::distributions_wait package_id=alembic-1.13.1
              0.243456s   0ms DEBUG uv_client::cached_client No cache entry for: https://files.pythonhosted.org/packages/7f/50/9fb3a5c80df6eb6516693270621676980acd6d5a9a7efdbfa273f8d616c7/alembic-1.13.1-py3-none-any.whl.metadata
           uv_client::cached_client::fresh_request url="https://files.pythonhosted.org/packages/7f/50/9fb3a5c80df6eb6516693270621676980acd6d5a9a7efdbfa273f8d616c7/alembic-1.13.1-py3-none-any.whl.metadata"
           uv_client::cached_client::new_cache file=/Users/tomasperez/Library/Caches/uv/wheels-v0/pypi/alembic/alembic-1.13.1-py3-none-any.msgpack
           uv_client::registry_client::parse_metadata21 
        0.314879s  71ms DEBUG uv_resolver::resolver Adding transitive dependency: sqlalchemy>=1.3.0
        0.314896s  71ms DEBUG uv_resolver::resolver Adding transitive dependency: mako*
        0.314900s  71ms DEBUG uv_resolver::resolver Adding transitive dependency: typing-extensions>=4
   uv_resolver::resolver::choose_version package=sqlalchemy
     uv_resolver::resolver::package_wait package_name=sqlalchemy
 uv_resolver::resolver::process_request request=Versions sqlalchemy
   uv_client::registry_client::simple_api package=sqlalchemy
     uv_client::cached_client::get_cacheable 
       uv_client::cached_client::read_and_parse_cache file=/Users/tomasperez/Library/Caches/uv/simple-v3/pypi/sqlalchemy.rkyv
 uv_resolver::resolver::process_request request=Versions mako
   uv_client::registry_client::simple_api package=mako
     uv_client::cached_client::get_cacheable 
       uv_client::cached_client::read_and_parse_cache file=/Users/tomasperez/Library/Caches/uv/simple-v3/pypi/mako.rkyv
 uv_client::cached_client::from_path_sync path="/Users/tomasperez/Library/Caches/uv/simple-v3/pypi/sqlalchemy.rkyv"
 uv_resolver::resolver::process_request request=Versions typing-extensions
   uv_client::registry_client::simple_api package=typing-extensions
 uv_client::cached_client::from_path_sync path="/Users/tomasperez/Library/Caches/uv/simple-v3/pypi/mako.rkyv"
     uv_client::cached_client::get_cacheable 
       uv_client::cached_client::read_and_parse_cache file=/Users/tomasperez/Library/Caches/uv/simple-v3/pypi/typing-extensions.rkyv
 uv_resolver::resolver::process_request request=Prefetch typing-extensions >=4
 uv_resolver::resolver::process_request request=Prefetch mako *
 uv_resolver::resolver::process_request request=Prefetch sqlalchemy >=1.3.0
          0.315076s   0ms DEBUG uv_client::cached_client No cache entry for: https://pypi.org/simple/sqlalchemy/
       uv_client::cached_client::fresh_request url="https://pypi.org/simple/sqlalchemy/"
 uv_client::cached_client::from_path_sync path="/Users/tomasperez/Library/Caches/uv/simple-v3/pypi/typing-extensions.rkyv"
          0.315115s   0ms DEBUG uv_client::cached_client No cache entry for: https://pypi.org/simple/mako/
       uv_client::cached_client::fresh_request url="https://pypi.org/simple/mako/"
          0.315128s   0ms DEBUG uv_client::cached_client No cache entry for: https://pypi.org/simple/typing-extensions/
       uv_client::cached_client::fresh_request url="https://pypi.org/simple/typing-extensions/"
       uv_client::cached_client::new_cache file=/Users/tomasperez/Library/Caches/uv/simple-v3/pypi/mako.rkyv
       uv_client::registry_client::parse_simple_api package=mako
       uv_client::cached_client::new_cache file=/Users/tomasperez/Library/Caches/uv/simple-v3/pypi/typing-extensions.rkyv
       uv_client::registry_client::parse_simple_api package=typing-extensions
   uv_resolver::version_map::from_metadata 
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=mako==1.3.2
     uv_client::registry_client::wheel_metadata built_dist=mako==1.3.2
       uv_client::cached_client::get_serde 
         uv_client::cached_client::get_cacheable 
           uv_client::cached_client::read_and_parse_cache file=/Users/tomasperez/Library/Caches/uv/wheels-v0/pypi/mako/mako-1.3.2-py3-none-any.msgpack
 uv_client::cached_client::from_path_sync path="/Users/tomasperez/Library/Caches/uv/wheels-v0/pypi/mako/mako-1.3.2-py3-none-any.msgpack"
              0.333096s   0ms DEBUG uv_client::cached_client No cache entry for: https://files.pythonhosted.org/packages/2b/8d/9f11d0b9ac521febb806e7f30dc5982d0f4f5821217712c59005fbc5c1e3/Mako-1.3.2-py3-none-any.whl.metadata
           uv_client::cached_client::fresh_request url="https://files.pythonhosted.org/packages/2b/8d/9f11d0b9ac521febb806e7f30dc5982d0f4f5821217712c59005fbc5c1e3/Mako-1.3.2-py3-none-any.whl.metadata"
       uv_client::cached_client::new_cache file=/Users/tomasperez/Library/Caches/uv/simple-v3/pypi/sqlalchemy.rkyv
       uv_client::registry_client::parse_simple_api package=sqlalchemy
   uv_resolver::version_map::from_metadata 
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=typing-extensions==4.10.0
     uv_client::registry_client::wheel_metadata built_dist=typing-extensions==4.10.0
       uv_client::cached_client::get_serde 
         uv_client::cached_client::get_cacheable 
           uv_client::cached_client::read_and_parse_cache file=/Users/tomasperez/Library/Caches/uv/wheels-v0/pypi/typing-extensions/typing_extensions-4.10.0-py3-none-any.msgpack
 uv_client::cached_client::from_path_sync path="/Users/tomasperez/Library/Caches/uv/wheels-v0/pypi/typing-extensions/typing_extensions-4.10.0-py3-none-any.msgpack"
              0.339147s   0ms DEBUG uv_client::cached_client No cache entry for: https://files.pythonhosted.org/packages/f9/de/dc04a3ea60b22624b51c703a84bbe0184abcd1d0b9bc8074b5d6b7ab90bb/typing_extensions-4.10.0-py3-none-any.whl.metadata
           uv_client::cached_client::fresh_request url="https://files.pythonhosted.org/packages/f9/de/dc04a3ea60b22624b51c703a84bbe0184abcd1d0b9bc8074b5d6b7ab90bb/typing_extensions-4.10.0-py3-none-any.whl.metadata"
           uv_client::cached_client::new_cache file=/Users/tomasperez/Library/Caches/uv/wheels-v0/pypi/mako/mako-1.3.2-py3-none-any.msgpack
           uv_client::registry_client::parse_metadata21 
           uv_client::cached_client::new_cache file=/Users/tomasperez/Library/Caches/uv/wheels-v0/pypi/typing-extensions/typing_extensions-4.10.0-py3-none-any.msgpack
           uv_client::registry_client::parse_metadata21 
   uv_resolver::version_map::from_metadata 
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=sqlalchemy==2.0.27
     uv_client::registry_client::wheel_metadata built_dist=sqlalchemy==2.0.27
       uv_client::cached_client::get_serde 
         uv_client::cached_client::get_cacheable 
           uv_client::cached_client::read_and_parse_cache file=/Users/tomasperez/Library/Caches/uv/wheels-v0/pypi/sqlalchemy/sqlalchemy-2.0.27-cp312-cp312-macosx_11_0_arm64.msgpack
        0.402676s  87ms DEBUG uv_resolver::resolver Searching for a compatible version of sqlalchemy (>=1.3.0)
        0.402684s  87ms DEBUG uv_resolver::resolver Selecting: sqlalchemy==2.0.27 (SQLAlchemy-2.0.27-cp312-cp312-macosx_11_0_arm64.whl)
   uv_resolver::resolver::get_dependencies package=sqlalchemy, version=2.0.27
     uv_resolver::resolver::distributions_wait package_id=sqlalchemy-2.0.27
 uv_client::cached_client::from_path_sync path="/Users/tomasperez/Library/Caches/uv/wheels-v0/pypi/sqlalchemy/sqlalchemy-2.0.27-cp312-cp312-macosx_11_0_arm64.msgpack"
              0.402744s   0ms DEBUG uv_client::cached_client No cache entry for: https://files.pythonhosted.org/packages/b6/8f/7a6bda3ebe31b381325aaae470336c2e6777eb71bfd237b5560d5cb5a5ff/SQLAlchemy-2.0.27-cp312-cp312-macosx_11_0_arm64.whl.metadata
           uv_client::cached_client::fresh_request url="https://files.pythonhosted.org/packages/b6/8f/7a6bda3ebe31b381325aaae470336c2e6777eb71bfd237b5560d5cb5a5ff/SQLAlchemy-2.0.27-cp312-cp312-macosx_11_0_arm64.whl.metadata"
           uv_client::cached_client::new_cache file=/Users/tomasperez/Library/Caches/uv/wheels-v0/pypi/sqlalchemy/sqlalchemy-2.0.27-cp312-cp312-macosx_11_0_arm64.msgpack
           uv_client::registry_client::parse_metadata21 
        0.420858s  18ms DEBUG uv_resolver::resolver Adding transitive dependency: typing-extensions>=4.6.0
   uv_resolver::resolver::choose_version package=mako
     uv_resolver::resolver::package_wait package_name=mako
        0.420891s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of mako (*)
        0.420896s   0ms DEBUG uv_resolver::resolver Selecting: mako==1.3.2 (Mako-1.3.2-py3-none-any.whl)
   uv_resolver::resolver::get_dependencies package=mako, version=1.3.2
     uv_resolver::resolver::distributions_wait package_id=mako-1.3.2
        0.420913s   0ms DEBUG uv_resolver::resolver Adding transitive dependency: markupsafe>=0.9.2
   uv_resolver::resolver::choose_version package=typing-extensions
     uv_resolver::resolver::package_wait package_name=typing-extensions
        0.420925s   0ms DEBUG uv_resolver::resolver Searching for a compatible version of typing-extensions (>=4.6.0)
        0.420929s   0ms DEBUG uv_resolver::resolver Selecting: typing-extensions==4.10.0 (typing_extensions-4.10.0-py3-none-any.whl)
   uv_resolver::resolver::get_dependencies package=typing-extensions, version=4.10.0
     uv_resolver::resolver::distributions_wait package_id=typing-extensions-4.10.0
   uv_resolver::resolver::choose_version package=markupsafe
     uv_resolver::resolver::package_wait package_name=markupsafe
 uv_resolver::resolver::process_request request=Prefetch typing-extensions >=4.6.0
 uv_resolver::resolver::process_request request=Versions markupsafe
   uv_client::registry_client::simple_api package=markupsafe
     uv_client::cached_client::get_cacheable 
       uv_client::cached_client::read_and_parse_cache file=/Users/tomasperez/Library/Caches/uv/simple-v3/pypi/markupsafe.rkyv
 uv_resolver::resolver::process_request request=Prefetch markupsafe >=0.9.2
 uv_client::cached_client::from_path_sync path="/Users/tomasperez/Library/Caches/uv/simple-v3/pypi/markupsafe.rkyv"
          0.421012s   0ms DEBUG uv_client::cached_client No cache entry for: https://pypi.org/simple/markupsafe/
       uv_client::cached_client::fresh_request url="https://pypi.org/simple/markupsafe/"
       uv_client::cached_client::new_cache file=/Users/tomasperez/Library/Caches/uv/simple-v3/pypi/markupsafe.rkyv
       uv_client::registry_client::parse_simple_api package=markupsafe
   uv_resolver::version_map::from_metadata 
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=markupsafe==2.1.5
     uv_client::registry_client::wheel_metadata built_dist=markupsafe==2.1.5
       uv_client::cached_client::get_serde 
         uv_client::cached_client::get_cacheable 
           uv_client::cached_client::read_and_parse_cache file=/Users/tomasperez/Library/Caches/uv/wheels-v0/pypi/markupsafe/markupsafe-2.1.5-cp312-cp312-macosx_10_9_universal2.msgpack
        0.442344s  21ms DEBUG uv_resolver::resolver Searching for a compatible version of markupsafe (>=0.9.2)
        0.442350s  21ms DEBUG uv_resolver::resolver Selecting: markupsafe==2.1.5 (MarkupSafe-2.1.5-cp312-cp312-macosx_10_9_universal2.whl)
 uv_client::cached_client::from_path_sync path="/Users/tomasperez/Library/Caches/uv/wheels-v0/pypi/markupsafe/markupsafe-2.1.5-cp312-cp312-macosx_10_9_universal2.msgpack"
   uv_resolver::resolver::get_dependencies package=markupsafe, version=2.1.5
     uv_resolver::resolver::distributions_wait package_id=markupsafe-2.1.5
              0.442386s   0ms DEBUG uv_client::cached_client No cache entry for: https://files.pythonhosted.org/packages/53/bd/583bf3e4c8d6a321938c13f49d44024dbe5ed63e0a7ba127e454a66da974/MarkupSafe-2.1.5-cp312-cp312-macosx_10_9_universal2.whl.metadata
           uv_client::cached_client::fresh_request url="https://files.pythonhosted.org/packages/53/bd/583bf3e4c8d6a321938c13f49d44024dbe5ed63e0a7ba127e454a66da974/MarkupSafe-2.1.5-cp312-cp312-macosx_10_9_universal2.whl.metadata"
           uv_client::cached_client::new_cache file=/Users/tomasperez/Library/Caches/uv/wheels-v0/pypi/markupsafe/markupsafe-2.1.5-cp312-cp312-macosx_10_9_universal2.msgpack
           uv_client::registry_client::parse_metadata21 
Resolved 5 packages in 283ms
    0.459382s DEBUG uv_installer::plan Identified uncached requirement: alembic ==1.13.1
    0.459439s DEBUG uv_installer::plan Identified uncached requirement: mako ==1.3.2
    0.459458s DEBUG uv_installer::plan Identified uncached requirement: markupsafe ==2.1.5
    0.459475s DEBUG uv_installer::plan Identified uncached requirement: sqlalchemy ==2.0.27
    0.459491s DEBUG uv_installer::plan Identified uncached requirement: typing-extensions ==4.10.0
 uv_installer::downloader::download total=5
   uv_installer::downloader::get_wheel name=sqlalchemy==2.0.27, size=Some(2070533), url="https://files.pythonhosted.org/packages/b6/8f/7a6bda3ebe31b381325aaae470336c2e6777eb71bfd237b5560d5cb5a5ff/SQLAlchemy-2.0.27-cp312-cp312-macosx_11_0_arm64.whl"
     uv_distribution::distribution_database::get_or_build_wheel dist=Built(Registry(RegistryBuiltDist { filename: WheelFilename { name: PackageName("sqlalchemy"), version: "2.0.27", python_tag: ["cp312"], abi_tag: ["cp312"], platform_tag: ["macosx_11_0_arm64"] }, file: File { dist_info_metadata: Some(Hashes(Hashes { md5: None, sha256: Some("7d91ab371812aa863fbcb8cfea95f2ec0c7ff45be9cba3f448dfd09a3a5a7fc3") })), filename: "SQLAlchemy-2.0.27-cp312-cp312-macosx_11_0_arm64.whl", hashes: Hashes { md5: None, sha256: Some("ce850db091bf7d2a1f2fdb615220b968aeff3849007b1204bf6e3e50a57b3d32") }, requires_python: Some(VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "3.7" }])), size: Some(2070533), upload_time_utc_ms: Some(1707837632837), url: AbsoluteUrl("https://files.pythonhosted.org/packages/b6/8f/7a6bda3ebe31b381325aaae470336c2e6777eb71bfd237b5560d5cb5a5ff/SQLAlchemy-2.0.27-cp312-cp312-macosx_11_0_arm64.whl"), yanked: Some(Bool(false)) }, index: Pypi }))
       uv_client::cached_client::get_serde 
         uv_client::cached_client::get_cacheable 
           uv_client::cached_client::read_and_parse_cache file=/Users/tomasperez/Library/Caches/uv/wheels-v0/pypi/sqlalchemy/sqlalchemy-2.0.27-cp312-cp312-macosx_11_0_arm64.http
   uv_installer::downloader::get_wheel name=alembic==1.13.1, size=Some(233424), url="https://files.pythonhosted.org/packages/7f/50/9fb3a5c80df6eb6516693270621676980acd6d5a9a7efdbfa273f8d616c7/alembic-1.13.1-py3-none-any.whl"
 uv_client::cached_client::from_path_sync path="/Users/tomasperez/Library/Caches/uv/wheels-v0/pypi/sqlalchemy/sqlalchemy-2.0.27-cp312-cp312-macosx_11_0_arm64.http"
     uv_distribution::distribution_database::get_or_build_wheel dist=Built(Registry(RegistryBuiltDist { filename: WheelFilename { name: PackageName("alembic"), version: "1.13.1", python_tag: ["py3"], abi_tag: ["none"], platform_tag: ["any"] }, file: File { dist_info_metadata: Some(Hashes(Hashes { md5: None, sha256: Some("5b517634146486a5b9e47886984229766891b76b24539c3025ddb550715b49d4") })), filename: "alembic-1.13.1-py3-none-any.whl", hashes: Hashes { md5: None, sha256: Some("2edcc97bed0bd3272611ce3a98d98279e9c209e7186e43e75bbb1b2bdfdbcc43") }, requires_python: Some(VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "3.8" }])), size: Some(233424), upload_time_utc_ms: Some(1703091976839), url: AbsoluteUrl("https://files.pythonhosted.org/packages/7f/50/9fb3a5c80df6eb6516693270621676980acd6d5a9a7efdbfa273f8d616c7/alembic-1.13.1-py3-none-any.whl"), yanked: Some(Bool(false)) }, index: Pypi }))
       uv_client::cached_client::get_serde 
         uv_client::cached_client::get_cacheable 
           uv_client::cached_client::read_and_parse_cache file=/Users/tomasperez/Library/Caches/uv/wheels-v0/pypi/alembic/alembic-1.13.1-py3-none-any.http
   uv_installer::downloader::get_wheel name=mako==1.3.2, size=Some(78667), url="https://files.pythonhosted.org/packages/2b/8d/9f11d0b9ac521febb806e7f30dc5982d0f4f5821217712c59005fbc5c1e3/Mako-1.3.2-py3-none-any.whl"
     uv_distribution::distribution_database::get_or_build_wheel dist=Built(Registry(RegistryBuiltDist { filename: WheelFilename { name: PackageName("mako"), version: "1.3.2", python_tag: ["py3"], abi_tag: ["none"], platform_tag: ["any"] }, file: File { dist_info_metadata: Some(Hashes(Hashes { md5: None, sha256: Some("1b796c3d36003da9da61d93efdef327a4e43669b8e8d3dd071ff9130bae5314d") })), filename: "Mako-1.3.2-py3-none-any.whl", hashes: Hashes { md5: None, sha256: Some("32a99d70754dfce237019d17ffe4a282d2d3351b9c476e90d8a60e63f133b80c") }, requires_python: Some(VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "3.8" }])), size: Some(78667), upload_time_utc_ms: Some(1706621454179), url: AbsoluteUrl("https://files.pythonhosted.org/packages/2b/8d/9f11d0b9ac521febb806e7f30dc5982d0f4f5821217712c59005fbc5c1e3/Mako-1.3.2-py3-none-any.whl"), yanked: Some(Bool(false)) }, index: Pypi }))
       uv_client::cached_client::get_serde 
         uv_client::cached_client::get_cacheable 
           uv_client::cached_client::read_and_parse_cache file=/Users/tomasperez/Library/Caches/uv/wheels-v0/pypi/mako/mako-1.3.2-py3-none-any.http
 uv_client::cached_client::from_path_sync path="/Users/tomasperez/Library/Caches/uv/wheels-v0/pypi/alembic/alembic-1.13.1-py3-none-any.http"
   uv_installer::downloader::get_wheel name=typing-extensions==4.10.0, size=Some(33926), url="https://files.pythonhosted.org/packages/f9/de/dc04a3ea60b22624b51c703a84bbe0184abcd1d0b9bc8074b5d6b7ab90bb/typing_extensions-4.10.0-py3-none-any.whl"
 uv_client::cached_client::from_path_sync path="/Users/tomasperez/Library/Caches/uv/wheels-v0/pypi/mako/mako-1.3.2-py3-none-any.http"
     uv_distribution::distribution_database::get_or_build_wheel dist=Built(Registry(RegistryBuiltDist { filename: WheelFilename { name: PackageName("typing-extensions"), version: "4.10.0", python_tag: ["py3"], abi_tag: ["none"], platform_tag: ["any"] }, file: File { dist_info_metadata: Some(Hashes(Hashes { md5: None, sha256: Some("6ce7f9d063dc08ad3338ff2708359238004dc021225fad944210d45a734e6b15") })), filename: "typing_extensions-4.10.0-py3-none-any.whl", hashes: Hashes { md5: None, sha256: Some("69b1a937c3a517342112fb4c6df7e72fc39a38e7891a5730ed4985b5214b5475") }, requires_python: Some(VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "3.8" }])), size: Some(33926), upload_time_utc_ms: Some(1708899167720), url: AbsoluteUrl("https://files.pythonhosted.org/packages/f9/de/dc04a3ea60b22624b51c703a84bbe0184abcd1d0b9bc8074b5d6b7ab90bb/typing_extensions-4.10.0-py3-none-any.whl"), yanked: Some(Bool(false)) }, index: Pypi }))
       uv_client::cached_client::get_serde 
         uv_client::cached_client::get_cacheable 
           uv_client::cached_client::read_and_parse_cache file=/Users/tomasperez/Library/Caches/uv/wheels-v0/pypi/typing-extensions/typing_extensions-4.10.0-py3-none-any.http
   uv_installer::downloader::get_wheel name=markupsafe==2.1.5, size=Some(18215), url="https://files.pythonhosted.org/packages/53/bd/583bf3e4c8d6a321938c13f49d44024dbe5ed63e0a7ba127e454a66da974/MarkupSafe-2.1.5-cp312-cp312-macosx_10_9_universal2.whl"
 uv_client::cached_client::from_path_sync path="/Users/tomasperez/Library/Caches/uv/wheels-v0/pypi/typing-extensions/typing_extensions-4.10.0-py3-none-any.http"
     uv_distribution::distribution_database::get_or_build_wheel dist=Built(Registry(RegistryBuiltDist { filename: WheelFilename { name: PackageName("markupsafe"), version: "2.1.5", python_tag: ["cp312"], abi_tag: ["cp312"], platform_tag: ["macosx_10_9_universal2"] }, file: File { dist_info_metadata: Some(Hashes(Hashes { md5: None, sha256: Some("d9d4433da9ba3992dfa57d3083524de4fdeef5aaea002c5549f7469ac254ee8f") })), filename: "MarkupSafe-2.1.5-cp312-cp312-macosx_10_9_universal2.whl", hashes: Hashes { md5: None, sha256: Some("8dec4936e9c3100156f8a2dc89c4b88d5c435175ff03413b443469c7c8c5f4d1") }, requires_python: Some(VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "3.7" }])), size: Some(18215), upload_time_utc_ms: Some(1706891433081), url: AbsoluteUrl("https://files.pythonhosted.org/packages/53/bd/583bf3e4c8d6a321938c13f49d44024dbe5ed63e0a7ba127e454a66da974/MarkupSafe-2.1.5-cp312-cp312-macosx_10_9_universal2.whl"), yanked: Some(Bool(false)) }, index: Pypi }))
       uv_client::cached_client::get_serde 
         uv_client::cached_client::get_cacheable 
           uv_client::cached_client::read_and_parse_cache file=/Users/tomasperez/Library/Caches/uv/wheels-v0/pypi/markupsafe/markupsafe-2.1.5-cp312-cp312-macosx_10_9_universal2.http
              0.459825s   0ms DEBUG uv_client::cached_client No cache entry for: https://files.pythonhosted.org/packages/b6/8f/7a6bda3ebe31b381325aaae470336c2e6777eb71bfd237b5560d5cb5a5ff/SQLAlchemy-2.0.27-cp312-cp312-macosx_11_0_arm64.whl
           uv_client::cached_client::fresh_request url="https://files.pythonhosted.org/packages/b6/8f/7a6bda3ebe31b381325aaae470336c2e6777eb71bfd237b5560d5cb5a5ff/SQLAlchemy-2.0.27-cp312-cp312-macosx_11_0_arm64.whl"
 uv_client::cached_client::from_path_sync path="/Users/tomasperez/Library/Caches/uv/wheels-v0/pypi/markupsafe/markupsafe-2.1.5-cp312-cp312-macosx_10_9_universal2.http"
              0.459847s   0ms DEBUG uv_client::cached_client No cache entry for: https://files.pythonhosted.org/packages/7f/50/9fb3a5c80df6eb6516693270621676980acd6d5a9a7efdbfa273f8d616c7/alembic-1.13.1-py3-none-any.whl
           uv_client::cached_client::fresh_request url="https://files.pythonhosted.org/packages/7f/50/9fb3a5c80df6eb6516693270621676980acd6d5a9a7efdbfa273f8d616c7/alembic-1.13.1-py3-none-any.whl"
              0.459860s   0ms DEBUG uv_client::cached_client No cache entry for: https://files.pythonhosted.org/packages/2b/8d/9f11d0b9ac521febb806e7f30dc5982d0f4f5821217712c59005fbc5c1e3/Mako-1.3.2-py3-none-any.whl
           uv_client::cached_client::fresh_request url="https://files.pythonhosted.org/packages/2b/8d/9f11d0b9ac521febb806e7f30dc5982d0f4f5821217712c59005fbc5c1e3/Mako-1.3.2-py3-none-any.whl"
              0.459871s   0ms DEBUG uv_client::cached_client No cache entry for: https://files.pythonhosted.org/packages/f9/de/dc04a3ea60b22624b51c703a84bbe0184abcd1d0b9bc8074b5d6b7ab90bb/typing_extensions-4.10.0-py3-none-any.whl
           uv_client::cached_client::fresh_request url="https://files.pythonhosted.org/packages/f9/de/dc04a3ea60b22624b51c703a84bbe0184abcd1d0b9bc8074b5d6b7ab90bb/typing_extensions-4.10.0-py3-none-any.whl"
              0.459882s   0ms DEBUG uv_client::cached_client No cache entry for: https://files.pythonhosted.org/packages/53/bd/583bf3e4c8d6a321938c13f49d44024dbe5ed63e0a7ba127e454a66da974/MarkupSafe-2.1.5-cp312-cp312-macosx_10_9_universal2.whl
           uv_client::cached_client::fresh_request url="https://files.pythonhosted.org/packages/53/bd/583bf3e4c8d6a321938c13f49d44024dbe5ed63e0a7ba127e454a66da974/MarkupSafe-2.1.5-cp312-cp312-macosx_10_9_universal2.whl"
           uv_client::cached_client::new_cache file=/Users/tomasperez/Library/Caches/uv/wheels-v0/pypi/typing-extensions/typing_extensions-4.10.0-py3-none-any.http
           uv_distribution::distribution_database::download wheel=typing-extensions==4.10.0
           uv_client::cached_client::new_cache file=/Users/tomasperez/Library/Caches/uv/wheels-v0/pypi/alembic/alembic-1.13.1-py3-none-any.http
           uv_distribution::distribution_database::download wheel=alembic==1.13.1
           uv_client::cached_client::new_cache file=/Users/tomasperez/Library/Caches/uv/wheels-v0/pypi/mako/mako-1.3.2-py3-none-any.http
           uv_distribution::distribution_database::download wheel=mako==1.3.2
           uv_client::cached_client::new_cache file=/Users/tomasperez/Library/Caches/uv/wheels-v0/pypi/markupsafe/markupsafe-2.1.5-cp312-cp312-macosx_10_9_universal2.http
           uv_distribution::distribution_database::download wheel=markupsafe==2.1.5
           uv_client::cached_client::new_cache file=/Users/tomasperez/Library/Caches/uv/wheels-v0/pypi/sqlalchemy/sqlalchemy-2.0.27-cp312-cp312-macosx_11_0_arm64.http
           uv_distribution::distribution_database::download wheel=sqlalchemy==2.0.27
Downloaded 5 packages in 135ms
 uv_installer::installer::install num_wheels=5
Installed 5 packages in 5ms
 + alembic==1.13.1
 + mako==1.3.2
 + markupsafe==2.1.5
 + sqlalchemy==2.0.27
 + typing-extensions==4.10.0
➜  backend git:(main) ✗ alembic
zsh: permission denied: alembic
```

---

_Comment by @konstin on 2024-02-27 12:39_

Can you run `which alembic` and post the output of `ls -lah <path from the which command>`?

---

_Comment by @Tomperez98 on 2024-02-27 15:24_

I just tried to replicate the error on another laptop (also Mac m2), it works in this one just fine. It seems to be an issue with my laptop configuration.

In this one working this is the output of the command you just asked for:
- `which alembic`
```zsh
/Users/tomasperez/Documents/project/issue-uv/.venv/bin/alembic
```

- `ls -lah /Users/tomasperez/Documents/project/issue-uv/.venv/bin/alembic`
```zsh
-rwxr-xr-x@ 1 tomasperez  staff   258B Feb 27 10:18 /Users/tomasperez/Documents/project/issue-uv/.venv/bin/alembic
```


---

_Comment by @konstin on 2024-02-27 15:50_

Odd, the executable bit is there, so it should work. Does it work with pip?

---

_Comment by @Tomperez98 on 2024-02-27 17:05_

Yes. It works just fine with `pip`. 

---

_Comment by @Tomperez98 on 2024-02-28 02:58_

This is the result when no sufficient permissions

```zsh
-rwxr-xr-x  1 tomasperez  staff   271B Feb 26 22:43 .venv/bin/alembic
```

---

_Comment by @Tomperez98 on 2024-02-28 18:15_

Even with sudo it does not work

```zsh
sudo: unable to execute /Users/tomasperez/Documents Local/customer-engine/backend/.venv/bin/alembic: Permission denied
```

---

_Comment by @Tomperez98 on 2024-02-28 19:51_

@charliermarsh @konstin I recorded the behavior

https://github.com/astral-sh/uv/assets/72174660/ef5b499a-e828-4232-934b-8c2a81182a77



---

_Comment by @konstin on 2024-02-28 23:15_

Could you post the output of `ls -le .venv/bin/alembic` and try https://apple.stackexchange.com/a/243708? Could you show the contents of `.venv/bin/alembic` for both pip and uv? I must admit the i otherwise don't have too many ideas what the cause could be. 

---

_Comment by @Tomperez98 on 2024-02-28 23:24_

That is fine. At this point I'm pretty sure it's an issue with my laptop configuration and not an `uv` issue

---

_Comment by @Tomperez98 on 2024-02-28 23:31_

Outputs: 
- `ls -le .venv/bin/alembic`
```zsh
-rwxr-xr-x  1 tomasperez  staff  271 Feb 28 16:13 .venv/bin/alembic
```

- file content with `uv`
```py
#!/Users/tomasperez/Documents Local/customer-engine/backend/.venv/bin/python
# -*- coding: utf-8 -*-
import re
import sys
from alembic.config import main
if __name__ == "__main__":
    sys.argv[0] = re.sub(r"(-script\.pyw|\.exe)?$", "", sys.argv[0])
    sys.exit(main())

```

- file content with `pip`
```py
#!/bin/sh
'''exec' "/Users/tomasperez/Documents Local/customer-engine/backend/.venv/bin/python" "$0" "$@"
' '''
# -*- coding: utf-8 -*-
import re
import sys
from alembic.config import main
if __name__ == '__main__':
    sys.argv[0] = re.sub(r'(-script\.pyw|\.exe)?$', '', sys.argv[0])
    sys.exit(main())

```

The main difference seems to be the `#!/bin/sh`


This still does not remove the fact that seems to be more like an issue with my laptop security config more that an `uv` issue


---

_Comment by @Tomperez98 on 2024-02-28 23:36_

Weird still is that these both ways to run the command works

`python .venv/bin/alembic`

and 

`python -m alembic`

it's calling the script that fails

`alembic` -> `zsh: permission denied: alembic`

---

_Comment by @Tomperez98 on 2024-02-28 23:50_

Holy 💩 . It's solved

Botton line the issue is that I was working on `~/Documents Local` (a folder created by me) as soon as I moved the project to the `~/Document` folder (which is created an managed by the mac) it just works


---

_Closed by @Tomperez98 on 2024-02-29 00:21_

---

_Comment by @konstin on 2024-02-29 08:51_

Oh no, i see the problem now: There's an unescaped space, so we're running `/Users/tomasperez/Documents` with the argument `Local/customer-engine/backend/.venv/bin/python`, which has a permission denied error since `/Users/tomasperez/Documents` but is executable, as a directory.

---

_Comment by @Tomperez98 on 2024-02-29 12:07_

Oh! I'd like to help fixing that!

---

_Reopened by @Tomperez98 on 2024-02-29 14:13_

---

_Closed by @Tomperez98 on 2024-03-01 12:49_

---
