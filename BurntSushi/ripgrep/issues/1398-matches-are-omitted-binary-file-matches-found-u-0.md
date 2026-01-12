```yaml
number: 1398
title: "Matches are omitted: Binary file matches (found \"\\u{0}\" byte around offset ...)"
type: issue
state: closed
author: jeschkies
labels:
  - invalid
assignees: []
created_at: 2019-10-02T11:25:28Z
updated_at: 2019-10-02T11:54:22Z
url: https://github.com/BurntSushi/ripgrep/issues/1398
synced_at: 2026-01-12T16:13:23Z
```

# Matches are omitted: Binary file matches (found "\u{0}" byte around offset ...)

---

_@jeschkies_

#### What version of ripgrep are you using?

```
❯ rg --version
ripgrep 11.0.2
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

```
brew install ripgrep
```

#### What operating system are you using ripgrep on?

macOS 10.14.6

#### Describe your question, feature request, or bug.

`rg` does not scan the complete log file.

#### If this is a bug, what are the steps to reproduce the behavior?

1. Extract [marathon-loop-master-43355.log.tar.gz](https://github.com/BurntSushi/ripgrep/files/3680823/marathon-loop-master-43355.log.tar.gz).
2. Run `rg "SharedMemoryIntegrationTest-MesosAgent" ~/Downloads/marathon-loop-master-43355.log`

#### If this is a bug, what is the actual behavior?

The last line of the output is

```
Binary file matches (found "\u{0}" byte around offset 2869884)
```

#### If this is a bug, what is the expected behavior?

The last line with `grep` is

```
WARN [06:48:21 SharedMemoryIntegrationTest-MesosAgent-32793] E1002 06:48:21.171119  7978 main.cpp:511] EXIT with status 1: Failed to create a containerizer: Could not create DockerContainerizer: Failed to create docker: Timed out getting docker version
```

It does not show up via `rg`.


---

_Renamed from "Matches are omitted." to "Matches are omitted: Binary file matches (found "\u{0}" byte around offset ...)" by @jeschkies on 2019-10-02 11:26_

---

_Comment by @BurntSushi on 2019-10-02 11:34_

Thanks for the issue and the easy reproduction. I really appreciate that.

However, this is very much working as intended. Have you read the guide yet? In particular, the [section on binary data](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#binary-data)?

I note that grep behaves exactly the same way:

```
$ grep "SharedMemoryIntegrationTest-MesosAgent" marathon-loop-master-43355.log
WARN [06:48:16 SharedMemoryIntegrationTest-MesosAgent-32793] I1002 06:48:16.166388  7978 logging.cpp:206] Logging to STDERR
WARN [06:48:16 SharedMemoryIntegrationTest-MesosAgent-32793] I1002 06:48:16.166604  7978 main.cpp:350] Build: 2019-09-05 03:25:52 by admin
WARN [06:48:16 SharedMemoryIntegrationTest-MesosAgent-32793] I1002 06:48:16.166620  7978 main.cpp:351] Version: 1.9.0
WARN [06:48:16 SharedMemoryIntegrationTest-MesosAgent-32793] I1002 06:48:16.166627  7978 main.cpp:354] Git tag: 1.9.0
WARN [06:48:16 SharedMemoryIntegrationTest-MesosAgent-32793] I1002 06:48:16.166632  7978 main.cpp:358] Git SHA: 5e79a584e6ec3e9e2f96e8bf418411df9dafac2e
WARN [06:48:16 SharedMemoryIntegrationTest-MesosAgent-32793] I1002 06:48:16.167569  7978 process.cpp:1247] libprocess is initialized on 172.16.10.150:32793 with 8 worker threads
WARN [06:48:16 SharedMemoryIntegrationTest-MesosAgent-32793] I1002 06:48:16.167773  7978 resolver.cpp:69] Creating default secret resolver
Binary file marathon-loop-master-43355.log matches
```

Just as with grep, pass the `-a` option to forcefully search all binary data (by always treating it as text):

```
$ rg "SharedMemoryIntegrationTest-MesosAgent" marathon-loop-master-43355.log -a
22133:WARN [06:48:16 SharedMemoryIntegrationTest-MesosAgent-32793] I1002 06:48:16.166388  7978 logging.cpp:206] Logging to STDERR
22134:WARN [06:48:16 SharedMemoryIntegrationTest-MesosAgent-32793] I1002 06:48:16.166604  7978 main.cpp:350] Build: 2019-09-05 03:25:52 by admin
22135:WARN [06:48:16 SharedMemoryIntegrationTest-MesosAgent-32793] I1002 06:48:16.166620  7978 main.cpp:351] Version: 1.9.0
22136:WARN [06:48:16 SharedMemoryIntegrationTest-MesosAgent-32793] I1002 06:48:16.166627  7978 main.cpp:354] Git tag: 1.9.0
22137:WARN [06:48:16 SharedMemoryIntegrationTest-MesosAgent-32793] I1002 06:48:16.166632  7978 main.cpp:358] Git SHA: 5e79a584e6ec3e9e2f96e8bf418411df9dafac2e
22138:WARN [06:48:16 SharedMemoryIntegrationTest-MesosAgent-32793] I1002 06:48:16.167569  7978 process.cpp:1247] libprocess is initialized on 172.16.10.150:32793 with 8 worker threads
22139:WARN [06:48:16 SharedMemoryIntegrationTest-MesosAgent-32793] I1002 06:48:16.167773  7978 resolver.cpp:69] Creating default secret resolver
24775:WARN [06:48:21 SharedMemoryIntegrationTest-MesosAgent-32793] E1002 06:48:21.171119  7978 main.cpp:511] EXIT with status 1: Failed to create a containerizer: Could not create DockerContainerizer: Failed to create docker: Timed out getting docker version
```

---

_Closed by @BurntSushi on 2019-10-02 11:34_

---

_Label `invalid` added by @BurntSushi on 2019-10-02 11:34_

---

_Comment by @BurntSushi on 2019-10-02 11:36_

Note that I used GNU grep in my above example. Since you're on macOS, you're probably using BSD grep, which might have different heuristics (if any) for detecting binary data.

---

_Comment by @jeschkies on 2019-10-02 11:54_

Thanks a ton. `rg -a` works like a charm. BSD grep behaves differently indeed.

---
