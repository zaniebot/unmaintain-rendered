```yaml
number: 1139
title: file is unexpectedly being ignored
type: issue
state: closed
author: directorscut82
labels:
  - question
assignees: []
created_at: 2018-12-13T10:22:08Z
updated_at: 2018-12-13T13:05:20Z
url: https://github.com/BurntSushi/ripgrep/issues/1139
synced_at: 2026-01-12T16:13:23Z
```

# file is unexpectedly being ignored

---

_@directorscut82_

#### What version of ripgrep are you using?

ripgrep 0.10.0 (rev 8a7db1a918)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

$ curl -LO https://github.com/BurntSushi/ripgrep/releases/download/0.10.0/ripgrep_0.10.0_amd64.deb
$ sudo dpkg -i ripgrep_0.10.0_amd64.deb

#### What operating system are you using ripgrep on?

Ubuntu 16.04.5 LTS

#### Describe your question, feature request, or bug.

I am trying to replicate a simple grep search. I am not very familiar with all the command line
options so it is probably an incorrect use case:

```
grep -inr 'Delay(25'
saorview_app/build/board/src.ali/DVBCore/midware/stb/src/stbpes.c:184:            //STB_OSTaskDelay(25);
saorview_app/build/board/src.ali/DVBCore/midware/stb/src/stbpes.c:190:         //STB_OSTaskDelay(25);

rg -iFnp 'Delay(25'

rg -iFnp -j1 --no-mmap 'Delay(25'

cd saorview_app/build/board/src.ali/DVBCore/
rg -iFnp 'Delay(25'
midware/stb/src/stbpes.c
184:         //STB_OSTaskDelay(25);
190:         //STB_OSTaskDelay(25);
```

Is there a particular reason rg ignores the file 'stbpes.c' unless i change directory closer to the file?
(i tried max depth and also debug flag which just shows all the compressed files it ignored while searching)



---

_Comment by @directorscut82 on 2018-12-13 10:26_

I also checked the file in case something weird is going on with the permissions (even chmod 777 it just to be sure)

```
ls -lah saorview_app/build/board/src.ali/DVBCore/midware/stb/src/stbpes.c
-rwxrwxrwx 1   

file saorview_app/build/board/src.ali/DVBCore/midware/stb/src/stbpes.c
saorview_app/build/board/src.ali/DVBCore/midware/stb/src/stbpes.c: C source, UTF-8 Unicode text
```

---

_Comment by @BurntSushi on 2018-12-13 11:57_

> Is there a particular reason rg ignores the file 'stbpes.c' unless i change directory closer to the file?

I can't answer this question without more information or a reproducible test case. I believe the issue template requested the debug output. Could you please include it?

Please also consider reading the guide section on automatic filtering. 

---

_Comment by @directorscut82 on 2018-12-13 12:12_

sorry, i forgot my pastebin:)

https://pastebin.com/x7DzR0wx

---

_Comment by @BurntSushi on 2018-12-13 12:41_

@directorscut82 Did you read the guide's section on automatic filtering? Have you tried disabling filtering? If so, did that find the file?

Your debug output suggests you have a rule to specifically ignore the `src.ali` directory:

```
DEBUG|ignore::walk|ignore/src/walk.rs:1611: ignoring ./src.ali: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/home/ekt/Desktop/PVR_SAORVIEW/saorview_app/.gitignore"), original: "src.ali", actual: "**/src.ali", is_whitelist: false, is_only_dir: false })))
```

---

_Comment by @directorscut82 on 2018-12-13 12:58_

Thank you very much for your help.

I had tried with --hidden which i thought it was same with filtering out the restrictions from gitignore. So, it was indeed an incorrect use case:)

---

_Closed by @BurntSushi on 2018-12-13 13:04_

---

_Label `question` added by @BurntSushi on 2018-12-13 13:05_

---

_Renamed from "incorrect use case?" to "file is unexpectedly being ignored" by @BurntSushi on 2018-12-13 13:05_

---
