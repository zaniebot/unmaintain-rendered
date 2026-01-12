```yaml
number: 7484
title: "Please add socks5 support via the `reqwest` rust package"
type: issue
state: closed
author: rh314
labels: []
assignees: []
created_at: 2024-09-18T05:00:14Z
updated_at: 2024-09-18T17:20:09Z
url: https://github.com/astral-sh/uv/issues/7484
synced_at: 2026-01-12T15:59:14Z
```

# Please add socks5 support via the `reqwest` rust package

---

_@rh314_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
I've been trying to get `uv` to connect via a socks5 proxy. To my surprise, I could not get it working. After some digging, I see that the `reqwest` does support `socks5`, but you would need to add it explicitly to [`Cargo.toml`](https://github.com/astral-sh/uv/blob/fda2276/Cargo.toml#L125) as per [this example](https://github.com/seanmonstar/reqwest/issues/790#issuecomment-576237254).

Here is what I have tried so far (minimal code snippet that reproduces the bug):
```shell
# Set up proxy variables as per documentation
MY_SOCKS_PROXY=socks5://127.0.0.1:8080   # a proxy provided by an SSH connection
export HTTP_PROXY=$MY_SOCKS_PROXY
export HTTPS_PROXY=$MY_SOCKS_PROXY
export ALL_PROXY=$MY_SOCKS_PROXY
# then,
uv ...   # any uv command
```
The error messages varied depending on what I tried, but it was usually something along the lines of `Caused by: error sending request for url (https://REDACTED/simple/REDACTED/)`

Version:
```
$ uv --version
uv 0.4.0
```
Summary:
* Can we please [add `"socks"`](https://github.com/seanmonstar/reqwest/issues/790#issuecomment-576237254) in the line that specifies `reqwests` in the [`Cargo.toml`](https://github.com/astral-sh/uv/blob/fda2276/Cargo.toml#L125) file? Or some other method to allow the use of socks-based proxies :pray: 


---

_Comment by @rh314 on 2024-09-18 05:03_

For completeness' sake, I'm also going to share my current workaround.

I'm currently layering another proxy (an HTTP proxy) on top of the SOCKS5 proxy to work around this. To this end, I'm using `privoxy`.

Here is a snippet of my `privoxy_config` file:
```
# Listen on port 8081
listen-address  127.0.0.1:8081

# Allow CONNECT method for HTTPS traffic
enable-remote-toggle  1
enable-remote-http-toggle  1
enable-edit-actions  1

# Forward all traffic through the SOCKS5 proxy
forward-socks5 / 127.0.0.1:8080 .

# start command:
#   privoxy --no-daemon /REDACTED/privoxy_config
```

Then I just set the `*PROXY*` env variables to use port 8081.

---

_Closed by @charliermarsh on 2024-09-18 15:46_

---

_Comment by @rh314 on 2024-09-18 17:20_

Thanks!

---
