```yaml
number: 9134
title: How to properly setup a mirror for the standalone installer and updater?
type: issue
state: open
author: leodevian
labels:
  - question
assignees: []
created_at: 2024-11-14T23:52:33Z
updated_at: 2025-05-19T18:36:08Z
url: https://github.com/astral-sh/uv/issues/9134
synced_at: 2026-01-10T03:41:46Z
```

# How to properly setup a mirror for the standalone installer and updater?

---

_Issue opened by @leodevian on 2024-11-14 23:52_

Hello ! Noob here !

I would like to distribute uv using an internal repository and I don't understand how to set it up as uv expects it.

The base URL is `https://example.com/uv/` and it contains folders containing uv versions, such as `https://example.com/uv/0.5.1/` or `https://example.com/uv/0.5.2`. Is using `INSTALLER_DOWNLOAD_URL=https://example.com/uv/0.5.2` okay? It seems that the standalone installer works while the updater fails... Also, would you add the latest standalone installer at the root of the repository (inspired from uv docs)? Does the whole thing seem to be okay with you?

Thank you for awesome work!

---

_Comment by @charliermarsh on 2024-11-16 03:06_

@gaborbernat -- Sorry to ping but are you able to provide any insight here?

---

_Label `question` added by @charliermarsh on 2024-11-16 03:06_

---

_Comment by @gaborbernat on 2024-11-16 03:52_

That depends. Did we pull in and release the changes from cargo dist? 😂

---

_Comment by @charliermarsh on 2024-11-16 04:07_

Haha yeah, I believe so.

---

_Comment by @gaborbernat on 2024-11-16 04:18_

Do you know when that happened because I need a week to pass to validate it 😂 once that happens I can come back here with the details. 

---

_Comment by @charliermarsh on 2024-11-16 04:32_

I believe it was this PR: https://github.com/astral-sh/uv/pull/8873

Which I believe went out in v0.5: https://github.com/astral-sh/uv/releases/tag/0.5.0

---

_Comment by @leodevian on 2024-11-16 14:52_

I would like to provide better insights on my issue. Internal IT policies block requests to external websites such as `https://github.com` or `https://api.github.com`.

If I could, I would create a proxy repository for `https://github.com/astral-sh/uv/releases/download/`. `uv self update` would still fail, but it would enable installing uv using its standalone installer.

```bash
curl -LsSf https://example.com/uv/0.5.2/uv-installer.sh | env INSTALLER_DOWNLOAD_URL="https://example.com/uv/0.5.2" sh
```

As it is impossible and as there is no room for discussion, I have to download your artifacts and upload them myself.

I tried nesting release artifacts in `https://example.com/uv/astral-sh/uv/releases/download/` and using `UV_INSTALLER_GITHUB_BASE_URL="https://example.com/uv/"` but it does not work. `uv self update` would download artifacts from `https://api.example.com/uv/astral-sh/uv/releases/download/`, which does not exist.

I have no idea if what I'm doing makes any sense.

---

_Comment by @gaborbernat on 2025-01-17 21:44_

I finally got to this today, here's my findings.

To tell to `uv` to install the interpreters and itself from the internal mirrors you need to set two environment
variables, preferably within your shells start script:

| Environment variable        | Value                                       | Description                                                           |
| --------------------------- | ------------------------------------------- | --------------------------------------------------------------------- |
| `UV_INSTALLER_GHE_BASE_URL` | `https://magic.com`    | HTTP endpoint hosting uv, allowing easy installation and self update. |
| `UV_PYTHON_INSTALL_MIRROR`  | `https://magic.com/py` | HTTP endpoint hosting standalone builds of the Python interpreter.    |


Now when it comes to the file layout on the server you are looking at something like:

```
api/v3/repos/astral-sh/uv/releases
astral-sh/uv/releases/download/0.5.17/uv-aarch64-apple-darwin.tar.gz
astral-sh/uv/releases/download/0.5.17/uv-installer.ps1
astral-sh/uv/releases/download/0.5.17/uv-installer.sh
astral-sh/uv/releases/download/0.5.17/uv-x86_64-pc-windows-msvc.zip
astral-sh/uv/releases/download/0.5.17/uv-x86_64-unknown-linux-gnu.tar.gz
astral-sh/uv/releases/download/0.5.17/uv-x86_64-unknown-linux-musl.tar.gz
```

for the `uv` artifacts and:

```
py/20240726/cpython-3.12.4+20240726-aarch64-apple-darwin-install_only_stripped.tar.gz
py/20240726/cpython-3.12.4+20240726-x86_64-pc-windows-msvc-install_only_stripped.tar.gz
```

for the standalone `Python` builds.

Most of the files are straight mirror, minus one: `api/v3/repos/astral-sh/uv/releases`. This file is something you will need to rewrite, is a list of dictionaries and the following transformation is required:

```python
 for entry in content:
      for asset in entry["assets"]:  # fix the download URL to the internal link
          asset["browser_download_url"] = asset["browser_download_url"].replace(
              "https://github.com",
              "https://magic.com",
          )
```

Following that you should be good to go (works for me at least).

And with that it should work.

---

_Comment by @leodevian on 2025-01-18 14:48_

Thank you @gaborbernat 

I think that this should solve my issue! I guess that `api/v3/repos/astral-sh/uv/releases` is the same JSON that I get when I run `gh api repos/astral-sh/uv/releases`. I'm closing the issue as soon as I can try this at work.

---

_Comment by @leodevian on 2025-01-22 20:53_

Turns out that I cannot host the JSON on https://magic.com/api/v3/repos/astral-sh/uv/releases as I am using a Nexus repository. The URL for all repositories must start with https://magic.com/repository/... I guess that I will have to ask my users to make their updates manually from time to time. Thanks anyway, I really appreciate it!

Should I close the issue?

---

_Comment by @jamesharris-garmin on 2025-01-29 20:24_

It feels like a major oversight that we can't handle the case of hosting a nexus mirror repository.  Is there anything we can do to progress this?

---

_Comment by @gaborbernat on 2025-01-30 18:54_

This is provided by `cargo-dist`, so if you want some feature request to support your use case, you need to create an issue in their tracker. Make sure to link here the created issue.

---

_Comment by @jamesharris-garmin on 2025-01-30 22:11_

sounds good, I'll reach out to them.

---

_Comment by @jamesharris-garmin on 2025-01-30 22:16_

Looks like the original feature was added here: https://github.com/axodotdev/cargo-dist/issues/1495 (adding it for reference)

---

_Comment by @leodevian on 2025-02-13 13:48_

Do you think that I could bypass this issue using another domain and redirections? I think that I could do something similar that what is done for https://astral.sh but I don't know how to do it... If I could create redirections from https://wizardry.com/astral-sh/uv/releases/download to https://magic.com/repository/uv/ (which would redirect to https://github.com/astral-sh/uv/releases/download) and create https://wizardry.com/api/v3/repos/astral-sh/releases manually, would that work or is it non-sense? I have no idea how to implement this and I would love to know how it was done for https://astral.sh to try it by myself.

---

_Comment by @zanieb on 2025-02-13 16:16_

We use https://developers.cloudflare.com/rules/

---

_Comment by @leodevian on 2025-05-19 18:15_

Due to constraints on which I have no leverage, I cannot implement the solution provided by @gaborbernat, even though I believe that it is optimal.

Because of it, I developed a wrapper for the `uv self update` command. It uses an HTTP server and environment variables to redirect the requests of the updater to arbitrary URLs. I think that I'm going to distribute it as a package, so that it can be installed using `uv tool install uv-self-update` and called as a replacement for the `uv self update` command.

It is a hack, I'm not proud of it, but it might help other people in similar situations. Feedback or alternatives that don't involve setting up a server would be appreciated 🙏

```python
# /// script
# requires-python = ">=3.9"
# dependencies = [  ]
# ///
"""Update uv to its latest version."""

# ruff: noqa: D101, D102, D103

from __future__ import annotations

import os
import socket
import subprocess
import sys
import threading
from http.server import BaseHTTPRequestHandler, HTTPServer
from typing import Any
from urllib.parse import urlparse


class RequestHandler(BaseHTTPRequestHandler):
    # Redirect requests to the appropriate URLs.
    # https://docs.python.org/3/library/http.server.html#http.server.BaseHTTPRequestHandler
    def do_GET(self) -> None:  # noqa: N802
        parsed = urlparse(self.path)

        if parsed.path == "/api/v3/repos/astral-sh/uv/releases/latest":
            latest_release_url = os.getenv(
                "LATEST_RELEASE_URL",
                "https://api.github.com/repos/astral-sh/uv/releases/latest",
            )
            self.send_response(301)
            self.send_header("Location", latest_release_url)
            self.end_headers()

        elif parsed.path.startswith("/astral-sh/uv/releases/download/"):
            releases_download_url = os.getenv(
                "RELEASES_DOWNLOAD_URL",
                "https://github.com/astral-sh/uv/releases/download/",
            )
            path = parsed.path.partition("/download/")[2]
            self.send_response(301)
            self.send_header("Location", f"{releases_download_url}{path}")
            self.end_headers()

        else:
            self.send_response(404)
            self.end_headers()

    # Suppress the logging of requests.
    # https://docs.python.org/3/library/http.server.html#http.server.BaseHTTPRequestHandler.log_message
    def log_message(self, format: str, *args: Any) -> None:  # noqa: A002
        pass


def main(host: str = "127.0.0.1") -> int:
    # Request an unused local port from the operating system.
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
        s.bind((host, 0))
        port = s.getsockname()[1]

    # Start a local HTTP server to intercept requests of the updater.
    with HTTPServer((host, port), RequestHandler) as server:
        # Run the server in a daemon thread so that it stops with the main program.
        thread = threading.Thread(target=server.serve_forever, daemon=True)
        thread.start()

        try:
            # Remove environment variables that could interfere with the update.
            # https://docs.astral.sh/uv/configuration/environment/
            for env in os.environ:
                if env.startswith("UV_INSTALLER_"):
                    del os.environ[env]

            # Set the updater to use the local server.
            # https://docs.astral.sh/uv/configuration/environment/#uv_installer_ghe_base_url
            os.environ["UV_INSTALLER_GHE_BASE_URL"] = f"http://{host}:{port}"

            # Run the updater and return the exit code.
            result = subprocess.run(("uv", "self", "update", *sys.argv[1:]))  # noqa: PLW1510, S603
            return result.returncode

        finally:
            server.shutdown()


if __name__ == "__main__":
    raise SystemExit(main())
```

---
