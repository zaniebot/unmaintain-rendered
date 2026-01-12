```yaml
number: 1539
title: ParallelVisitor being called multiple times with the same path
type: issue
state: closed
author: orf
labels:
  - invalid
assignees: []
created_at: 2020-04-03T18:49:06Z
updated_at: 2020-04-03T18:58:36Z
url: https://github.com/BurntSushi/ripgrep/issues/1539
synced_at: 2026-01-12T16:13:23Z
```

# ParallelVisitor being called multiple times with the same path

---

_@orf_

I'm experimenting with the [ParallelVisitor](https://docs.rs/ignore/0.4.14/ignore/trait.ParallelVisitor.html) feature of the ignore crate, and I came across some surprising behaviour:

```rust
fn main() {
    let mut walker = WalkBuilder::new(path);
    walker
        .hidden(false)
        .parents(false)
        .ignore(false)
        .git_ignore(false)
        .git_global(false)
        .git_exclude(false)
        .follow_links(false);

    walker.add(PathBuf::from("... some path .."));
    let mut collection_manager = CollectorManager::new();
    walker
        .threads(10)
        .build_parallel()
        .visit(&mut collection_manager);
}

struct CollectorManager {}

impl CollectorManager {
    pub fn new() -> CollectorManager {
        CollectorManager {}
    }
}

impl<'a> ParallelVisitorBuilder<'a> for CollectorManager {
    fn build(&mut self) -> Box<dyn ParallelVisitor + 'a> {
        Box::new(Collector::new())
    }
}

struct Collector {}

impl Collector {
    pub fn new() -> Collector {
        Collector {}
    }
}

impl ParallelVisitor for Collector {
    fn visit(&mut self, entry: Result<DirEntry, Error>) -> WalkState {
        println!("{:?} = {:?}", std::thread::current().id(), entry);
        WalkState::Continue
    }
}
```

On my machine, when run against a simple directory structure like 

```
a-directory/1/saved_model.pb
a-directory/2/saved_model.pb
a-directory/3/saved_model.pb
```

I can see from the output that several threads both visit the same file. In the case below thread 3 and 4 both visit `a-directory/2/saved_model.pb`:

```
ThreadId(4) = Ok(DirEntry { dent: Raw(DirEntryRaw { path: "a-directory/3/saved_model.pb", follow_link: false, depth: 3 }), err: None })
ThreadId(3) = Ok(DirEntry { dent: Raw(DirEntryRaw { path: "a-directory/2/saved_model.pb", follow_link: false, depth: 3 }), err: None })
ThreadId(4) = Ok(DirEntry { dent: Raw(DirEntryRaw { path: "a-directory/1/saved_model.pb", follow_link: false, depth: 3 }), err: None })
ThreadId(3) = Ok(DirEntry { dent: Raw(DirEntryRaw { path: "a-directory/3/saved_model.pb", follow_link: false, depth: 3 }), err: None })
ThreadId(4) = Ok(DirEntry { dent: Raw(DirEntryRaw { path: "a-directory/2/saved_model.pb", follow_link: false, depth: 3 }), err: None })
ThreadId(2) = Ok(DirEntry { dent: Raw(DirEntryRaw { path: "a-directory/1/saved_model.pb", follow_link: false, depth: 3 }), err: None })
```

This behaviour is surprising - I assumed that each thread would visit a given `DirEntry` once. Is this a bug? 

---

_Comment by @BurntSushi on 2020-04-03 18:51_

Please provide a _complete_ code sample that compiles and runs.

---

_Comment by @orf on 2020-04-03 18:55_

Forgive me, I'm a complete idiot. I'm constructing the `WalkBuilder` with `path`, and I'm also passing it to `walker.add`.

Sorry for wasting your time.

---

_Closed by @orf on 2020-04-03 18:55_

---

_Label `invalid` added by @BurntSushi on 2020-04-03 18:58_

---
