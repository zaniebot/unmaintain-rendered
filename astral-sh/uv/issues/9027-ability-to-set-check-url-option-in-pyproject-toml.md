```yaml
number: 9027
title: "Ability to set `--check-url` option in `pyproject.toml` or `uv.toml`"
type: issue
state: closed
author: bruckner
labels:
  - configuration
  - needs-decision
assignees: []
created_at: 2024-11-11T18:53:19Z
updated_at: 2024-12-02T23:30:14Z
url: https://github.com/astral-sh/uv/issues/9027
synced_at: 2026-01-10T04:36:20Z
```

# Ability to set `--check-url` option in `pyproject.toml` or `uv.toml`

---

_Issue opened by @bruckner on 2024-11-11 18:53_

I'm using the new `--check-url` feature introduced in https://github.com/astral-sh/uv/pull/8531 to support to ability to `uv publish` skip uploads when an artifact already exists. It's great!

This is a request for a minor enhancement: to be able to set `check-url` in a configuration file instead of only on the command line.

Running with the parameter set on the command line I get the expected behavior:
```
$ uv publish --check-url https://<my-pypi-url> --verbose
...snip...
File <my-package>+gf9b8e7c45.d20241111.tar.gz already exists, skipping
```

But when I tried moving it to my `pyproject.toml`:
```
[tool.uv]
publish-url = "<my-pypi-url>"
check-url = "https://<my-pypi-url>"
```

And got the warning message:
```
$ uv publish --verbose
...snip...
warning: Failed to parse `pyproject.toml` during settings discovery:
  TOML parse error at line 22, column 1
     |
  22 | check-url = "https://<my-pypi-url>"
     | ^^^^^^^^^
  unknown field `check-url`, expected one of `native-tls`, `offline`, `no-cache`, `cache-dir`, `preview`, `python-preference`, `python-downloads`, `concurrent-downloads`, `concurrent-builds`, `concurrent-installs`, `index`, `index-url`, `extra-index-url`, `no-index`, `find-links`, `index-strategy`, `keyring-provider`, `allow-insecure-host`, `resolution`, `prerelease`, `dependency-metadata`, `config-settings`, `no-build-isolation`, `no-build-isolation-package`, `exclude-newer`, `link-mode`, `compile-bytecode`, `no-sources`, `upgrade`, `upgrade-package`, `reinstall`, `reinstall-package`, `no-build`, `no-build-package`, `no-binary`, `no-binary-package`, `publish-url`, `trusted-publishing`, `pip`, `cache-keys`, `override-dependencies`, `constraint-dependencies`, `environments`, `workspace`, `sources`, `managed`, `package`, `default-groups`, `dev-dependencies`
```

The expected behavior for this enhancement would be that setting `check-url` in a configuration file has the same effect as if it were set on the command line.

For reference, I've seen the behavior described above with uv `0.5.0` and `0.5.1` on macos and ubuntu.

Thank you!

---

_Comment by @zanieb on 2024-11-11 19:30_

I believe this would be solved by my suggestion at https://github.com/astral-sh/uv/pull/8531#issuecomment-2442698889

---

_Assigned to @konstin by @zanieb on 2024-11-11 19:30_

---

_Comment by @charliermarsh on 2024-11-11 19:40_

Oh that's a nice solution.

---

_Comment by @bruckner on 2024-11-11 19:51_

@zanieb thanks for the super quick uptake. I like the suggestion to consolidate under the Index config, partly for reasons tangential to this issue. I use Google Artifact Registry and have to configure auth two distinct ways for read and write (username in the URL for reads, username via `--username` for writes). This was confusing to get working initially—would be really nice to consolidate all URLs and auth config under one localized umbrella for the Index.

That said, a quick fix for this issue would be valuable pending any discussion planning required for something like the above.

---

_Comment by @bruckner on 2024-11-11 21:19_

@konstin - when I looked at your code I saw this appears to be a trivial extension so I put up a PR ^

---

_Label `configuration` added by @charliermarsh on 2024-11-12 17:34_

---

_Label `needs-decision` added by @charliermarsh on 2024-11-12 17:34_

---

_Comment by @bruckner on 2024-11-25 23:34_

@zanieb I understand there are internal discussions about next steps here and saw your comment on the PR. I don't want to preempt that process, but how would you feel about landing the simple extension while that plays out? It wouldn't create any additional interface exposure beyond what's already in, and would be valuable improvement for build configuration in projects like mine. Let me know what you think, I'll be happy to rebase and get the PR rolling if you give the green light.

---

_Comment by @zanieb on 2024-11-26 00:37_

Ideally, we'd just deliver the full feature in the very near future. Let me check back in with the team to see where that's at. 

---

_Comment by @konstin on 2024-11-26 08:32_

I'm sorry to delay this for now, but i'm hesistant to merge since i want to see how `--index` would play out against this, it might be better to only advertise `--index`.

---

_Comment by @zanieb on 2024-11-26 18:26_

We talked a bit more about this — the `--index` work is probably a ways out and we already have a `publish_url` configuration option so  I think this makes sense to add for short-term feature parity. We may hide it in the future, once we implement the new interface. If you want to rebase, I'll review the pul request again.

---

_Comment by @bruckner on 2024-12-01 05:35_

@konstin @zanieb - thanks for the discussion, sounds like a plan. I rebased the PR.

---

_Closed by @zanieb on 2024-12-02 23:30_

---

_Closed by @zanieb on 2024-12-02 23:30_

---
