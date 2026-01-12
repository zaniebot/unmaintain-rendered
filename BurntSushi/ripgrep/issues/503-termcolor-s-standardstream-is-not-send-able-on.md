```yaml
number: 503
title: "termcolor's StandardStream is not Send-able on Windows platforms"
type: issue
state: closed
author: Limeth
labels:
  - bug
assignees: []
created_at: 2017-06-04T19:22:57Z
updated_at: 2017-08-27T15:06:21Z
url: https://github.com/BurntSushi/ripgrep/issues/503
synced_at: 2026-01-12T16:13:22Z
```

# termcolor's StandardStream is not Send-able on Windows platforms

---

_@Limeth_

Successful build on non-windows: https://travis-ci.org/Limeth/ethaddrgen/jobs/239361468
Unsuccessful build on windows: https://ci.appveyor.com/project/Limeth/ethaddrgen/build/1.0.11

As far as I can tell, it is impossible to use termcolor to output to stdout from separate threads.
I ran into issues trying to create multiple `StandardStream::stdout`s, only the first instance actually output into the terminal (both `cmd` and `powershell`), although outputting into a file worked fine.
I thought I could fix this issue by having a single instance of Arc<Mutex<StandardStream>>, but that type does not compile for Windows.

---

_Comment by @BurntSushi on 2017-06-04 19:56_

This does look like a bug. The `termcolor` implementation stuffs a mutex guard into an internal enum (I believe) only for the purpose of code reuse, but it ends up infecting the `StandardStream` type when it should only impact the `StandardStreamLock` type (which I believe should not be `Send`).

> it is impossible to use termcolor to output to stdout from separate threads

You need to be careful. `termcolor` could use with more docs, but the [`Buffer` and `BufferWriter`](https://docs.rs/termcolor/0.3.2/termcolor/#example-using-bufferwriter) types are provided specifically for the purpose of efficiently writing to stdout with colors in a platform independent way. Namely, if you're writing to stdout on Windows from multiple threads and you don't use the buffer types, then you will need to be careful to lock stdout and apply color settings and reset them before releasing the lock, otherwise you'll end up with color settings from multiple threads influencing each other.

---

_Label `bug` added by @BurntSushi on 2017-06-04 19:56_

---

_Comment by @remram44 on 2017-08-27 04:07_

I ran into this, was very surprised to see my code didn't compile on Windows although I made sure to use multiplatform dependencies. I'm using a `StandardStream` in a [`log::Log`](https://docs.rs/log/0.3.8/log/trait.Log.html) implementation.

---

_Closed by @BurntSushi on 2017-08-27 15:05_

---

_Comment by @BurntSushi on 2017-08-27 15:06_

`termcolor 0.3.3` is on crates.io with a fix for this.

---
