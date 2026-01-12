```yaml
number: 469
title: "How to finalize walk in ignore::WalkParallel"
type: issue
state: closed
author: elliottslaughter
labels:
  - enhancement
  - question
  - icebox
assignees: []
created_at: 2017-04-30T22:38:41Z
updated_at: 2020-02-17T22:16:38Z
url: https://github.com/BurntSushi/ripgrep/issues/469
synced_at: 2026-01-12T16:13:22Z
```

# How to finalize walk in ignore::WalkParallel

---

_@elliottslaughter_

Hi there,

I have a use case for `ignore::WalkParallel` where I want to accumulate results into a local data structure in each thread, and then at the end of the walk merge the local results into a single global data structure. I could implement this by wrapping a single global copy of the data structure in a lock or similar, but that just seems wasteful, since there isn't any need for the threads to interact until the very end. Keeping the partial results local is a classic optimization in parallel programming.

As best I can tell, I can pass a local data structure to each thread by building it in the `mkf` function and passing it into the closure I produce. However, I can't figure out any way to get the local data structures back out, in part because I don't see that the threads receive any notice that the walk is ending. The threads just keep getting `DirEntry`s until they don't, and at that point it's already too late to do anything.

Can you suggest any way to do what I want with `ignore::WalkParallel`? Or if this isn't possible right now, could you consider it as a feature request?

Thanks.

---

_Comment by @BurntSushi on 2017-04-30 23:22_

Could you describe the problem you are trying to solve? (i.e., Without talking about the ignore crate, or perhaps even better, without mentioning threads altogether.)

---

_Comment by @elliottslaughter on 2017-05-01 06:53_

I'm writing a tool that walks files recursively inside a directory and computes various statistics about each file (e.g. checksums). I collect the statistics into a in-memory tree data structure that mirrors the file system, and then use that data structure for various purposes. For example, I can perform a "diff" of sorts against another directory without needing to access the original directory, possibly on another machine, or at a later time when the original data doesn't exist any more.

I'd also like to have at least some level of support for intelligently ignoring files, though I'm not yet sure if it makes sense to turn it on by default the way that ripgrep does.

The operations I'm doing on files can be at least mildly compute intensive (e.g. checksums), so I expect I'll need some degree of parallelism to saturate disk bandwidth. For certain shapes of file systems (with many small files and deeply nested directories), parallelizing the directory traversal itself might matter as well.

Having thought about this some more, I suppose I could just separate file processing from the directory traversal. I.e., I could use parallel directory traversal to dump a list of files, then hand that list to a completely different set of threads responsible for processing files. (I'm not that familiar with Rust parallelism, but if Rust has a channel abstraction or something similar, that would work.) The big downside would be complexity. And it's not clear to me if the performance will be better or worse than just biting the bullet in order to do everything inside `ignore::WalkParallel` and paying the cost to synchronize on every access to the tree.

---

_Comment by @BurntSushi on 2017-05-01 12:03_

Thank you for writing that out. That's very helpful and makes it much easier for me to understand what you're trying to do. :-)

So I'll just say this up front: I think the current API is pretty bad. I am quite unhappy with it. I think a closure that returns a closure is very unfriendly, and the type signature that results from it is hairy. Hell, I wrote the damn thing and it still takes me a moment to digest the type signature. On top of this, I think the closure API precludes some sort of obvious "finalization" step that you're looking for (but it's not impossible, see below). The one thing that the closure API has going for it is that it's very succinct, and at the time, I didn't want to spend the effort building a nicer API. Perhaps you can help me figure out a better API? The type signature will actually become more complicated, but some uses I think could be simpler.

For example, I think we'd want to push at least the visitor closure into a trait:

```rust
trait WalkVisitor {
    visit(&mut self, entry: Result<DirEntry, Error>) -> WalkState;
    visit_end(&mut self) {
        // by default, do nothing
    }
}
```

We could then perhaps provide an impl for closures by default?

```rust
impl<F: FnMut(Result<DirEntry, Error>) -> WalkState> WalkVisitor for F {
    fn visit(&mut self, entry: Result<DirEntry, Error>) -> WalkState {
        (self)(entry)
    }
```

I can't tell whether we should have another trait for "building" visitors (analogous to the outer closure in today's API).

Another choice that doesn't involve traits at all might be to change the input parameter to the visitor from `Result<DirEntry, Error>` to:

```rust
enum {
    Visit(Result<DirEntry, Error>),
    End,
}
```

but I feel like this is even more cumbersome and could potentially limit what you might want to do upon seeing `End`. (For example, the `end` method in the `WalkVisitor` trait might want to take an owned `self`.)

But this probably requires a bit of experimentation...

---

With the above out of the way, I think the simplest thing for you to do would indeed be to use a channel of some sort. The `WalkParallel` implementation already sends every file path over a [multi-producer multi-consumer queue](https://docs.rs/crossbeam/0.2.10/crossbeam/sync/struct.MsQueue.html) (soon to be a [stack](https://docs.rs/crossbeam/0.2.10/crossbeam/sync/struct.TreiberStack.html), I think), so I don't think it would cost you much to do it yourself.

If you really wanted to chase your existing implementation idea (where you build up thread local state and then merge at the end), then I'd think you could do something with destructors, but it would be a little cumbersome. The key would probably be combining global and thread local state into a single struct:

```rust
struct State {
    thread_local: Whatever,
    global: Arc<Mutex<Global>>,
}
```

During traversal, you'd only ever modify `thread_local`, but once `State` goes out of scope, you could do a merge step by hooking into the destructor:

```rust
impl Drop for State {
    fn drop(&mut self) {
        let mut global = self.global.lock().unwrap();
        global.merge(self.thread_local);
    }
}
```

By the time `WalkParallel::run` completes, you should be guaranteed that each of your destructors will have run and everything will have been merged back into your more "global" state.

---

_Comment by @elliottslaughter on 2017-05-01 17:20_

Thanks for the comments, these are very helpful.

I should preface this by saying I'm hardly an expert in Rust, but of the options you listed, the WalkVisitor trait looks like the nicest way to do this. It's flexible, reasonably clean, preserves the ability to work directly with closures (if that's what you want), and can be extended in a straightforward way if you determine that there are other things you want the interface to do. I'm not sure I see a reason to turn the outer builder into a trait since there's really only one thing it needs to do, though I wouldn't stand against it either.

---

_Label `enhancement` added by @BurntSushi on 2017-05-08 21:27_

---

_Label `icebox` added by @BurntSushi on 2017-05-08 21:27_

---

_Label `question` added by @BurntSushi on 2017-05-08 21:27_

---

_Comment by @durka on 2017-08-07 17:57_

I would like to see this improved as well. My application is quite similar to @elliottslaughter -- I want to have a local counter on each thread and join them after iteration. Another wishlist item: provide a value to `WalkState::Quit` and have `run` return it (I just want to die on the first error, but I didn't see a simpler way to do that than using an `Arc<Mutex<Option<Error>>>`). I'm not sure if that is even possible without a lot more generics.

---

_Comment by @epage on 2019-10-27 00:57_

Something related to this is blocking me, so I'm starting work on it.

```rust
/// Create per-thread `ParallelVisitor`s for `WalkParallel`.
pub trait ParallelVisitorBuilder {
    /// Create per-thread `ParallelVisitor`s for `WalkParallel`.
    fn build(&mut self) -> Box<dyn ParallelVisitor>;
}

/// Receives files and directories for the current thread/
///
/// - Setup can be implemented in `ParallelVisitorBuilder::build`
/// - Teardown can be implemented in `trait Drop`.
pub trait ParallelVisitor: Send {
    /// Receives files and directories for the current thread/
    fn visit(&mut self, entry: Result<DirEntry, Error>) -> WalkState;
}
```

and I am adding a
```rust
impl WalkParallel {
    pub fn visit(self, builder: &mut dyn ParallelVisitorBuilder);
}
```
I am leaving `run` (with it converting the `FnMut` into a `ParallelVisitorBuilder`) because
- For simple cases, it is much easier to read the logic inline
- Helped to verify I did my `trait`s correctly
- Decouples adding this feature and discussing when to break compatibility.

If people have better names, I'm fully willing to take them

---

Another thought I had is to provide an optional way for the threads to return values.  We could have the interface be something like
```rust
pub trait ParallelVisitorBuilder {
    type VisitorOutput = ();
    type Visitor: ParallelVisitor<Output=VisitorOutput>;
    type Output = ();

    fn build(&mut self) -> Self::Visitor;

    fn reduce(self, output: &[Self::VisitorOutput], state: WalkState) -> Self::Output {}
}

pub trait ParallelVisitor: Send {
    type Output = ();

    fn visit(&mut self, entry: Result<DirEntry, Error>) -> WalkState;

    fn reduce(self, state: WalkState) -> Self::Output {}
}

impl WalkParallel {
    pub fn map_reduce<...>(self, builder: impl ParallelVisitorBuilder) -> Output;
}
```
- We'd be switch from `dyn` to `impl`
- `reduce` gets `WalkState` to make life easier for the implementer
- This would be a separate function from `visit` to avoid the overhead of creating `Vec<VisitorOutput>`

Thoughts?  
(again, I am not married to any of the names)

---

_Closed by @BurntSushi on 2020-02-17 22:16_

---
