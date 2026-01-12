```yaml
number: 16209
title: "Generated shebang lines output 'realpath: --: No such file or directory'"
type: issue
state: open
author: maxwell-k
labels:
  - bug
  - compatibility
  - great writeup
assignees: []
created_at: 2025-10-09T15:31:01Z
updated_at: 2025-11-27T13:29:48Z
url: https://github.com/astral-sh/uv/issues/16209
synced_at: 2026-01-12T16:02:26Z
```

# Generated shebang lines output 'realpath: --: No such file or directory'

---

_@maxwell-k_

Example command to reproduce on Alpine Linux:

    uv tool run --from charset-normalizer normalizer --version

Expected output:

    Charset-Normalizer 3.4.3 - Python 3.12.11 - Unicode 15.0.0 - SpeedUp ON

Actual output:

    realpath: --: No such file or directory
    Charset-Normalizer 3.4.3 - Python 3.12.11 - Unicode 15.0.0 - SpeedUp ON

<details>
<summary>Background to --</summary>

As background POSIX guidelines encourage utilities to accept `--`; this can be helpful to handle file name arguments that start with a dash. Python itself follows this guideline.

> Guideline 10: The first -- argument that is not an option-argument should be accepted as a delimiter indicating the end of options. Any following arguments should be treated as operands, even if they begin with the '-' character.

—<https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap12.html>

</details>

Alpine Linux utilities like `realpath` come from BusyBox not GNU Coreutils. The BusyBox `realpath` does not support `--`; and as a result outputs `realpath: --: No such file or directory`:

    ~ # touch /file
    ~ # realpath -- /file
    realpath: --: No such file or directory
    /file
    ~ # which realpath
    /usr/bin/realpath
    ~ # apk info --who-owns /usr/bin/realpath
    /usr/bin/realpath symlink target is owned by busybox-1.37.0-r19

Because, in certain conditions, uv adds a shebang line that includes `realpath --` the output from certain uv commands includes an extra error message.

A naive solution is a pull request removing `--` from shebang lines. This makes `uv` more portable to distributions like Alpine Linux that use BusyBox. A negative of that solution is for the majority of distributions, which use GNU coreutils, uv will perhaps not handle scripts starting with a dash, `-`, as effectively. I haven't quantified this negative.

<details>

<summary>Detailed reproduction steps</summary>

Commands to reproduce:

    incus launch images:alpine/3.22 c1
    incus exec c1 apk add curl python3
    incus exec c1 -- su -lc 'curl -LsSf https://astral.sh/uv/install.sh | sh'
    incus exec c1 -- su -lc \
        'uv tool run --from charset-normalizer normalizer --version'

Commands to view the shebang line:

    incus exec c1 -- sh -c \
        "find /root/.cache/uv -path '*/bin/normalizer' | xargs head -n5"

Output:

    #!/bin/sh
    '''exec' "$(dirname -- "$(realpath -- "$0")")"/'python' "$0" "$@"
    ' '''
    # -*- coding: utf-8 -*-
    import sys

</details>

_I edited the description to use a more popular package in the reproduction steps. [charset-normalizer](https://pypi.org/project/charset-normalizer/) appears around number 8 on <https://pypistats.org/top>. The original report used <https://github.com/Vimjas/vint> which is niche by comparison._



---

_Label `bug` added by @maxwell-k on 2025-10-09 15:31_

---

_Comment by @zanieb on 2025-10-09 17:34_

Thanks for the report.

cc @charliermarsh / @geofft 

---

_Label `compatibility` added by @zanieb on 2025-10-09 17:34_

---

_Label `great writeup` added by @zanieb on 2025-10-09 17:34_

---

_Comment by @charliermarsh on 2025-10-28 23:57_

@paveldikov may be interested too.

---

_Comment by @paveldikov on 2025-10-29 08:08_

Since this only seems to affect `busybox` (GNU and Mac are fine), I'd be happy with this being implemented as an edge case. You could have a logic branch that introspects the flavour of `/bin/sh`, and if it smells like `busybox`, post-process the POSIX-ly correct `--`s out for compatibility.

---

_Comment by @Lenormju on 2025-11-27 11:13_

I observed this with `uv 0.9.13` (released yesterday) in the Alpine image of uv.

To reproduce easily :
```sh
podman run --rm -it \
    --entrypoint /bin/sh \
    ghcr.io/astral-sh/uv@sha256:e7fb76eb20807c7f8d79bdb5bc9b863786556e2b62a1adb6947572a28424b204 \
    -c 'uv tool run --from charset-normalizer normalizer --version 2>&1 | grep realpath | wc -l'
```
gives `1`

---

_Comment by @charliermarsh on 2025-11-27 13:29_

That image is distroless, ie, it’s not intended to be used on its own like that. Please see https://docs.astral.sh/uv/guides/integration/docker/#getting-started.

---
