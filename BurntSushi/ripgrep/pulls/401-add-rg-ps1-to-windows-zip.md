```yaml
number: 401
title: Add _rg.ps1 to windows zip
type: pull_request
state: merged
author: dstcruz
labels: []
assignees: []
merged: true
base: master
head: patch-1
created_at: 2017-03-09T13:49:47Z
updated_at: 2017-03-14T01:54:08Z
url: https://github.com/BurntSushi/ripgrep/pull/401
synced_at: 2026-01-12T18:23:13Z
```

# Add _rg.ps1 to windows zip

---

_@dstcruz_

Tested with local cargo build paths.

---

_Comment by @dstcruz on 2017-03-09 13:51_

Apologies for the extra hassle.

---

_Comment by @BurntSushi on 2017-03-09 13:54_

No worries! This looks good. If something goes wrong at the next release I will deal with it. Thank you for doing the leg work on this. :-)

---

_Merged by @BurntSushi on 2017-03-09 14:45_

---

_Closed by @BurntSushi on 2017-03-09 14:45_

---

_Branch deleted on 2017-03-09 14:58_

---

_Comment by @BurntSushi on 2017-03-13 03:01_

Sadly, I had to revert this because it didn't actually work. I don't have time at the moment to figure out how to do globbing on Windows, but apparently the obvious thing doesn't work? See logs here: https://ci.appveyor.com/project/BurntSushi/ripgrep/build/1.0.341/job/7o1jqicmtwm7oa00

---

_Comment by @dstcruz on 2017-03-13 13:46_

Ugh. That glob worked on my Win7 machine using Powershell.  I'll take a look at the referenced URL to see if I can determine what went wrong.

---

_Comment by @BurntSushi on 2017-03-13 13:50_

@dstcruz Thanks. :-) I will also eventually look into it, but it's such a pain to debug and I was trying to hurry off to bed last night. :-)

---

_Comment by @leafgarland on 2017-03-14 01:45_

Appveyor by default runs cmd scripts and cmd copy will not do directory globbing.

Powershell copy will do directory globbing and you can ask appveyor to run a line as powershell by prepending `ps:` to the command line:

```yaml
before_deploy:
  # Generate artifacts for release
  # TODO(burntsushi): How can we enable SSSE3 on Windows?
  - cargo build --release
  - mkdir staging
  - copy target\release\rg.exe staging
  - ps: copy target\release\build\ripgrep-*\out\_rg.ps1 staging
  - cd staging
  - # release zipfile will look like 'rust-everywhere-v1.2.3-x86_64-pc-windows-msvc'
  - 7z a ../%PROJECT_NAME%-%APPVEYOR_REPO_TAG_NAME%-%TARGET%.zip *
  - appveyor PushArtifact ../%PROJECT_NAME%-%APPVEYOR_REPO_TAG_NAME%-%TARGET%.zip
```

I have tested the above on my own appveyor build and it works as expected.

If you wanted to run the whole script in powershell then you can use this syntax (note the environment variables have been changed to use powershell syntax):
```yaml
before_deploy:
  # Generate artifacts for release
  # TODO(burntsushi): How can we enable SSSE3 on Windows?
  - ps: |
    cargo build --release
    mkdir staging
    copy target\release\rg.exe staging
    copy target\release\build\ripgrep-*\out\_rg.ps1 staging
    cd staging
    # release zipfile will look like 'rust-everywhere-v1.2.3-x86_64-pc-windows-msvc'
    7z a "../$($env:PROJECT_NAME)-$($env:APPVEYOR_REPO_TAG_NAME)-$($env:TARGET).zip" *
    appveyor PushArtifact "../$($env:PROJECT_NAME)-$($env:APPVEYOR_REPO_TAG_NAME)-$($env:TARGET).zip"
```

---

_Comment by @leafgarland on 2017-03-14 01:54_

It was quicker to make the PR than edit the above post 😄 

See #408 

---
