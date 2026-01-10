---
number: 10878
title: Add option to save pypi token with cli command
type: issue
state: closed
author: thewh1teagle
labels:
  - enhancement
assignees: []
created_at: 2025-01-22T22:26:52Z
updated_at: 2025-12-18T15:03:25Z
url: https://github.com/astral-sh/uv/issues/10878
synced_at: 2026-01-10T01:24:58Z
---

# Add option to save pypi token with cli command

---

_Issue opened by @thewh1teagle on 2025-01-22 22:26_

### Summary

`uv auth login` will be useful to enter pypi token once and save it automatically in uv configuration. currently I need to declare the pypi token in environment variable each time when I publish.
we can take inspiration from github cli.

### Example

```console
uv auth login
> Please open https://pypi.org/manage/account/token and create new token and then paste it here. Finally - press Enter
> *********************
> Success! token saved in ~/.uv-token
```

---

_Label `enhancement` added by @thewh1teagle on 2025-01-22 22:26_

---

_Comment by @MithicSpirit on 2025-01-22 22:49_

There are precedents for this kind of behavior. For example, github-cli has a similar feature with `gh auth login`.

---

_Referenced in [davep/hike#125](../../davep/hike/pulls/125.md) on 2025-08-18 10:41_

---

_Comment by @asmaier on 2025-10-31 14:43_

In the meantime `uv` has added and `uv auth login` cli command, see https://docs.astral.sh/uv/concepts/authentication/cli/ .

To use it with pypi do the following
```
$ uv auth login upload.pypi.org                      
username: __token__
password: 
```
As username you have to enter `__token__`. As password copy-paste your token from pypi. 
To publish your packages on pypi you can then simply do
```
$ uv publish --username __token__
```

> [!CAUTION]
>  Be aware that this will store your token **unencrypted** in your home directory. You can see it in clear text at
> ```
> $ less ~/.local/share/uv/credentials/credentials.toml
> ```



---

_Comment by @konstin on 2025-12-18 15:03_

We're working on improving the auth experience to make it more ergonomic and have better keyring integration, but the general feature now exists.

---

_Closed by @konstin on 2025-12-18 15:03_

---
