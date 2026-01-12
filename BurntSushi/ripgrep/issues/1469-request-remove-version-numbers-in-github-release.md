```yaml
number: 1469
title: "Request: Remove version numbers in github release artifact filenames"
type: issue
state: closed
author: bbenne10
labels:
  - wontfix
assignees: []
created_at: 2020-01-22T20:04:25Z
updated_at: 2020-01-22T22:34:52Z
url: https://github.com/BurntSushi/ripgrep/issues/1469
synced_at: 2026-01-12T16:13:23Z
```

# Request: Remove version numbers in github release artifact filenames

---

_@bbenne10_

#### What version of ripgrep are you using?
None

#### How did you install ripgrep?
I have not yet

#### What operating system are you using ripgrep on?
linux amd64

#### Describe your question, feature request, or bug.

Firstly, amazing project! Thanks for it!
Secondly, would it be too much to ask to remove the version strings from the release artifacts filenames? I am currently looking to incorporate automatic downloading and installing of some non-core utilities in my shell setup. For docker-compose, it looks a lot like this (in my ~/.bashrc at the moment):

which docker-compose 1>/dev/null || curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)"
 -o ~/.local/bin/docker-compose && chmod +x ~/.local/bin/docker-compose

This is possible because docker-compose doesn't have the release number as a substring of the file name, which makes it relatively reliable to guess where I'll be able to download it from (if used alongside the /latest/ release alias). This is SUPER convenient and REALLY nice! It allows me to grab up to date tooling without having to worry about installing it from an upstream. I use a somewhat out of date linux distro at work, which is unavoidable and unfortunate.

If you're unwilling to make this work, I 100% understand but would ask you to consider what value the version in the artifact's downloaded filename provides. Thanks again for the time and effort in this tool! I find it invaluable in my daily usage :)

---

_Comment by @BurntSushi on 2020-01-22 21:58_

I appreciate the convenience of a missing version number, but I'd rather not change the artifact naming convention without a better reason.

You can fairly easily come up with the latest version via GitHub's API:

```
$ version="$(curl -s "https://api.github.com/repos/BurntSushi/ripgrep/releases/latest" | jq -r .tag_name)"

$ echo $version
11.0.2

$ curl -LO "https://github.com/BurntSushi/ripgrep/releases/download/$version/ripgrep-$version-x86_64-unknown-linux-musl.tar.gz"
```

---

_Closed by @BurntSushi on 2020-01-22 21:58_

---

_Label `wontfix` added by @BurntSushi on 2020-01-22 21:59_

---

_Comment by @bbenne10 on 2020-01-22 22:24_

Seeing as how jq is non-standard, that solution is untenable from a fresh install perspective, which makes this a chicken/egg problem for me. 

It is, however, your prerogative. Thanks for consideration! 

EDIT: though i suppose that I am lucky: jq has release artifacts without a version number. I can grab it first and put it in `$PATH` and THEN grab other artifacts. Feels hacky, but it should work

---
