```yaml
number: 950
title: "\"Cannot find path\" on Windows"
type: issue
state: closed
author: akavel
labels:
  - duplicate
assignees: []
created_at: 2018-06-15T22:06:24Z
updated_at: 2018-06-15T22:35:42Z
url: https://github.com/BurntSushi/ripgrep/issues/950
synced_at: 2026-01-12T16:13:22Z
```

# "Cannot find path" on Windows

---

_@akavel_

#### What version of ripgrep are you using?

ripgrep 0.8.1 (rev c8e9f25b85)
-SIMD -AVX

#### How did you install ripgrep?

https://github.com/BurntSushi/ripgrep/releases/download/0.8.1/ripgrep-0.8.1-x86_64-pc-windows-msvc.zip

#### What operating system are you using ripgrep on?

Windows 10

#### Describe your question, feature request, or bug.



#### If this is a bug, what are the steps to reproduce the behavior?



#### If this is a bug, what is the actual behavior?

Show the command you ran and the actual output. Include the `--debug` flag in
your invocation of ripgrep.

```
[...some successful matches...]
node_modules\lottie-react-native\src\android\build\tmp\expandedArchives\classes.jar_9x65my2kdi5p9awbep9cbvzwk\android\support\v4\media\session\MediaControllerCompat$MediaControllerImplApi21$ExtraBinderRequestResultReceiver.class: System nie może odnaleźć określonej ścieżki. (os error 3)
```

(the message "System nie może odnaleźć [...]" corresponds to English "The system cannot find the path specified.")

#### If this is a bug, what is the expected behavior?

Searched the file successfully; or at least continued searching subsequent files after printing the error, instead of stopping.


---

_Comment by @okdana on 2018-06-15 22:23_

I think this is related to #364, there are some work-arounds mentioned there

---

_Comment by @BurntSushi on 2018-06-15 22:35_

This is most definitely the long file name bug.

---

_Closed by @BurntSushi on 2018-06-15 22:35_

---

_Label `duplicate` added by @BurntSushi on 2018-06-15 22:35_

---
