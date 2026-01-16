```yaml
number: 3009
title: "ignore: parallel walker occasionally hangs if visitor panics"
type: issue
state: open
author: cosmicexplorer
labels:
  - bug
assignees: []
created_at: 2025-03-06T00:53:58Z
updated_at: 2026-01-16T06:15:34Z
url: https://github.com/BurntSushi/ripgrep/issues/3009
synced_at: 2026-01-16T06:54:36Z
```

# ignore: parallel walker occasionally hangs if visitor panics

---

_@cosmicexplorer_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

master (de4baa10024f2cb62d438596274b9b710e01c59b)

### How did you install ripgrep?

I am able to reproduce this on `master`.

### What operating system are you using ripgrep on?

Arch Linux (64-bit, x86):

```shell
> uname -a
Linux terrestrial-gamma-ray-flash 6.13.5-zen1-1-zen #1 ZEN SMP PREEMPT_DYNAMIC Thu, 27 Feb 2025 18:09:27 +0000 x86_64 GNU/Linux
```

### Describe your bug.

`WalkParallel::visit()` will nondeterministically hang if the `ParallelVisitor::visit()` implementation panics. This also occurs when providing a closure to `WalkParallel::run()`.

### What are the steps to reproduce the behavior?

I am working on a minimal fix to this right now (see https://github.com/BurntSushi/ripgrep/compare/master...cosmicexplorer:ripgrep:parallel-panic-quit?expand=1), but I think it will require a change to the parallelization methods we use from crossbeam, so I wanted to create this issue beforehand. I am currently able to reproduce this with the following:

```diff
diff --git a/crates/ignore/src/walk.rs b/crates/ignore/src/walk.rs
index d6ea9c2..4387fca 100644
--- a/crates/ignore/src/walk.rs
+++ b/crates/ignore/src/walk.rs
@@ -2221,6 +2221,18 @@ mod tests {
         assert!(!dents[0].path_is_symlink());
     }
 
+    #[test]
+    #[should_panic(expected = "oops!")]
+    fn panic_in_parallel() {
+        let td = tmpdir();
+        wfile(td.path().join("foo.txt"), "");
+
+        WalkBuilder::new(td.path())
+            .threads(40)
+            .build_parallel()
+            .run(|| Box::new(|_| panic!("oops!")));
+    }
+
     #[cfg(unix)] // because symlinks on windows are weird
     #[test]
     fn symlink_loop() {

```

Apply the above diff, then run the following command:

```shell
> cd crates/ignore
> timeout -k 2 1 cargo test --lib -- parallel
```

I typically have to run this many many times on my very fast laptop to get it to trigger. More reliably, you can run:

```shell
> cd crates/ignore
> cargo test
```

And running all the tests at once makes this trigger much more reliably.

### What is the actual behavior?

The process hangs, typically somewhere in the `Worker::get_work()` method. If I add a print statement within this sub-`loop`: https://github.com/BurntSushi/ripgrep/blob/de4baa10024f2cb62d438596274b9b710e01c59b/crates/ignore/src/walk.rs#L1695-L1707 it will print this out endlessly.

This especially seems to occur more often if the panic handler is non-trivial: I first noticed this when using `color-eyre`, which prints the panic to the console after composing a backtrace.

### What is the expected behavior?

The `WalkParallel::visit()` method should not hang, and should instead propagate the panic.

My first attempt to solve this (see https://github.com/cosmicexplorer/ripgrep/commit/246c1621760b41f631606c7b3061a25efe86e0fc) set the `quit_now` flag if any worker panics, and checked `self.is_quit_now()` within the sub-`loop`. This seems to have made it repro less often, but I am still able to get it to repro regardless, even without modifying the panic handler.

There are some other changes I want to propose to the `ParallelVisitor{,Builder}` traits as well which may make this easier (such as propagating unwind safety), but I'm currently hoping to solve this without making any changes to the external interface. This may require changing the parallelism primitives we use, but maybe not.

---

_Label `bug` added by @BurntSushi on 2025-08-17 21:05_

---

_Comment by @cosmicexplorer on 2026-01-16 06:15_

I wrote a very thorough report in my comment on the PR https://github.com/BurntSushi/ripgrep/pull/3010#issuecomment-3585898743 which described three separate components of what I believe to constitute a much more robust architecture for parallel directory traversal. Since this is the issue we're keeping to track parallelism strategies for this *inherently recursive* and *completely unpredictable* problem, I wanted to highlight the two points specifically relevant to the hang this issue describes:

# File Nodes Do Not Spawn New Work
> Personally, I have also separated the worker threads by category, and given each category individually configurable thread counts. I don't know if that will help performance, but I do think that the definition of "done" differs based upon whether the current node is a directory or not, which I think may justify slightly different looping techniques.

This just refers to the observation that visiting a file node is not going to produce any further recursive work like a symlink or directory would. The actual work a file visit is likely to perform also necessarily involves negotiating through the filesystem's block layer, which is almost an entirely distinct field of research than the VFS abstraction provided by the OS.

## Parallelism in Non-Recursive Directory Iteration Requires More Research
The `getdents()` method (described at length below) introduces a kind of "SIMD" primitive operation. Now we can retrieve enough data from the OS at once to meaningfully avoid rapid-fire blocking on syscalls, and instead look to *pipeline* our computations across distinct threads of control. There are many ways to conceive of this. I would say the most general one I have found (which can be translated to many distinct execution models) is:

0. [opendir] attempt to retrieve a file descriptor from an input path string
1. [spread] `getdents()` upon an open directory node produces a stream of directory entries
2. [classify] use the `d_type` field of `posix_dent` to classify the entry kind.
    - depending on platform support this may be set to the "unknown" value, requiring an additional `stat()` call
    - depending upon the `d_name` and `d_type`, the entry may either be discarded, or sent into a *worker pool* as a unit of work
3. [expand] extend symlinks as a string
4. [lstat] symlinks have no associated metadata like directory entries, so we must dereference the path string
5. [open\<file\>] do the user-defined file action
    - similar for other types

**You'll note that one worker pool must cover directories. This introduces the exact same kind of cyclic dependency that led to the livelock described in this issue!**
- However, we now have a barrier between them—this is a length-2 cycle.
- Practically speaking, it means we can tag the outputs of `getdents()` calls to identify when a worker has completed at a distinct point in time.
    - See the explicit codification of "done" vs "error": https://github.com/cosmicexplorer/rust/blob/87c614c0532125b2536b05c79c2cbafc2364149a/library/std/src/sys/fs/unix.rs#L1052

# VFS Abstractions Have Implicit Lifetimes
On that note, the VFS abstractions provided by the rust stdlib utterly fail to represent the lifetime bounds of their source material. This led to a string of decisions, which include [copying every path string to a new heap-allocated buffer to add a null byte before any vfs syscall](https://github.com/rust-lang/rust/blob/18ae99075575810a158cc670dcc7579f1c2ca012/library/std/src/sys/helpers/small_c_string.rs#L22) and then immediately dropping that allocation.

Since I haven't produced an apples-to-apples benchmark yet, I will simply note how the `ignore` crate (correctly and effectively) relies upon repeatedly joining path components together for use as unique identifiers. This is in many ways more robust than the guarantees provided by e.g. an inode in a multithreaded/multiprocess environment (like I said, I've been researching this very deeply), but it also triggers the heap allocation code path hidden in the stdlib for vfs syscalls.

My in-progress diff against the rust stdlib wraps an opened directory fd witb a read-write lock and generalizes this into a very simple trait with methods that propagate the lifetime bounds of `&self`: https://github.com/cosmicexplorer/rust/blob/87c614c0532125b2536b05c79c2cbafc2364149a/library/std/src/sys/fs/unix.rs#L863
```rust
    pub(super) trait FileDescriptorHandle {
        type FdRead<'h>: ops::Deref<Target = fd::RawFd> where Self: 'h;
        type FdWrite<'h>: ops::DerefMut<Target = fd::RawFd> where Self: 'h;

        fn as_fd<'h>(&'h self) -> Self::FdRead<'h>;
        fn as_mut_fd<'h>(&'h self) -> Self::FdWrite<'h>;
    }
```

## `getdents()` and `readlink{,at}()` as Case Studies for Fixed-Size Buffers
There are many things to say about `getdents()`, which I will be able to say more fully after my third (and hopefully final) whack at translating its model of the universe into rust (see rust-lang/libc#4522 regarding stabilization).

While the size of the`getdents()` buffer size is very directly interpretable as a classic flow control mechanism that trades off between latency and throughput, I have also introduced a separate API for reading symlinks which makes use of such a provided buffer: https://codeberg.org/cosmicexplorer/deep-link/src/commit/1ea3eba5d599d8c48ea56816b7103b12ee49d505/d-major/readdir-sys/src/wrappers.rs#L397
```rust
  fn readlink_static_impl<'buf>(
    mut buf: Pin<&'buf mut [MaybeUninit<u8>]>,
    f: impl FnOnce(*mut libc::c_char, usize) -> isize,
  ) -> Result<Pin<&'buf ffi::CStr>, ReadlinkError> {
    if buf.is_empty() {
      return Err(ReadlinkError::BufferTooSmall { len: buf.len() });
    }
    if buf.len() > isize::MAX as usize {
      return Err(ReadlinkError::BufferTooLarge { len: buf.len() });
    }
    let base_ptr: *mut libc::c_char = buf.as_mut_ptr().cast();
    let rc = f(base_ptr, buf.len());
    if rc == -1 {
      return Err(io::Error::last_os_error().into());
    }
    /* POSIX specifies either -1 or >= 0. */
    debug_assert!(rc >= 0);
    let num_read = rc as usize;
    // ...
  }
```

Instead of *unconditionally* starting small then repeatedly allocating in a loop like the stdlib, exposing a slice here means you can identify the precise path to the  symlink with the linked data length that exceeded your expectations.

I subsequently leverage `ops::ControlFlow` to translate the success/failure cases of this logic upon fixed slice lengths into a loop that reallocates a `Vec` like the stdlib. `ops::ControlFlow` is a really useful way to codify the ambiguities of iteration logic in systems with multiple interacting components like this one.

### `Arc::make_mut()` Minimizes Copies with Lifetimes

So I've built this tower of type parameters and carefully propagated lifetime bounds through our associated types. I am pretty confident this third implementation I've finally landed on is *correctly* decoupled to this degree.

One reason this can be helpful is that it can identify places where one concrete type was playing two distinct roles. See my comment which describes the `getdents()` buffer as a counterexample to simpler models of extern types https://github.com/rust-lang/rust/issues/43467#issuecomment-3741642799 which attempts to describe how knowledge of alignment vs field layout can be meaningfully demarcated in cases like `getdents()`.

Another reason is that it can help to avoid overeager cloning just because the type system said so. If you know your file visitors literally never need to know anything but the file descriptor itself, then you can pipeline the `open()` calls in a distinct phase/pool and avoid cloning path strings! And maybe you can close the parent dir now too!

But this becomes more difficult without lifetimes, and the stable `fs::{ReadDir,DirEntry}` structs directly employ `Arc` to avoid lifetime bounds. So then what to do about the `getdents()` buffer if the user is still playing with a`DirEntry` struct synthesized from data in the buffer in one thread, and also wants to read the next batch of entries in another thread? https://github.com/cosmicexplorer/rust/blob/87c614c0532125b2536b05c79c2cbafc2364149a/library/std/src/sys/fs/unix.rs#L1249
```rust
    impl<Buf, MutRef> IterEntries<Pin<MutRef>>
    where
        MutRef: CloneMutRef<R = Buf>,
        Buf: AsMut<[mem::MaybeUninit<u8>]> + Clone,
    {
        fn next_getdents_entries<'fd>(
            buf: MutRef,
            fd: &'fd mut fd::RawFd,
        ) -> ChunkedStreamResult<Self, io::Error> {
            let mut buf = pin::UnsafePinned::new(buf);
            let mut buf_pin = unsafe { Pin::new_unchecked(&mut buf) };
            let entries: ops::Range<*const u8> = unsafe {
                // Ensure we have unique ownership of the now-pinned allocation.
                let buf = unsafe { &mut *(buf_pin.as_mut().get_mut_pinned()) }.make_unique_mut();
                // Now we can point to it freely.
                getdents_impl::getdents_single(buf.as_mut(), fd)?.as_ptr_range()
            };
            let buf = unsafe { Pin::new_unchecked(buf.into_inner()) };
            let state = IterState::with_buf(buf);
            ChunkedStreamResult::Entries(Self { state, entries })
        }
    }
```

So I would say that lifetimes remain a very useful way to categorize complex runtime interactions, even when the "lifetime" is more complex than the basic Drop model supports at the moment.

---
