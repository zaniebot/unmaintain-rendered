```yaml
number: 1817
title: "stdin not detected by ripgrep when it's spawned by luv (libuv)"
type: issue
state: closed
author: mnowotnik
labels:
  - question
assignees: []
created_at: 2021-03-10T12:48:43Z
updated_at: 2021-03-10T14:34:37Z
url: https://github.com/BurntSushi/ripgrep/issues/1817
synced_at: 2026-01-12T16:13:24Z
```

# stdin not detected by ripgrep when it's spawned by luv (libuv)

---

_@mnowotnik_

#### What version of ripgrep are you using?

ripgrep 11.0.2
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

ubuntu apt repo

#### What operating system are you using ripgrep on?

Linux mint 20

#### Describe your bug.

It seems that under some circumstances rg doesn't detect attached stdin.
I can only replicate it using libuv library to spawn rg with redirected fds.

#### What are the steps to reproduce the behavior?

It's pretty tricky to replicate. The easiest way is to install either neovim 0.5.0
and using [plenary](https://github.com/nvim-lua/plenary.nvim) library create a job that
writes to rg process.
Example:

```lua
local Job = require('plenary.job')
local job = Job:new {
      command = 'rg',
      args = {'foo'},
      writer = Job:new {
        command = 'echo',
        args = {'foobar' },
      }
}
local result = job:sync()
for idx,val in ipairs(result) do
  print(val)
end
```

#### What is the actual behavior?

ripgrep doesn't detects stdin and searches local directory for files with `foo` string.

#### What is the expected behavior?

ripgrep should print "foobar".
If I switch 'rg' to 'grep', it works without a problem.
If I switch 'rg' to 'cat', it returns stdin correctly.

#### What do you think ripgrep should have done?

Frankly, no idea. Ripgrep works just fine in shell.


---

_Comment by @BurntSushi on 2021-03-10 13:13_

Please try it with the latest release of ripgrep.

---

_Comment by @BurntSushi on 2021-03-10 13:15_

Also, I don't know how to execute your reproduction. Is it possible to provide more detailed steps? Or find another reproduction?

As a debugging step, could you try passing `-` to ripgrep and see if that causes it to search stdin?

---

_Label `question` added by @BurntSushi on 2021-03-10 13:15_

---

_Comment by @mnowotnik on 2021-03-10 14:31_

I confirm that the latest release of ripgrep doesn't have this issue. And also argument `-` can be used as a workaround. Thanks for quick reply!

---

_Closed by @mnowotnik on 2021-03-10 14:34_

---
