```yaml
number: 7939
title: "after I   uv tool  install   httpie  , I  try  to  run  \"uvx   http  google.com \"  ,but   happen  unexpected    error* "
type: issue
state: closed
author: Super1Windcloud
labels:
  - question
assignees: []
created_at: 2024-10-05T11:52:07Z
updated_at: 2025-12-05T08:16:57Z
url: https://github.com/astral-sh/uv/issues/7939
synced_at: 2026-01-12T15:59:18Z
```

# after I   uv tool  install   httpie  , I  try  to  run  "uvx   http  google.com "  ,but   happen  unexpected    error* 

---

_@Super1Windcloud_

 # win11  uv@latest  

##*after I   uv tool  install   httpie  , I  try  to  run  "uvx   http  google.com "  ,but   happen  unexpected    error* 


![image](https://github.com/user-attachments/assets/3c6dcc57-91f0-4526-8e39-010579146bb2)
 

---

_Comment by @tonnico on 2024-10-05 14:06_

You need to run `http` with `--from httpie`, else `uvx` will look for `http`.

```
uv tool run --from httpie http google.com
```

---

_Comment by @Super1Windcloud on 2024-10-05 14:31_

 thank you  very much  

---

_Label `question` added by @zanieb on 2024-10-05 14:43_

---

_Closed by @zanieb on 2024-10-21 21:55_

---

_Comment by @kennell on 2025-05-27 21:40_

For comfy httpie usage, add to your shell config:

```
alias http='uv tool run --from httpie http'
```


---

_Comment by @zanieb on 2025-05-27 21:54_

If you do `uv tool install httpie`, then `http` should "just be available" without `uvx`.

---

_Comment by @iddqd3 on 2025-12-05 08:16_

> If you do `uv tool install httpie`, then `http` should "just be available" without `uvx`.

yes, if you add to PATH `$HOME/.local/bin`

After installing the tool, uv will notify you if the paths are not specified in the PATH
```
....
Installed 3 executables: http, httpie, https
warning: `/home/username/.local/bin` is not on your PATH. To use installed tools, run `export PATH="/home/username/.local/bin:$PATH"` or `uv tool update-shell`.
```

---
