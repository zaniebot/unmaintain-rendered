---
number: 11849
title: "Failed to download https://github.com/astral-sh/uv/releases/download/0.6.3/uv-x86_64-unknown-linux-gnu.tar.gz"
type: issue
state: closed
author: rmndrs89
labels:
  - question
assignees: []
created_at: 2025-02-28T09:23:07Z
updated_at: 2025-03-11T09:53:12Z
url: https://github.com/astral-sh/uv/issues/11849
synced_at: 2026-01-10T01:25:11Z
---

# Failed to download https://github.com/astral-sh/uv/releases/download/0.6.3/uv-x86_64-unknown-linux-gnu.tar.gz

---

_Issue opened by @rmndrs89 on 2025-02-28 09:23_

### Summary

On my Ubuntu 22.04 LTS system, when running the code to install uv (see: [here](https://docs.astral.sh/uv/getting-started/installation/#standalone-installer)), I get the following network error:

```bash
$ curl -LsSf https://astral.sh/uv/install.sh | sh
downloading uv 0.6.3 x86_64-unknown-linux-gnu
curl: (23) client returned ERROR on write of 1369 bytes
failed to download https://github.com/astral-sh/uv/releases/download/0.6.3/uv-x86_64-unknown-linux-gnu.tar.gz
this may be a standard network error, but it may also indicate
that uv's release process is not working. When in doubt
please feel free to open an issue!
```

I have tried to run the command also as superuser, but I get the same error. Should I try an older version of uv? Do you recommend installing uv only within an existing virtual environment?

BR
Robbin

### Platform

Ubuntu 22.04.5 LTS Linux 6.8.0-52-generic x86_64 GNU/Linux

### Version

uv 0.6.3 x86_64-unknown-linux-gnu

### Python version

_No response_

---

_Label `bug` added by @rmndrs89 on 2025-02-28 09:23_

---

_Comment by @konstin on 2025-02-28 09:25_

From machine, I'm able to download the file. Can you manually download https://github.com/astral-sh/uv/releases/download/0.6.3/uv-x86_64-unknown-linux-gnu.tar.gz,in the browser and/or in the terminal (e.g. `wget https://github.com/astral-sh/uv/releases/download/0.6.3/uv-x86_64-unknown-linux-gnu.tar.gz`)? Are you in a network with specific firewall or proxy settings?

---

_Comment by @rmndrs89 on 2025-02-28 09:46_

I can download the archive from the repository without any problem. I am not in a network with specific firewall or proxy settings, as far as I know.

---

_Label `bug` removed by @konstin on 2025-02-28 09:51_

---

_Label `needs-mre` added by @konstin on 2025-02-28 09:51_

---

_Comment by @konstin on 2025-02-28 09:52_

That's odd, I can't reproduce this on my end, neither my dev machine (with ubuntu 24.04 x86_64) nor a server I tried.

---

_Comment by @cyril-ponce on 2025-02-28 16:38_

I had this same issue with curl installed with snap, and I found this  : https://askubuntu.com/questions/1356327/cant-write-to-a-hidden-path-using-curl

`sudo snap remove curl`
`sudo apt install curl`

---

_Comment by @rmndrs89 on 2025-03-03 07:43_

> I had this same issue with curl installed with snap, and I found this : https://askubuntu.com/questions/1356327/cant-write-to-a-hidden-path-using-curl
> 
> `sudo snap remove curl` `sudo apt install curl`

Wow, this indeed solved the issue, though I don't quite understand why.

```bash
robbin@Z390:~$ sudo snap remove curl
robbin@Z390:~$ sudo apt install curl
robbin@Z390:~$ curl -LsSf https://astral.sh/uv/install.sh | sh
downloading uv 0.6.3 x86_64-unknown-linux-gnu
no checksums to verify
installing to /home/robbin/.local/bin
  uv
  uvx
everything's installed!
robbin@Z390:~$ uv
An extremely fast Python package manager.

Usage: uv [OPTIONS] <COMMAND>

...
```

Thanks!

---

_Label `needs-mre` removed by @konstin on 2025-03-03 10:51_

---

_Label `question` added by @konstin on 2025-03-03 10:51_

---

_Comment by @konstin on 2025-03-03 10:51_

Closing this as it looks like a problem with the curl installation instead of uv's install script.

---

_Closed by @konstin on 2025-03-03 10:51_

---

_Comment by @humphreyde on 2025-03-11 09:53_

> > I had this same issue with curl installed with snap, and I found this : https://askubuntu.com/questions/1356327/cant-write-to-a-hidden-path-using-curl
> > `sudo snap remove curl` `sudo apt install curl`
> 
> Wow, this indeed solved the issue, though I don't quite understand why.
> 
> robbin@Z390:~$ sudo snap remove curl
> robbin@Z390:~$ sudo apt install curl
> robbin@Z390:~$ curl -LsSf https://astral.sh/uv/install.sh | sh
> downloading uv 0.6.3 x86_64-unknown-linux-gnu
> no checksums to verify
> installing to /home/robbin/.local/bin
>   uv
>   uvx
> everything's installed!
> robbin@Z390:~$ uv
> An extremely fast Python package manager.
> 
> Usage: uv [OPTIONS] <COMMAND>
> 
> ...
> Thanks!

It really solved my problem, although my problem is : `sh uv-installer.sh` failed. It's really magic.

---

_Referenced in [astral-sh/uv#12728](../../astral-sh/uv/issues/12728.md) on 2025-04-27 03:17_

---

_Referenced in [boukendesho/curl-snap#1](../../boukendesho/curl-snap/issues/1.md) on 2025-04-29 08:25_

---

_Referenced in [rust-lang/rustup#4389](../../rust-lang/rustup/pulls/4389.md) on 2025-06-19 08:50_

---
