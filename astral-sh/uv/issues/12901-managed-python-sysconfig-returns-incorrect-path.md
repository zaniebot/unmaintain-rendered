---
number: 12901
title: Managed Python sysconfig returns incorrect path to CC on Amazon Linux 2023
type: issue
state: closed
author: nwalters512
labels:
  - bug
assignees: []
created_at: 2025-04-15T18:02:39Z
updated_at: 2025-09-12T13:16:51Z
url: https://github.com/astral-sh/uv/issues/12901
synced_at: 2026-01-10T01:25:26Z
---

# Managed Python sysconfig returns incorrect path to CC on Amazon Linux 2023

---

_Issue opened by @nwalters512 on 2025-04-15 18:02_

### Summary

This simplest way to reproduce this is to build the following simple Docker image:

```Dockerfile
FROM amazonlinux:2023

RUN dnf install -y gcc g++ tar

RUN curl -LO https://astral.sh/uv/install.sh \
    && sh install.sh \
    && rm install.sh \
    && source $HOME/.local/bin/env \
    && uv version \
    && uv venv --python 3.10 \
    && source ./.venv/bin/activate \
    && python3 -c 'import sysconfig; print(sysconfig.get_config_var("CC"))'
```

That ultimately logs `/usr/bin/aarch64-linux-gnu-gcc`, but on this system, the correct path is `/usr/bin/aarch64-amazon-linux-gcc`.

If we add the following command, we can see that `CC` indeed points at the wrong path:

```sh
cat /root/.local/share/uv/python/cpython-3.10.17-linux-aarch64-gnu/lib/python3.10/_sysconfigdata__linux_aarch64-linux-gnu.py
```

```py
# system configuration generated and used by the sysconfig module
build_time_vars = {
    # omitted for brevity...
    "CC": "/usr/bin/aarch64-linux-gnu-gcc",
    # omitted for brevity...
}
```

To reproduce an actual error that stems from this, build the following Docker image, which installs `pygraphviz`, which in turn needs to be built with `gcc`:

```Dockerfile
FROM amazonlinux:2023

RUN dnf install -y gcc g++ tar

RUN curl -LO https://astral.sh/uv/install.sh \
    && sh install.sh \
    && rm install.sh \
    && source $HOME/.local/bin/env \
    && uv venv --python 3.10 \
    && uv pip install pygraphviz==1.14
```

This ultimately fails with the following:

```
error: command '/usr/bin/aarch64-linux-gnu-gcc' failed: No such file or directory
```


### Platform

Amazon Linux 2023 aarch64 (Linux 6.10.14-linuxkit aarch64 GNU/Linux)

### Version

uv 0.6.14

### Python version

3.10.17

---

_Label `bug` added by @nwalters512 on 2025-04-15 18:02_

---

_Referenced in [PrairieLearn/PrairieLearn#11039](../../PrairieLearn/PrairieLearn/pulls/11039.md) on 2025-04-15 18:02_

---

_Comment by @zanieb on 2025-04-16 13:32_

I think this is fixed by https://github.com/astral-sh/uv/pull/12239 (which I just merged)

---

_Assigned to @zanieb by @zanieb on 2025-04-16 13:36_

---

_Comment by @nwalters512 on 2025-04-22 18:12_

@zanieb I just tested my above repros with the latest release of `uv`.

- In the first example, it now logs `cc` (which I think is what I expect).
- In the second one, I'm still seeing the same error, though this time things get further than they did before. Note that I added a step to install `graphviz` and `graphviz-devel` with `dnf`, so the full Dockerfile is now this:

```Dockerfile
FROM amazonlinux:2023

RUN dnf install -y gcc g++ tar graphviz graphviz-devel

RUN curl -LO https://astral.sh/uv/install.sh \
    && sh install.sh \
    && rm install.sh \
    && source $HOME/.local/bin/env \
    && uv venv --python 3.10 \
    && uv pip install pygraphviz==1.14
```

After `cat`ing the same file in the first example, I see there are still a number of sysconfig vars that reference the path `/usr/bin/aarch64-linux-gnu-gcc`. Here's a list of them:

```
"BLDSHARED": "/usr/bin/aarch64-linux-gnu-gcc -shared -Wl,--exclude-libs,ALL",
"CONFIG_ARGS": "'--build=x86_64-unknown-linux-gnu' '--host=aarch64-unknown-linux-gnu' '--prefix=/install' '--with-openssl=/tools/deps' '--with-system-expat' '--with-system-libmpdec' '--without-ensurepip' '--with-readline=editline' '--enable-shared' '--with-lto' '--with-dbmliborder=bdb' 'ac_cv_buggy_getaddrinfo=no' 'ac_cv_file__dev_ptc=no' 'ac_cv_file__dev_ptmx=no' 'ac_cv_working_tzset=yes' 'build_alias=x86_64-unknown-linux-gnu' 'host_alias=aarch64-unknown-linux-gnu' 'CC=/usr/bin/aarch64-linux-gnu-gcc' 'CFLAGS= -fPIC ' 'LDFLAGS= -Wl,--exclude-libs,ALL' 'CPPFLAGS= -fPIC ' 'PKG_CONFIG=pkg-config --static' 'PKG_CONFIG_PATH=/tools/deps/share/pkgconfig:/tools/deps/lib/pkgconfig'",
"LDSHARED": "/usr/bin/aarch64-linux-gnu-gcc -shared -Wl,--exclude-libs,ALL",
"LINKCC": "/usr/bin/aarch64-linux-gnu-gcc",
"MAINCC": "/usr/bin/aarch64-linux-gnu-gcc",
```

I'm unsure exactly which of these `pygraphziz` is referencing.

---

_Comment by @zanieb on 2025-04-22 21:14_

Thanks! I can take a look at that when I'm back from vacation. Feel free to poke me if I don't reply next week. 

---

_Comment by @nwalters512 on 2025-04-22 21:17_

Sorry for the ping, enjoy your time off! Definitely stop replying to this for now 🙂

---

_Comment by @zanieb on 2025-05-08 20:29_

It looks like they read `LDSHARED`. I guess we need to update more variables. cc @samypr100 / @bluss 

---

_Comment by @zanieb on 2025-05-08 20:32_

You can unblock yourself with `ENV LDSHARED="cc -shared"`

---

_Referenced in [astral-sh/uv#13441](../../astral-sh/uv/pulls/13441.md) on 2025-05-14 02:29_

---

_Comment by @nwalters512 on 2025-09-12 00:26_

@zanieb I'm inclined to call this closed; I can now successfully install and build `pygraphviz` in an `amazonlinux:2023` Docker image! Note that because things now get farther, I had to add a few more things to install with `dnf`. Here's the latest, working Dockerfile:

```Dockerfile
FROM amazonlinux:2023

RUN dnf install -y gcc g++ tar gzip graphviz graphviz-devel

RUN curl -LO https://astral.sh/uv/install.sh \
    && sh install.sh \
    && rm install.sh \
    && source $HOME/.local/bin/env \
    && uv venv --python 3.10 \
    && uv pip install pygraphviz==1.14

```

I'll close this out, feel free to reopen if you need this to track any remaining work!

---

_Closed by @nwalters512 on 2025-09-12 00:26_

---

_Comment by @zanieb on 2025-09-12 13:16_

Thanks for the update!

---
