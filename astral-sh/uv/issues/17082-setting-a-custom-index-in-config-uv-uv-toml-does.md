---
number: 17082
title: Setting a custom index in ~/.config/uv/uv.toml does not work
type: issue
state: closed
author: moritzdietz
labels:
  - question
assignees: []
created_at: 2025-12-11T11:00:27Z
updated_at: 2025-12-11T11:58:18Z
url: https://github.com/astral-sh/uv/issues/17082
synced_at: 2026-01-10T01:26:13Z
---

# Setting a custom index in ~/.config/uv/uv.toml does not work

---

_Issue opened by @moritzdietz on 2025-12-11 11:00_

### Question

When I set this in `~/.config/uv/uv.toml`:

```
[[tool.uv.index]]
name = "Nexus"
url = "https://nexus.contoso.com/service/rest/repository/browse/pypi/"
default = true
native-tls = true
```

and run 
```
uv tool install --with-executables-from ansible-core,ansible-lint ansible
```
I get:
```
error: Failed to parse: `.config/uv/uv.toml`
  Caused by: TOML parse error at line 1, column 3
  |
1 | [[tool.uv.index]]
  |   ^^^^
unknown field `tool`, expected one of `required-version`, `native-tls`, `offline`, `no-cache`, `cache-dir`, `preview`, `python-preference`, `python-downloads`, `concurrent-downloads`, `concurrent-builds`, `concurrent-installs`, `index`, `index-url`, `extra-index-url`, `no-index`, `find-links`, `index-strategy`, `keyring-provider`, `allow-insecure-host`, `resolution`, `prerelease`, `fork-strategy`, `dependency-metadata`, `config-settings`, `config-settings-package`, `no-build-isolation`, `no-build-isolation-package`, `extra-build-dependencies`, `extra-build-variables`, `exclude-newer`, `exclude-newer-package`, `link-mode`, `compile-bytecode`, `no-sources`, `upgrade`, `upgrade-package`, `reinstall`, `reinstall-package`, `no-build`, `no-build-package`, `no-binary`, `no-binary-package`, `python-install-mirror`, `pypy-install-mirror`, `python-downloads-json-url`, `publish-url`, `trusted-publishing`, `check-url`, `add-bounds`, `pip`, `cache-keys`, `override-dependencies`, `exclude-dependencies`, `constraint-dependencies`, `build-constraint-dependencies`, `environments`, `required-environments`, `conflicts`, `workspace`, `sources`, `managed`, `package`, `default-groups`, `dependency-groups`, `dev-dependencies`, `build-backend`
```

I can only use a mirror for anything from Pypy and would like to use uv to install things from there.
Would love some pointers of what I'm doing wrong here :) 

I did read the documentation from here https://docs.astral.sh/uv/concepts/configuration-files/ and it says

> For tool commands, which operate at the user level, local configuration files will be ignored. Instead, uv will exclusively read from user-level configuration (e.g., ~/.config/uv/uv.toml) and system-level configuration (e.g., /etc/uv/uv.toml).

but then again in other documents it seems that this is something to be set in a project file?!

### Platform

Ubuntu 24.04 amd64

### Version

uv 0.9.17

---

_Label `question` added by @moritzdietz on 2025-12-11 11:00_

---

_Comment by @konstin on 2025-12-11 11:39_

In `uv.toml`, it's just `[[index]]`, while `[[tool.uv.index]]` is for `pyproject.toml`.

---

_Comment by @moritzdietz on 2025-12-11 11:54_

Thanks for getting back to me so quickly! 

I updated the conf to this:
```
❯ cat .config/uv/uv.toml
[[index]]
name = "Nexus"
url = "https://nexus.contoso.com/service/rest/repository/browse/pypi/"
default = true
native-tls = true
```

but this still fails:
```
❯ uv tool install --with-executables-from ansible-core,ansible-lint ansible
  × No solution found when resolving dependencies:
  ╰─▶ Because there are no versions of ansible and you require ansible, we can conclude that your requirements are unsatisfiable.
```

I do have pip installed as well and there this URL does work as I thought it might be a typo in the URL to our nexus.
```
❯ cat .config/pip/pip.conf
[global]
cert = /etc/ssl/certs/ca-certificates.crt
index = https://nexus.contoso.com/repository/pypi/
index-url = https://nexus.contoso.com/repository/pypi/simple
trusted-host = https://nexus.contoso.com

❯ pip list --outdated
Package                   Version   Latest   Type
------------------------- --------- -------- -----
ansible                   11.2.0    13.1.0   wheel
ansible-compat            25.1.2    25.12.0  wheel
ansible-core              2.18.2    2.20.1   wheel
ansible-lint              25.1.2    25.12.1  wheel
attrs                     24.3.0    25.4.0   wheel
[...]
```



---

_Comment by @moritzdietz on 2025-12-11 11:58_

Ah I see it now! I got it working after all with this:

```
[[index]]
name = "Nexus"
url = "https://nexus.contoso.com/repository/pypi/simple"
default = true
native-tls = true
```

```
❯ uv tool install --with-executables-from ansible-core,ansible-lint ansible
Resolved 32 packages in 47.51s
Prepared 32 packages in 2.76s
Installed 32 packages in 369ms
 + ansible==13.1.0
 + ansible-compat==25.12.0
 + ansible-core==2.20.1
 + ansible-lint==25.12.1
 + attrs==25.4.0
 + black==25.12.0
 + bracex==2.6
 + cffi==2.0.0
 + click==8.3.1
 + cryptography==46.0.3
 + distro==1.9.0
 + filelock==3.20.0
 + jinja2==3.1.6
 + jsonschema==4.25.1
 + jsonschema-specifications==2025.9.1
 + markupsafe==3.0.3
 + mypy-extensions==1.1.0
 + packaging==25.0
 + pathspec==0.12.1
 + platformdirs==4.5.1
 + pycparser==2.23
 + pytokens==0.3.0
 + pyyaml==6.0.3
 + referencing==0.37.0
 + resolvelib==1.2.1
 + rpds-py==0.30.0
 + ruamel-yaml==0.18.16
 + ruamel-yaml-clib==0.2.15
 + subprocess-tee==0.4.2
 + typing-extensions==4.15.0
 + wcmatch==10.1
 + yamllint==1.37.1
Installed 10 executables from `ansible-core`: ansible, ansible-config, ansible-console, ansible-doc, ansible-galaxy, ansible-inventory, ansible-playbook, ansible-pull, ansible-test, ansible-vault
Installed 1 executable from `ansible-lint`: ansible-lint
Installed 1 executable: ansible-community
```


---

_Closed by @moritzdietz on 2025-12-11 11:58_

---
