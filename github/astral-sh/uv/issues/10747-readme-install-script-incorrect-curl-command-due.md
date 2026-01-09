---
number: 10747
title: "`README` install script incorrect curl command due to 301 redirects"
type: issue
state: closed
author: GangGreenTemperTatum
labels:
  - needs-mre
assignees: []
created_at: 2025-01-19T01:38:19Z
updated_at: 2025-01-19T15:07:29Z
url: https://github.com/astral-sh/uv/issues/10747
synced_at: 2026-01-07T13:12:18-06:00
---

# `README` install script incorrect curl command due to 301 redirects

---

_Issue opened by @GangGreenTemperTatum on 2025-01-19 01:38_

thanks for the awesome project! :) 

fyi, within the `README` install instructions for downloading the install script ([here](https://github.com/astral-sh/uv/blob/c306e46e1d3342a923ab587a1e9d3313dddb3000/README.md#L53-L54)), this fails as  he curl command is downloading HTTP headers as well as the actual script content due to the redirects

https://github.com/astral-sh/uv/blob/c306e46e1d3342a923ab587a1e9d3313dddb3000/README.md#L51-L55

```bash
# On macOS and Linux.
curl -LsSf https://astral.sh/uv/install.sh | sh
```

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh

sh: line 1: HTTP/2: No such file or directory
sh: line 2: date:: command not found
sh: line 3: content-type:: command not found
sh: line 4: content-length:: command not found
sh: line 5: location:: command not found
sh: line 6: cache-control:: command not found
sh: line 7: expires:: command not found
sh: line 8: report-to:: command not found
sh: line 9: nel:: command not found
sh: line 10: vary:: command not found
sh: line 11: server:: command not found
sh: line 12: cf-ray:: command not found
: command not found
sh: line 14: HTTP/2: No such file or directory
sh: line 15: server:: command not found
```

the URL https://astral.sh/uv/install.sh redirects (HTTP 301) to a new location:
https://github.com/astral-sh/uv/releases/latest/download/uv-installer.sh. which is then redirected (HTTP 302) to a specific versioned installer download URL:
https://github.com/astral-sh/uv/releases/download/0.5.21/uv-installer.sh and the final URL points to an Azure Blob storage location

---

_Comment by @charliermarsh on 2025-01-19 02:42_

Unfortunately I can't reproduce this (and we've used these instructions successfully for a long time with a large number of users):

```
❯ curl -LsSf https://astral.sh/uv/install.sh | sh
downloading uv 0.5.21 aarch64-apple-darwin
no checksums to verify
installing to /Users/crmarsh/.local/bin
  uv
  uvx
everything's installed!
```

What shell are you using...? Something with your cURL version? Something in your network proxy? I have no idea.

---

_Label `needs-mre` added by @charliermarsh on 2025-01-19 02:42_

---

_Comment by @GangGreenTemperTatum on 2025-01-19 15:07_

@charliermarsh , my apologies i am super dumb! :( i didn't catch a `.curlrc` which was the outlier and root-cause here
my bad again, thanks for the awesome project and quick response!!

---

_Closed by @GangGreenTemperTatum on 2025-01-19 15:07_

---
