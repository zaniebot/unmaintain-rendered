---
number: 16983
title: Bail when trying to attach ambiguous credentials instead of picking an arbitrary one
type: pull_request
state: open
author: zsol
labels:
  - bug
  - breaking
assignees: []
base: main
head: zsol/jj-nyvrllosmoon
created_at: 2025-12-04T18:00:18Z
updated_at: 2025-12-08T12:21:43Z
url: https://github.com/astral-sh/uv/pull/16983
synced_at: 2026-01-07T13:12:19-06:00
---

# Bail when trying to attach ambiguous credentials instead of picking an arbitrary one

---

_Pull request opened by @zsol on 2025-12-04 18:00_

## Summary

This PR makes `TextCredentialStore.get_credentials` fail on ambiguous matches, instead of returning the first "best" match while iterating over all credentials.

## Test Plan

Added a unit test, plus tested via:

```
❯ jq -n '{uri: "https://foo"}' | cargo -q run auth helper --protocol=bazel get
error: Multiple credentials found for URL 'https://foo/', specify which username to use
```
(the above needed two username/password combos registered for `https://foo` using `uv auth login https://foo`)


---

_Review requested from @zanieb by @zsol on 2025-12-04 18:00_

---

_Review requested from @konstin by @zsol on 2025-12-04 18:00_

---

_Referenced in [astral-sh/uv#16886](../../astral-sh/uv/pulls/16886.md) on 2025-12-04 18:00_

---

_Label `bug` added by @zsol on 2025-12-04 19:06_

---

_Label `breaking` added by @zsol on 2025-12-04 19:06_

---

_Comment by @zsol on 2025-12-04 19:08_

I applied the `breaking` label because technically this might change might cause currently (accidentally) working setups to suddenly stop attaching credentials in cases when the credentials were ambiguous.

---

_Marked ready for review by @zsol on 2025-12-04 19:08_

---

_Comment by @konstin on 2025-12-05 18:39_

Given that this is a preview feature, I'd consider it a preview change instead of a breaking change.

---

_Comment by @zanieb on 2025-12-05 20:10_

Is the interface in preview? 🤔 

---

_Comment by @konstin on 2025-12-08 11:13_

I was thinking about this:

> warning: The `uv auth helper` command is experimental and may change without warning. Pass `--preview-features auth-helper` to disable this warning

If that affects stable code too then it's breaking change.



---

_Comment by @zanieb on 2025-12-08 12:17_

Yeah `uv auth token` should be affected by this change and is stable

---

_Comment by @zsol on 2025-12-08 12:21_

Yes it does, here's a repro:
```bash session
pushd $(mktemp -d)
# set up a basic auth pypi-server
htpasswd -bc passwords foo1 bar
uvx --python 3.12 --from pypiserver[passlib] pypi-server run -a 'download, list, update' -P passwords --log-stream stderr -v . &
# fetch a random wheel
curl -LO https://files.pythonhosted.org/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl
# configure uv with two usernames on the same url
uv auth login http://localhost:8080 -u foo1 --password=bar
uv auth login http://localhost:8080 -u foo2 --password=bar

uv venv --clear
# latest version of uv succeeds
uvx uv pip install --default-index http://localhost:8080 sniffio --no-cache
# with -vv you can see it's using `foo1` (on my machine)

# with this PR:
~/dev/uv/target/debug/uv pip install --default-index http://localhost:8080 anyio --no-cache

  × No solution found when resolving dependencies:
  ╰─▶ Because anyio was not found in the package registry and you require anyio, we can conclude that your requirements are unsatisfiable.

      hint: An index URL (http://localhost:8080/) could not be queried due to a lack of valid authentication credentials (401 Unauthorized).
```


---
