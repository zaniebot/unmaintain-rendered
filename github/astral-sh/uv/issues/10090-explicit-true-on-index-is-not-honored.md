---
number: 10090
title: explicit = true on index is not honored
type: issue
state: closed
author: tmatup
labels:
  - needs-mre
assignees: []
created_at: 2024-12-22T03:39:41Z
updated_at: 2024-12-27T13:44:58Z
url: https://github.com/astral-sh/uv/issues/10090
synced_at: 2026-01-07T13:12:18-06:00
---

# explicit = true on index is not honored

---

_Issue opened by @tmatup on 2024-12-22 03:39_

pyproject.toml
````
[project]
name = "my-project"
version = "0.1.0"
description = "My Project"
authors = []
readme = "README.md"
requires-python = ">=3.10,<3.12"
dependencies = [
    # FROZEN DEPENDENCIES
    "mypackage==1.0.1024",

    # UPDATEABLE DEPENDENCIES - MAJOR
    "pandas>=2.2.3"
]

[[tool.uv.index]]
name = "myindex"
url = "https://myorg.pkgs.visualstudio.com/myproject/_packaging/myfeed/pypi/simple/"
explicit = true

[tool.uv.sources]
mypackage = { index = "myindex" }
````

uv lock
```
(py311) joe@test:~/root/$ uv lock
Using CPython 3.10.16
error: Failed to fetch: `https://https//myorg.pkgs.visualstudio.com/myproject/_packaging/myfeed/pypi/simple/gevent/`
  Caused by: Could not connect, are you offline?
  Caused by: Request failed after 3 retries
  Caused by: error sending request for url (https://https//myorg.pkgs.visualstudio.com/myproject/_packaging/myfeed/pypi/simple/gevent/)
  Caused by: client error (Connect)
  Caused by: dns error: failed to lookup address information: Temporary failure in name resolution
  Caused by: failed to lookup address information: Temporary failure in name resolution
```

uv version
```
(py311) joe@test:~/root/$ pip show uv
Name: uv
Version: 0.5.11
Summary: An extremely fast Python package and project manager, written in Rust.
Home-page: https://pypi.org/project/uv/
Author: uv
Author-email: "Astral Software Inc." <hey@astral.sh>
License: MIT OR Apache-2.0
Location: /home/joe/venvs/py311/lib/python3.11/site-packages
Requires: 
Required-by: 
```

Per https://docs.astral.sh/uv/configuration/indexes/#pinning-a-package-to-an-index, shouldn't the index be used for resolving only the `mypackage` package? 

---

_Comment by @charliermarsh on 2024-12-22 16:50_

Yes, but it's going to be hard for me to help without a reproduction that I can run myself. `uv lock --show-settings` might be helpful.

---

_Label `needs-mre` added by @charliermarsh on 2024-12-22 16:50_

---

_Comment by @tmatup on 2024-12-22 19:48_

Please see below for the `uv lock --show-settings` output. Note that I redacted the username, password, org, project, and feed values.

```
GlobalSettings {
    quiet: false,
    verbose: 0,
    color: Auto,
    native_tls: false,
    concurrency: Concurrency {
        downloads: 50,
        builds: 48,
        installs: 48,
    },
    connectivity: Online,
    allow_insecure_host: [],
    show_settings: true,
    preview: Disabled,
    python_preference: Managed,
    python_downloads: Automatic,
    no_progress: false,
    installer_metadata: true,
}
CacheSettings {
    no_cache: false,
    cache_dir: None,
}
LockSettings {
    locked: false,
    frozen: false,
    dry_run: false,
    python: None,
    install_mirrors: PythonInstallMirrors {
        python_install_mirror: None,
        pypy_install_mirror: None,
    },
    refresh: None(
        Timestamp(
            SystemTime {
                tv_sec: 1734896684,
                tv_nsec: 460173761,
            },
        ),
    ),
    settings: ResolverSettings {
        index_locations: IndexLocations {
            indexes: [
                Index {
                    name: None,
                    url: Url(
                        VerbatimUrl {
                            url: Url {
                                scheme: "https",
                                cannot_be_a_base: false,
                                username: "<username>",
                                password: Some(
                                    "<password>",
                                ),
                                host: Some(
                                    Domain(
                                        "https",
                                    ),
                                ),
                                port: None,
                                path: "//myorg.pkgs.visualstudio.com/myproject/_packaging/myfeed/pypi/simple",
                                query: None,
                                fragment: None,
                            },
                            given: Some(
                                "https://<username>:<password>@https://myorg.pkgs.visualstudio.com/myproject/_packaging/myfeed/pypi/simple",
                            ),
                        },
                    ),
                    explicit: false,
                    default: false,
                    origin: Some(
                        Cli,
                    ),
                    publish_url: None,
                },
            ],
            flat_index: [],
            no_index: false,
        },
        index_strategy: FirstIndex,
        keyring_provider: Disabled,
        resolution: Highest,
        prerelease: IfNecessaryOrExplicit,
        fork_strategy: RequiresPython,
        dependency_metadata: DependencyMetadata(
            {},
        ),
        config_setting: ConfigSettings(
            {},
        ),
        no_build_isolation: false,
        no_build_isolation_package: [],
        exclude_newer: None,
        link_mode: Hardlink,
        upgrade: None,
        build_options: BuildOptions {
            no_binary: None,
            no_build: None,
        },
        sources: Enabled,
    },
}
```

---

_Comment by @charliermarsh on 2024-12-22 20:27_

It looks like you're providing the index over the CLI -- is that correct?

---

_Comment by @tmatup on 2024-12-22 21:03_

Yep, I had the `UV_EXTRA_INDEX_URL` env added to my bashrc. I had it removed, and now I am getting a different error when doing `uv lock`, based on the pyproject.toml that I have (shared earlier), what could be wrong?

```
(py311) joe@test:~/root/$ uv lock
Using CPython 3.10.16
  × No solution found when resolving dependencies for split (python_full_version >= '3.10' and sys_platform == 'darwin'):
  ╰─▶ Because mypackage was not found in the package registry and sandbox depends on mypackage==1.0.2408007, we can conclude that sandbox 's requirements are unsatisfiable.
      And because your workspace requires sandbox, we can conclude that your workspace's requirements are unsatisfiable.
```

Though I was able to do pip install successfully of the package when directly install thru `pip`

```
(py311) joe@test:~/root/$ pip install --extra-index-url https://<username>:<pat>@myorg.pkgs.visualstudio.com/myproject/_packaging/myfeed/pypi/simple/ mypackage==1.0.1024
Looking in indexes: https://pypi.org/simple, https://<username>:****@myorg.pkgs.visualstudio.com/myproject/_packaging/myfeed/pypi/simple/
Requirement already satisfied: mypackage==1.0.1024 in /home/joe/venvs/py311/lib/python3.11/site-packages (1.0.1024)
Requirement already satisfied: Pillow in /home/venvs/venvs/py311/lib/python3.11/site-packages (from mypackage==1.0.1024) (11.0.0)
Requirement already satisfied: numpy in /home/venvs/venvs/py311/lib/python3.11/site-packages (from mypackage==1.0.1024) (1.26.4)
Requirement already satisfied: scikit-learn in /home/venvs/venvs/py311/lib/python3.11/site-packages (from mypackage==1.0.1024) (1.5.2)
Requirement already satisfied: scipy>=1.6.0 in /home/venvs/venvs/py311/lib/python3.11/site-packages (from scikit-learn->mypackage==1.0.1024) (1.14.1)
Requirement already satisfied: joblib>=1.2.0 in /home/venvs/venvs/py311/lib/python3.11/site-packages (from scikit-learn->mypackage==1.0.1024) (1.4.2)
Requirement already satisfied: threadpoolctl>=3.1.0 in /home/venvs/venvs/py311/lib/python3.11/site-packages (from scikit-learn->mypackage==1.0.1024) (3.5.0)
```

And I have the following env variable declared in my bashrc

```
# uv
export UV_INDEX_DUFEED_USERNAME="<username>"
export UV_INDEX_DUFEED_PASSWORD=<pat>
export AZURE_PACKAGES_TOKEN=<pat>
```

---

_Comment by @tmatup on 2024-12-23 00:27_

@charliermarsh any insights for this? I can create a separate issue for this if you want.

---

_Comment by @charliermarsh on 2024-12-23 00:35_

My guess is that authentication is failing somewhere. Can you share the (redacted) logs with `--verbose`?

---

_Comment by @tmatup on 2024-12-23 00:51_

@charliermarsh Please see attached for the log. `dm_shared` is the package name. I used the same credentials when doing `pip install`. And looks like it indeed found the path to the exact `whl` file that need to be downloaded and installed. Could the url (convention) be wrong in the `pyproject.toml` file?


---

_Comment by @charliermarsh on 2024-12-23 13:53_

Thank you! Just to clarify: are you using `mypackage = { index = "myindex" }` or `mypackage = { url = "https://url/to/wheel.whl" }`?

---

_Comment by @tmatup on 2024-12-23 15:41_

Hi @charliermarsh I am using the first one. I have the sample `pyproject.toml` content in my original post.

---

_Comment by @charliermarsh on 2024-12-23 15:47_

Sorry to ask this, but can you share the logs again with `-vvv`? They'll be pretty verbose but it should help understand whatever is happening with the authentication. You can also email them to me (my first name at our company domain) if you prefer.

---

_Comment by @tmatup on 2024-12-23 16:37_

Thanks @charliermarsh! Just emailed you the `-vvv` log output. 

---

_Comment by @tmatup on 2024-12-23 20:40_

Hi @charliermarsh so I looked into the log file, I don't see authentication errors, and couple things that looked interesting to me - first, the Expected vs. Actual which has the exact path and details for the whl file, how was it get found out and why was it not able to get used for the downloading and installation? second, why the package name were changed from having `_` to having `-`? 

I also checked the scope for the PAT used, it seems have the right scope. And the fact that I was able to do it thru `pip install --extra-index-ur` w/o any problem is also a positive sign. 

Is there anything else that I should look at for resolving this?

---

_Comment by @charliermarsh on 2024-12-23 21:31_

I also say the expected vs. actual. Do you have an existing `uv.lock` in the directory that used `mypackage = { url = "https://url/to/wheel.whl" }` or something? That's why I asked above -- it looks like uv is comparing against that. It shouldn't really matter though.

(The change from `_` to `-` also doesn't matter. That's just us normalizing the package name based on Python's standards.)

Does `https://myorg.pkgs.visualstudio.com/myproject/_packaging/myfeed/pypi/simple/dm-shared` look like it has the correct output?

---

_Comment by @charliermarsh on 2024-12-23 21:35_

Can you run with `RUST_LOG=trace`?

I don't think authentication is failing, since otherwise we'd see a hint at the end indicating that the index returned a 401.

---

_Comment by @tmatup on 2024-12-23 22:57_

Hi @charliermarsh , yes, `https://myorg.pkgs.visualstudio.com/myproject/_packaging/myfeed/pypi/simple/dm-shared` in browser does show the correct output. 

I went on to another machine and did a fresh start, now the expected vs actual section is gone from the log (so it much got picked up from somewhere, though I didn't see anything under the project folder). But I am still getting the same error.

  × No solution found when resolving dependencies for split (python_full_version >= '3.10' and sys_platform == 'darwin'):
  ╰─▶ Because mypackage was not found in the package registry and sandbox depends on mypackage ==1.0.1024, we can conclude that sandbox's requirements are unsatisfiable.
      And because your workspace requires sandbox, we can conclude that your workspace's requirements are unsatisfiable.

I looked into the log with RUST_LOG=trace turned on, it indeed ran into authentication (401) error when trying to access the url path. And I do have `UV_INDEX_MYFEED_USERNAME` and `UV_INDEX_MYFEED_PASSWORD` and also `AZURE_PACKAGES_TOKE` declared in my bashrc file.

I shared you the log via email. Please take a look.

---

_Comment by @charliermarsh on 2024-12-23 23:00_

Okay great, thank you. Taking a look...

---

_Comment by @tmatup on 2024-12-24 03:11_

@charliermarsh I ended up adding env variables for all of following to get it work

```
UV_INDEX_MYFEED_USERNAME
UV_INDEX_MYFEED_PASSWORD
UV_HTTP_BASIC_MYFEED_USERNAME
UV_HTTP_BASIC_MYFEED_PASSWORD
```

---

_Comment by @charliermarsh on 2024-12-24 03:15_

Ok! I'm fairly certain that we don't look at the `UV_HTTP_BASIC_` variables (we don't have any references in our codebase), but I think there were some old versions that did use that variant. Your logs suggest a recent version, though.

---

_Comment by @tmatup on 2024-12-24 03:44_

I went with the same set of env variables for my docker build as well, which installs `uv` thru the following command

```
RUN /opt/virtual_env/bin/pip install --upgrade pip
RUN /opt/virtual_env/bin/pip install uv
RUN /opt/virtual_env/bin/uv --version
```

---

_Closed by @charliermarsh on 2024-12-27 13:44_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-27 13:44_

---
