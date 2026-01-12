```yaml
number: 300
title: Symantec Endpoint Protection complains about rg.exe under Windows
type: issue
state: closed
author: mathiasdahl
labels: []
assignees: []
created_at: 2017-01-03T17:20:58Z
updated_at: 2017-01-03T22:59:25Z
url: https://github.com/BurntSushi/ripgrep/issues/300
synced_at: 2026-01-12T16:13:21Z
```

# Symantec Endpoint Protection complains about rg.exe under Windows

---

_@mathiasdahl_

Hi,

I just downloaded the latest binaries of ripgrep for Windows (the msvc version) and unpacked it. A minute later Symantec Endpoint Protection complains about it being infected by "SONAR.Heur.RGC!g201".

http://securityresponse.symantec.com/security_response/writeup.jsp?docid=2016-061612-2956-99

Should I be afraid?

Thanks!

/Mathias


---

_Comment by @BurntSushi on 2017-01-03 17:26_

I don't know. I'm not a Windows user, so I I don't understand the error message you've reported or how such an error is reported. Every piece of code that goes into producing the binary is open source. My guess is that it's a false positive. 

cc @retep998

---

_Comment by @retep998 on 2017-01-03 18:14_

Notice the `Heur` part of the signature. Basically it has a heuristic that searches for certain code behavior and flags binaries as being infected as a result. If you're worried you can try building ripgrep from source and seeing whether the same AV complaint occurs. If it does, that AV simply has a false positive that needs to be reported to the AV. There is nothing @BurntSushi or I can do about false positives unfortunately.

---

_Closed by @BurntSushi on 2017-01-03 18:32_

---

_Comment by @BurntSushi on 2017-01-03 18:32_

Thanks for chiming in @retep998! :-)

---

_Comment by @mathiasdahl on 2017-01-03 22:59_

Thanks for your comments/answers!

---
