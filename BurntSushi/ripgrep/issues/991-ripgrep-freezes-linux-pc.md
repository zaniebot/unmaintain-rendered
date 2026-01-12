```yaml
number: 991
title: ripgrep freezes Linux PC
type: issue
state: closed
author: jovere
labels:
  - question
assignees: []
created_at: 2018-07-23T22:01:02Z
updated_at: 2018-07-24T13:31:02Z
url: https://github.com/BurntSushi/ripgrep/issues/991
synced_at: 2026-01-12T16:13:22Z
```

# ripgrep freezes Linux PC

---

_@jovere_

#### What version of ripgrep are you using?

ripgrep 0.8.1 (rev c8e9f25b85)
-SIMD -AVX

#### How did you install ripgrep?

Came as a module with VSCode (https://github.com/Microsoft/vscode)

#### What operating system are you using ripgrep on?

Linux x64 4.10.0-20-generic
Ubuntu 16.04 LTS

#### Describe your question, feature request, or bug.

When running a search on a directory of many many files using `rg`, the entire system, except for mouse movement freezes.  If I'm quick enough and attempt to kill `rg`, it results in a zombie process while all the child processes continue running forever.  I don't use `rg` under normal circumstances, but it is apparently bundled with vscode.  I'm doing a search through the Qt source code base, as stated in the VSCode issue below.

@roblourens suggested I post a bug report here in the following issue https://github.com/Microsoft/vscode/issues/54730.

Show the command you ran and the actual output. Include the `--debug` flag in
your invocation of ripgrep.

`/usr/share/code/resources/app/node_modules.asar.unpacked/vscode-ripgrep/bin/rg --no-ignore --debug "QT_QPA_EGLFS_INTEGRATION" Qt/5.9.6/Src`

I apologize for the horrible camera picture, but once it "hangs", I can't start any new processes.  If I try to do an `ls` in a separate console window, nothing happens.  I can't shut down except for hard resetting the machine.

If the output is large, put it in a gist:  https://gist.github.com/jovere/47fd22f6caad3c28a27915528e6a6c55
[
![screenshot](https://user-images.githubusercontent.com/10420943/43104576-89d90f00-8e97-11e8-9942-8adc51eeda5a.jpg)
](url)



---

_Comment by @BurntSushi on 2018-07-23 22:24_

Exciting! Can you tell me how exactly to get a checkout of the code you're searching? I mean, I realize it's Qt, but what are the exact commands I should be running? (There are many Qt repos in my Google search results.)

---

_Comment by @BurntSushi on 2018-07-23 22:26_

See also #975, which produces a similar symptom, but on macOS.

---

_Comment by @BurntSushi on 2018-07-23 22:29_

My guess is this is the repo: https://github.com/qt/qt5

I ran

```
$ git clone git://github.com/qt/qt5 --recursive
```

will that get me into the same state you're in?

Looking at `htop`, it is interesting that all of your cores appear to be pegged while executing kernel code (assuming the red means "sys" time). That suggests `strace` might show something interesting.

Another thing to try is to run ripgrep in single threaded mode, to see if that fairs any better. You can force that with `-j1`.

---

_Comment by @BurntSushi on 2018-07-23 22:40_

Sadly, nothing interesting happens when searching that repo on my system. It finishes in about `350ms` and uses a maximum heap size of ~100MB. Running single threaded finishes in about `2.3s` with a maximum heap size of ~24MB.

---

_Label `question` added by @BurntSushi on 2018-07-24 12:56_

---

_Comment by @jovere on 2018-07-24 13:09_

Thank you for noticing the CPU time being kernel time (would explain why the entire system is brought to its knees).  I updated to the latest kernel available for Ubuntu 16.04 (4.15.0-29-generic) and it finishes nearly instantaneously.  Hopefully this doesn't result in any other issues.
Thanks!

---

_Closed by @jovere on 2018-07-24 13:09_

---

_Comment by @BurntSushi on 2018-07-24 13:21_

Wow. Does this mean ripgrep tripped over a kernel bug? That's pretty cool. I'd love to know what it was.

---

_Comment by @jovere on 2018-07-24 13:31_

I don't think it's pretty cool... It's been a thorn in my side for the last few days.  I think I've gone through at least a dozen resets.

If you're masochistic, the version was 4.10.0-20.

---
