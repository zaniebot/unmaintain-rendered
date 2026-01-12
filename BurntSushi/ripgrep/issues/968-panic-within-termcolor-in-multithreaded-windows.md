```yaml
number: 968
title: panic within termcolor in multithreaded windows application
type: issue
state: closed
author: lazyuser
labels:
  - question
assignees: []
created_at: 2018-06-26T11:53:50Z
updated_at: 2018-07-22T13:11:24Z
url: https://github.com/BurntSushi/ripgrep/issues/968
synced_at: 2026-01-12T16:13:22Z
```

# panic within termcolor in multithreaded windows application

---

_@lazyuser_

#### What version of ripgrep are you using?

I do not use ripgrep directly. I use env_logger version 0.5.10 which depends on termcolor 0.3.6.

`rustc --version --verbose`:
rustc 1.26.2 (594fb253c 2018-06-01)
binary: rustc
commit-hash: 594fb253c2b02b320c728391a425d028e6dc7a09
commit-date: 2018-06-01
host: x86_64-pc-windows-msvc
release: 1.26.2
LLVM version: 6.0

Rust is installed via rustup. I build for **i686-pc-windows-msvc** target.

#### How did you install ripgrep?

N/A

#### What operating system are you using ripgrep on?

I build under Windows 10 v. 1709 and run under Windows 10  v. 1803.

Not necessarily relevant: my multithreaded code runs under msys bash with stdout and stderr redirected to `tee`. The application itself is not built against msys/mingw/etc though. 

#### Describe your question, feature request, or bug.

The system is under relatively high load: 50%-100% CPU load for each of four cores. The code receives network traffic in a loop and writes a log message for each packet using env_logger. Sometimes (non-deterministically) logging leads to the panic (see below). My uneducated guess from the backtrace is that some buffer access is not properly syncronized. Since termcolor [promises proper syncronization](https://docs.rs/termcolor/0.3.6/termcolor/struct.BufferWriter.html#method.print) I'm filling the bugreport against termcolor and not env_logger.

Sample code:

```rust
const HEADER_SIZE: usize = 10;

loop {
    let mut buf = [0; 1500];
    // self.socket is net::UdpSocket
    let (size, src_addr) = self.socket.recv_from(&mut buf)?;
    // sometimes the next statement causes panic
    info!(
        "recv from {} size={} header={}",
        src_addr,
        size,
        hex::encode(&buf[..std::cmp::min(size, HEADER_SIZE)])
    );
    // ... omitted ...
    // sometimes the next statement causes panic
    info!(
        "seq {} offset {} + {} = {}",
        sequence,
        offset,
        payload.len(),
        offset_next
    );
}
```

##### Backtrace:
```
thread '<unnamed>' panicked at 'slice index starts at 104 but ends at 29', libcore\slice\mod.rs:791:5
note: Some details are omitted, run with `RUST_BACKTRACE=full` for a verbose backtrace.
stack backtrace:
   0: std::sys_common::backtrace::_print
             at C:\projects\rust\src\libstd\sys_common\backtrace.rs:71
   1: std::sys_common::backtrace::print
             at C:\projects\rust\src\libstd\sys_common\backtrace.rs:59
   2: std::panicking::default_hook::{{closure}}
             at C:\projects\rust\src\libstd\panicking.rs:207
   3: std::panicking::default_hook
             at C:\projects\rust\src\libstd\panicking.rs:223
   4: std::panicking::rust_panic_with_hook
             at C:\projects\rust\src\libstd\panicking.rs:402
   5: std::panicking::begin_panic_fmt
             at C:\projects\rust\src\libstd\panicking.rs:349
   6: std::panicking::rust_begin_panic
             at C:\projects\rust\src\libstd\panicking.rs:325
   7: core::panicking::panic_fmt
             at C:\projects\rust\src\libcore\panicking.rs:72
   8: core::slice::slice_index_order_fail
             at C:\projects\rust\src\libcore\slice\mod.rs:791
   9: core::slice::{{impl}}::index<u8>
             at C:\projects\rust\src\libcore\slice\mod.rs:914
  10: core::slice::{{impl}}::index<u8>
             at C:\projects\rust\src\libcore\slice\mod.rs:997
  11: core::slice::{{impl}}::index<u8,core::ops::range::RangeFrom<usize>>
             at C:\projects\rust\src\libcore\slice\mod.rs:767
  12: std::io::Write::write_all<termcolor::LossyStandardStream<termcolor::IoStandardStreamLock>>
             at C:\projects\rust\src\libstd\io\mod.rs:1101
  13: termcolor::BufferWriter::print
             at C:\Users\user\.cargo\registry\src\github.com-1ecc6299db9ec823\termcolor-0.3.6\src\lib.rs:708
  14: env_logger::fmt::Formatter::print
             at C:\Users\user\.cargo\registry\src\github.com-1ecc6299db9ec823\env_logger-0.5.10\src\fmt.rs:472
  15: env_logger::{{impl}}::log::{{closure}}::{{closure}}
             at C:\Users\user\.cargo\registry\src\github.com-1ecc6299db9ec823\env_logger-0.5.10\src\lib.rs:823
  16: core::result::Result<(), std::io::error::Error>::and_then<(),std::io::error::Error,(),closure>
             at C:\projects\rust\src\libcore\result.rs:621
  17: env_logger::{{impl}}::log::{{closure}}
             at C:\Users\user\.cargo\registry\src\github.com-1ecc6299db9ec823\env_logger-0.5.10\src\lib.rs:823
  18: std::thread::local::LocalKey<core::cell::RefCell<core::option::Option<env_logger::fmt::Formatter>>>::try_with<core::cell::RefCell<core::option::Option<env_logger::fmt::Formatter>>,closure,()>
             at C:\projects\rust\src\libstd\thread\local.rs:290
  19: std::thread::local::LocalKey<core::cell::RefCell<core::option::Option<env_logger::fmt::Formatter>>>::with<core::cell::RefCell<core::option::Option<env_logger::fmt::Formatter>>,closure,()>
             at C:\projects\rust\src\libstd\thread\local.rs:244
  20: env_logger::{{impl}}::log
             at C:\Users\user\.cargo\registry\src\github.com-1ecc6299db9ec823\env_logger-0.5.10\src\lib.rs:794
  21: log::__private_api_log
             at C:\Users\user\.cargo\registry\src\github.com-1ecc6299db9ec823\log-0.4.2\src\lib.rs:1149
  22: [a reference to info!() statement from the code example above]
  [omitted]
```

##### Another Backtrace:
```
Athread '<unnamed>' panicked at 'slice index starts at 104 but ends at 75', libcore\slice\mod.rs:791:5
stack backtrace:
   0:  0x16f7627 - std::sys_common::backtrace::_print
                       at C:\projects\rust\src\libstd\sys_common\backtrace.rs:71
   1:  0x16f7627 - std::sys_common::backtrace::print
                       at C:\projects\rust\src\libstd\sys_common\backtrace.rs:59
   2:  0x16e7138 - std::panicking::default_hook::{{closure}}
                       at C:\projects\rust\src\libstd\panicking.rs:207
   3:  0x16e6deb - std::panicking::default_hook
                       at C:\projects\rust\src\libstd\panicking.rs:223
   4:  0x16e75e8 - std::panicking::rust_panic_with_hook
                       at C:\projects\rust\src\libstd\panicking.rs:402
   5:  0x16e73fa - std::panicking::begin_panic_fmt
                       at C:\projects\rust\src\libstd\panicking.rs:349
   6:  0x16e730b - std::panicking::rust_begin_panic
                       at C:\projects\rust\src\libstd\panicking.rs:325
   7:  0x1700813 - core::panicking::panic_fmt
                       at C:\projects\rust\src\libcore\panicking.rs:72
   8:  0x17025b6 - core::slice::slice_index_order_fail
                       at C:\projects\rust\src\libcore\slice\mod.rs:791
   9:  0x1500b18 - core::slice::{{impl}}::index<u8>
                       at C:\projects\rust\src\libcore\slice\mod.rs:914
  10:  0x1500951 - core::slice::{{impl}}::index<u8>
                       at C:\projects\rust\src\libcore\slice\mod.rs:997
  11:  0x14f88e1 - core::slice::{{impl}}::index<u8,core::ops::range::RangeFrom<usize>>
                       at C:\projects\rust\src\libcore\slice\mod.rs:767
  12:  0x14ed7f7 - std::io::Write::write_all<termcolor::LossyStandardStream<termcolor::IoStandardStreamLock>>
                       at C:\projects\rust\src\libstd\io\mod.rs:1101
  13:  0x14ee974 - termcolor::BufferWriter::print
                       at C:\Users\user\.cargo\registry\src\github.com-1ecc6299db9ec823\termcolor-0.3.6\src\lib.rs:708
  14:  0x135ea96 - env_logger::fmt::Formatter::print
                       at C:\Users\user\.cargo\registry\src\github.com-1ecc6299db9ec823\env_logger-0.5.10\src\fmt.rs:472
  15:  0x135538b - env_logger::{{impl}}::log::{{closure}}::{{closure}}
                       at C:\Users\user\.cargo\registry\src\github.com-1ecc6299db9ec823\env_logger-0.5.10\src\lib.rs:823
  16:  0x1362dfd - core::result::Result<(), std::io::error::Error>::and_then<(),std::io::error::Error,(),closure>
                       at C:\projects\rust\src\libcore\result.rs:621
  17:  0x135568d - env_logger::{{impl}}::log::{{closure}}
                       at C:\Users\user\.cargo\registry\src\github.com-1ecc6299db9ec823\env_logger-0.5.10\src\lib.rs:823
  18:  0x13691be - std::thread::local::LocalKey<core::cell::RefCell<core::option::Option<env_logger::fmt::Formatter>>>::try_with<core::cell::RefCell<core::option::Option<env_logger::fmt::Formatter>>,closure,()>
                       at C:\projects\rust\src\libstd\thread\local.rs:290
  19:  0x1369011 - std::thread::local::LocalKey<core::cell::RefCell<core::option::Option<env_logger::fmt::Formatter>>>::with<core::cell::RefCell<core::option::Option<env_logger::fmt::Formatter>>,closure,()>
                       at C:\projects\rust\src\libstd\thread\local.rs:244
  20:  0x1355338 - env_logger::{{impl}}::log
                       at C:\Users\user\.cargo\registry\src\github.com-1ecc6299db9ec823\env_logger-0.5.10\src\lib.rs:794
  21:  0x1515c32 - log::__private_api_log
                       at C:\Users\user\.cargo\registry\src\github.com-1ecc6299db9ec823\log-0.4.2\src\lib.rs:1149
  22: [a reference to info!() statement from the code example above]
  [omitted]
```


---

_Comment by @BurntSushi on 2018-06-26 12:08_

Here is the `termcolor` code:

```rust
    pub fn print(&self, buf: &Buffer) -> io::Result<()> {
        if buf.is_empty() {
            return Ok(());
        }
        let mut stream = self.stream.wrap(self.stream.get_ref().lock());
        if let Some(ref sep) = self.separator {
            if self.printed.load(Ordering::SeqCst) {
                stream.write_all(sep)?;
                stream.write_all(b"\n")?;
            }
        }
        match buf.0 {
            BufferInner::NoColor(ref b) => stream.write_all(&b.0)?,
            BufferInner::Ansi(ref b) => stream.write_all(&b.0)?,
            #[cfg(windows)]
            BufferInner::Windows(ref b) => {
                // We guarantee by construction that we have a console here.
                // Namely, a BufferWriter is the only way to produce a Buffer.
                let console_mutex = self.console.as_ref()
                    .expect("got Windows buffer but have no Console");
                let mut console = console_mutex.lock().unwrap();
                b.print(&mut *console, &mut stream)?;
            }
        }
        self.printed.store(true, Ordering::SeqCst);
        Ok(())
    }
```

From your traceback, line 708 is the last line executed, which is `BufferInner::NoColor(ref b) => stream.write_all(&b.0)?`. All that's doing is taking the bytes in `Buffer` and passing them through to `write_all`.

The standard library implementation of `write_all` is:

```rust
    #[stable(feature = "rust1", since = "1.0.0")]
    fn write_all(&mut self, mut buf: &[u8]) -> Result<()> {
        while !buf.is_empty() {
            match self.write(buf) {
                Ok(0) => return Err(Error::new(ErrorKind::WriteZero,
                                               "failed to write whole buffer")),
                Ok(n) => buf = &buf[n..],
                Err(ref e) if e.kind() == ErrorKind::Interrupted => {}
                Err(e) => return Err(e),
            }
        }
        Ok(())
    }
```

In this case, line 1101, `Ok(n) => buf = &buf[n..]`, is the last line executed before the panic machinery kicks in. The panic message itself, `slice index starts at 104 but ends at 75`, suggests that `buf.len() == 75` but that `n == 104`. This is a seemingly impossible result.

Your hunch about synchronization is interesting (although "buffer access" seems like a red herring), but I don't see any evidence to support it. Certainly, the `print` function, as shown above, is acquiring an exclusive lock to the underlying stream:

```rust
let mut stream = self.stream.wrap(self.stream.get_ref().lock());
```

I'm not seeing the root cause here. Are you? The fact that `write` is returning more bytes written than what is actually in the buffer is incredibly interesting, but I actually don't know the circumstances under which that kind of behavior is possible. Do you have something else writing to the same stream that isn't synchronized? (And even so, I don't understand why that would impact the number of bytes written returned by `write`.)

---

_Comment by @BurntSushi on 2018-06-26 12:10_

cc @retep998 Have you ever seen cases where a `write` call on Windows reports a number of bytes written that is greater than the number of bytes given to it to write?

---

_Comment by @BurntSushi on 2018-06-26 12:22_

`env_logger` definitely has some interesting use of `termcolor`, but I don't see any `unsafe` anywhere. `termcolor` isn't using any `unsafe` either. So in theory, mutation of the buffer being printed shouldn't happen. Moreover, `env_logger` is explicitly using TLS to manage its buffers, so I don't think it's even trying to use one `BufferWriter` across multiple threads at once.

So, I'm still not seeing the root cause. Unless someone else sees it, I'm not sure I'll be able to make forward progress on this without a real reproducible test case (which I understand may be difficult to provide).

---

_Comment by @retep998 on 2018-06-26 17:57_

@BurntSushi The only time I've ever seen funky shenanigans occur is when you use `WriteConsoleA`/`ReadConsoleA` or `WriteFile`/`ReadFile` to write to/read from a windows console that is set to a multibyte codepage such as UTF-8. If it's going through `WriteConsoleW` and `WriteConsoleA` to a console then it should be fine, similarly `WriteFile`/`ReadFile` to a file or pipe should be fine.

Is it possible that `NoColor` is trying to write to a windows console as if it was a file or pipe?

---

_Label `question` added by @BurntSushi on 2018-07-06 14:59_

---

_Comment by @BurntSushi on 2018-07-06 15:07_

> Is it possible that NoColor is trying to write to a windows console as if it was a file or pipe?

@retep998 As I understand it, the write that is happening here is just going through `io::Stdout` (or `io::Stderr`).

---

_Comment by @BurntSushi on 2018-07-22 13:11_

I don't think there's anything actionable to do on my end here. I've never been able to see this error myself, and I've been unable to form a hypothesis for why it is occurring from reading the code.

I'm happy to see this re-opened if more information comes to light or if there are better reproduction steps.

---

_Closed by @BurntSushi on 2018-07-22 13:11_

---
