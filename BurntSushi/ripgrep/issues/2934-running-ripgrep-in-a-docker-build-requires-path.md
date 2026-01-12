```yaml
number: 2934
title: "Running ripgrep in a Docker build requires [PATH] to be specified"
type: issue
state: closed
author: dvdksn
labels:
  - wontfix
assignees: []
created_at: 2024-11-15T16:03:56Z
updated_at: 2025-07-11T16:45:01Z
url: https://github.com/BurntSushi/ripgrep/issues/2934
synced_at: 2026-01-12T16:13:25Z
```

# Running ripgrep in a Docker build requires [PATH] to be specified

---

_@dvdksn_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

Tested with:
ripgrep 13.0.0 (Debian)
ripgrep 14.1.0 (Alpine Linux)

### How did you install ripgrep?

`apk add ripgrep`
`apt install ripgrep`

### What operating system are you using ripgrep on?

Debian 12, Alpine Linux 3.20.3 (both in Docker)

### Describe your bug.

`rg` runs fine without a path argument if you do it interactively. But for some reason, when I try to run `rg` in a Docker build, it would always error out unless I specify the positional argument for `[PATH]`.

The following interactive use case works fine:

1. Start an interactive `alpine` container (with workdir `/etc` in this case)

```console
$ docker run -it -w /etc alpine
```

2. Install ripgrep in the container:

```console
/etc # apk add ripgrep
fetch https://dl-cdn.alpinelinux.org/alpine/v3.20/main/aarch64/APKINDEX.tar.gz
fetch https://dl-cdn.alpinelinux.org/alpine/v3.20/community/aarch64/APKINDEX.tar.gz
(1/3) Installing libgcc (13.2.1_git20240309-r0)
(2/3) Installing pcre2 (10.43-r0)
(3/3) Installing ripgrep (14.1.0-r0)
Executing busybox-1.36.1-r29.trigger
OK: 13 MiB in 17 packages
```

3. Execute:

```console
/etc # rg alpinelinux.org
motd
5:See <https://wiki.alpinelinux.org/>.

os-release
5:HOME_URL="https://alpinelinux.org/"
6:BUG_REPORT_URL="https://gitlab.alpinelinux.org/alpine/aports/-/issues"

modprobe.d/blacklist.conf
83:# https://gitlab.alpinelinux.org/alpine/aports/-/issues/12999

secfixes.d/alpine
1:https://secdb.alpinelinux.org/v3.20/main.json
2:https://secdb.alpinelinux.org/v3.20/community.json

apk/repositories
1:https://dl-cdn.alpinelinux.org/alpine/v3.20/main
2:https://dl-cdn.alpinelinux.org/alpine/v3.20/community
```

### What are the steps to reproduce the behavior?

Run the same steps as above but via a Dockerfile, you'll get an error.

```console
$ docker build -<<EOF
FROM alpine
WORKDIR /etc
RUN apk add ripgrep
RUN rg alpinelinux.org
EOF
```

```text
[+] Building 0.2s (8/8) FINISHED                                                            docker:desktop-linux
 => [internal] connecting to local controller                                                               0.0s
 => [internal] load build definition from Dockerfile                                                        0.0s
 => => transferring dockerfile: 105B                                                                        0.0s
 => [internal] load metadata for docker.io/library/alpine:latest                                            0.0s
 => [internal] load .dockerignore                                                                           0.0s
 => => transferring context: 2B                                                                             0.0s
 => [1/4] FROM docker.io/library/alpine:latest@sha256:1e42bbe2508154c9126d48c2b8a75420c3544343bf86fd041fb7  0.0s
 => => resolve docker.io/library/alpine:latest@sha256:1e42bbe2508154c9126d48c2b8a75420c3544343bf86fd041fb7  0.0s
 => CACHED [2/4] WORKDIR /etc                                                                               0.0s
 => CACHED [3/4] RUN apk add ripgrep                                                                        0.0s
 => ERROR [4/4] RUN rg alpinelinux.org                                                                      0.1s
------
 > [4/4] RUN rg alpinelinux.org:
------
Dockerfile:4
--------------------
   2 |     WORKDIR /etc
   3 |     RUN apk add ripgrep
   4 | >>> RUN rg alpinelinux.org
   5 |     
--------------------
ERROR: process "/bin/sh -c rg alpinelinux.org" did not complete successfully: exit code: 1
```

### What is the actual behavior?

```console
$ docker build --no-cache --progress=plain -<<EOF
FROM alpine
WORKDIR /etc
RUN apk add ripgrep
RUN rg --debug alpinelinux.org
EOF
```

```text
#0 building with "desktop-linux" instance using docker driver

#1 [internal] connecting to local controller
#1 DONE 0.0s

#2 [internal] load build definition from Dockerfile
#2 transferring dockerfile: 113B done
#2 DONE 0.0s

#3 [internal] load metadata for docker.io/library/alpine:latest
#3 DONE 0.0s

#4 [internal] load .dockerignore
#4 transferring context: 2B done
#4 DONE 0.0s

#5 [1/4] FROM docker.io/library/alpine:latest@sha256:1e42bbe2508154c9126d48c2b8a75420c3544343bf86fd041fb7527e017a4b4a
#5 resolve docker.io/library/alpine:latest@sha256:1e42bbe2508154c9126d48c2b8a75420c3544343bf86fd041fb7527e017a4b4a done
#5 DONE 0.0s

#6 [2/4] WORKDIR /etc
#6 CACHED

#7 [3/4] RUN apk add ripgrep
#7 0.076 fetch https://dl-cdn.alpinelinux.org/alpine/v3.20/main/aarch64/APKINDEX.tar.gz
#7 0.196 fetch https://dl-cdn.alpinelinux.org/alpine/v3.20/community/aarch64/APKINDEX.tar.gz
#7 0.396 (1/3) Installing libgcc (13.2.1_git20240309-r0)
#7 0.407 (2/3) Installing pcre2 (10.43-r0)
#7 0.502 (3/3) Installing ripgrep (14.1.0-r0)
#7 0.543 Executing busybox-1.36.1-r29.trigger
#7 0.547 OK: 13 MiB in 17 packages
#7 DONE 0.6s

#8 [4/4] RUN rg --debug alpinelinux.org
#8 0.099 rg: DEBUG|rg::flags::parse|crates/core/flags/parse.rs:97: no extra arguments found from configuration file
#8 0.099 rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1099: using heuristics to determine whether to read from stdin or search ./ (is_readable_stdin=true, stdin_consumed=false, mode=Search(Standard))
#8 0.099 rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1112: heuristic chose to search stdin
#8 0.099 rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1260: found hostname for hyperlink configuration: buildkitsandbox
#8 0.099 rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1270: hyperlink format: ""
#8 0.099 rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:174: using 1 thread(s)
#8 0.100 rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
#8 ERROR: process "/bin/sh -c rg --debug alpinelinux.org" did not complete successfully: exit code: 1
------
 > [4/4] RUN rg --debug alpinelinux.org:
0.099 rg: DEBUG|rg::flags::parse|crates/core/flags/parse.rs:97: no extra arguments found from configuration file
0.099 rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1099: using heuristics to determine whether to read from stdin or search ./ (is_readable_stdin=true, stdin_consumed=false, mode=Search(Standard))
0.099 rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1112: heuristic chose to search stdin
0.099 rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1260: found hostname for hyperlink configuration: buildkitsandbox
0.099 rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1270: hyperlink format: ""
0.099 rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:174: using 1 thread(s)
0.100 rg: DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
------
Dockerfile:4
--------------------
   2 |     WORKDIR /etc
   3 |     RUN apk add ripgrep
   4 | >>> RUN rg --debug alpinelinux.org
   5 |     
--------------------
ERROR: process "/bin/sh -c rg --debug alpinelinux.org" did not complete successfully: exit code: 1
```

### What is the expected behavior?

I didn't expect the `[PATH]` argument to be necessary if I want to query the CWD.

Running the same `docker build` but with a path argument (`.`) results in no error.

```console
$ docker build --no-cache -<<EOF
FROM alpine
WORKDIR /etc
RUN apk add ripgrep
RUN rg alpinelinux.org .
EOF
[+] Building 1.5s (9/9) FINISHED                                                            docker:desktop-linux
 => [internal] connecting to local controller                                                               0.0s
 => [internal] load build definition from Dockerfile                                                        0.0s
 => => transferring dockerfile: 107B                                                                        0.0s
 => [internal] load metadata for docker.io/library/alpine:latest                                            0.0s
 => [internal] load .dockerignore                                                                           0.0s
 => => transferring context: 2B                                                                             0.0s
 => [1/4] FROM docker.io/library/alpine:latest@sha256:1e42bbe2508154c9126d48c2b8a75420c3544343bf86fd041fb7  0.0s
 => => resolve docker.io/library/alpine:latest@sha256:1e42bbe2508154c9126d48c2b8a75420c3544343bf86fd041fb7  0.0s
 => CACHED [2/4] WORKDIR /etc                                                                               0.0s
 => [3/4] RUN apk add ripgrep                                                                               0.5s
 => [4/4] RUN rg alpinelinux.org .                                                                          0.1s
 => exporting to image                                                                                      0.2s
 => => exporting layers                                                                                     0.2s 
 => => exporting manifest sha256:7d99046d509ea55010ab077d45972f0091082ab94301ed0ab203cc33bd503d4b           0.0s 
 => => exporting config sha256:d902690c8d0622c14a76bb0dda908579850b504d9fa3281e89deaf5c4e3250f1             0.0s 
 => => exporting attestation manifest sha256:25a938de608a651b20953ac654bad26bac4feaaa515d3a98d6d520d1d4911  0.0s
 => => exporting manifest list sha256:290ad615981b729f42de884bac44ca06ff52c08c45cd7059bd5ccb3d141c8817      0.0s
 => => naming to moby-dangling@sha256:290ad615981b729f42de884bac44ca06ff52c08c45cd7059bd5ccb3d141c8817      0.0s
 => => unpacking to moby-dangling@sha256:290ad615981b729f42de884bac44ca06ff52c08c45cd7059bd5ccb3d141c8817   0.0s
```

---

_Comment by @BurntSushi on 2024-11-15 16:09_

It's likely because whatever is responsible for running ripgrep is advertising a readable stdin. You should be able to clearly see this given the `--debug` messages you're getting:

```
#8 0.099 rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1099: using heuristics to determine whether to read from stdin or search ./ (is_readable_stdin=true, stdin_consumed=false, mode=Search(Standard))
#8 0.099 rg: DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1112: heuristic chose to search stdin
```

And also from the man page:

```
       ripgrep  will  automatically  detect  if stdin exists and search
       stdin for a regex pattern, e.g. ls | rg foo.  In  some  environ‐
       ments,  stdin may exist when it shouldn't. To turn off stdin de‐
       tection, one can explicitly specify  the  directory  to  search,
       e.g. rg foo ./.
```

---

_Closed by @BurntSushi on 2024-11-15 16:09_

---

_Label `wontfix` added by @BurntSushi on 2024-11-15 16:09_

---

_Comment by @alexyao2015 on 2025-07-11 16:45_

I recently ran into this same issue and debugged it for quite sometime. It seems like this would be worth noting in the readme.

---
