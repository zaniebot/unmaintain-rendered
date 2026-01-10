---
number: 6304
title: Minor Points/Suggestions on Install Script
type: issue
state: closed
author: stevenwalton
labels:
  - external
assignees: []
created_at: 2024-08-21T07:27:42Z
updated_at: 2024-10-27T22:36:01Z
url: https://github.com/astral-sh/uv/issues/6304
synced_at: 2026-01-10T01:23:58Z
---

# Minor Points/Suggestions on Install Script

---

_Issue opened by @stevenwalton on 2024-08-21 07:27_

I've noticed a few little things in the install script that might be worth editing and could make it easier to maintain.

First off, thank you for wrapping the script in main! While I still think piping into `sh` is bad, it could be worse. If you don't want to pipe into bash, this is a better alternative. Longer, but we copy paste anyways, right?
```bash
 curl -SsLf  https://astral.sh/uv/install.sh -o /tmp/uv_install.sh && sh /tmp/uv_install.sh 
```

First off, there's `#!/bin/sh`. A better alternative would be [`#!/usr/bin/env bash`](https://www.baeldung.com/linux/bash-shebang-lines) as it'll provide some better flexibility and respect the user's `$PATH`

In the globals we have
```bash
APP_VERSION="0.3.0"
ARTIFACT_DOWNLOAD_URL="${INSATLLER_DOWNLOAD_URL:-https://github.com/astral-sh/uv/releases/download/0.3.0}"
```
This is fine but will not be flexible and needs to be modified each time a version is bumped. Instead, you can do the following
```bash
GIT_URL="${GIT_URL:-https://github.com/astral-sh/uv/}"
APP_VERSION="${APP_VERSION:-$(curl -Ls -o /dev/null -w %{url_effective} "${GIT_URL%/}/releases/latest/" | rev | cut -d "/" -f1 | rev)}"
ARTIFACT_DOWNLOAD_URL="${INSTALLER_DOWNLOAD_URL:-${GIT_URL%/}/releases/download/${APP_VERSION}}
```

- The first line allows flexibility if the Git URL changes or if someone downloads a fork they can easily use the script for theirs without needing to edit the version variable (though I think this should exist somewhere).
- The second line defaults the version to the latest release, but also allows an override in case someone doesn't want to.
- Third line now points to whatever version the user requested (or a complete alternative).

This should then allow you to clean up `json_binary_aliases` and `aliases_for_binary` since you can do
```bash
# Works on linux and OSX
regex="(.*${ARTIFACT_DOWNLOAD_URL}.*>)(.*(tar.gz|zip))(</a>.*$)"
# Probably better to add
# IFS=$'\n'
declare -a packages=( $(curl -s "${ARTIFACT_DOWNLOAD_URL//download\//}" | grep -E "${regex}" | sed -E "s@${regex}@\2@p" | uniq) )
# then `unset IFS`
```
to get all the packages and then of course you can now just grep the array and get the version you want. Or since you do `get_architecture` first, you can modify the regex to include the appropriate file and probably just avoid the array in general.

I believe this should allow users to then have full control over versioning from the install script (might as well add a version flag while you're at it). 

And `usage` can be fixed too.

I hope this can save you time when releasing new versions and simplifying the install script. Feel free to just close this issue if these are things you don't want to modify. There's some more things like this littered through the script that could make it more efficient but this should help clear a few hundred lines for you.

Edit: mistakenly wrote `/usr/bin/sh` instead of `/usr/bin/env`. Thanks @severen 

---

_Comment by @severen on 2024-08-21 07:36_

> First off, there's #!/bin/sh. A better alternative would be [#!/usr/bin/sh bash](https://www.baeldung.com/linux/bash-shebang-lines) as it'll provide some better flexibility and respect the user's $PATH

Did you mean `#!/usr/bin/env bash`? That's the usual idiom.

---

_Label `upstream` added by @charliermarsh on 2024-08-21 13:19_

---

_Label `installer` added by @charliermarsh on 2024-08-21 13:19_

---

_Label `installer` removed by @charliermarsh on 2024-08-21 13:19_

---

_Comment by @charliermarsh on 2024-08-21 15:40_

Thank you! I passed these along to the Axo team -- they maintain [cargo-dist](https://github.com/axodotdev/cargo-dist), which we use to generate the installers.

---

_Closed by @charliermarsh on 2024-08-21 15:40_

---

_Comment by @stevenwalton on 2024-08-21 21:09_

@severen haha yes I did. Thanks for the catch. I'll make a quick edit. 

@charliermarsh awesome! Lots of lost bash lore these days. I wasn't aware of that project and I'll have to check it out. And thanks for the tweets.

---

_Comment by @axel-kah on 2024-10-27 21:59_

FYI: #8069 will enable redefining the github base url used to fetch github release artifacts by the script. Seems like that's part of what you requested.

EDIT: not quite ... #8069 is really only about the self-update feature of the distributed apps and not about the installer scripts.

---
