```yaml
number: 3012
title: "ignore: fix confusing lifetimes in ParallelVisitor{,Builder} with an associated type"
type: pull_request
state: closed
author: cosmicexplorer
labels:
  - icebox
assignees: []
base: master
head: ignore-visitor-associated-type
created_at: 2025-03-06T09:47:09Z
updated_at: 2025-08-17T21:11:51Z
url: https://github.com/BurntSushi/ripgrep/pull/3012
synced_at: 2026-01-12T18:23:14Z
```

# ignore: fix confusing lifetimes in ParallelVisitor{,Builder} with an associated type

---

_@cosmicexplorer_

# Problem
The docs for [`WalkParallel::visit()`](https://docs.rs/ignore/latest/ignore/struct.WalkParallel.html#method.visit) state the following:
> Typically, creating a custom visitor is useful if you need to perform some kind of cleanup once traversal is finished. This can be achieved by implementing `Drop` for your builder (or for your visitor, if you want to execute cleanup for every thread that is launched).

However, by accepting an `&mut dyn ParallelVisitorBuilder<'_>`, the `visit()` method makes it rather unclear when the builder's `Drop` method is called. There's also no example of this custom visitor usage in the repo, and all examples instead use the neighboring `.run()` method.

It's also relatively unclear what the purpose of the lifetime `'s` in `ParallelVisitorBuilder<'s>` is--I understand it's supposed to just avoid the need to enforce a `'static` lifetime so we can use `std::thread::scope()`, but implementors may not know what to do with it. While looking to use this API, I was under the impression that the builder would be able to create visitors scoped to the builder's lifetime, but this ended up not being possible, because of the `&mut self` requirement meaning that the builder can't hand out references to its own fields. This further increases confusion about where and how the `Drop` impl of the builder is supposed to get called.

Finally, the API requires creating a `Box` where none is necessary. This is very unlikely to cause any performance issues, but is a little awkward.

# Solution
1. Make the `ParallelVisitorBuilder` return a lifetime-annotated associated type `Self::Visitor<'s>`.
2. Make the `build()` method return a concrete instance of `Self::Visitor`, scoped to the builder's own lifetime.
3. Make the `WalkParallel::visit()` method accept an `impl ParallelVisitorBuilder`, which allows the user to move a builder into the `visit()` method so that the `Drop` method can be called immediately after the conclusion of the parallel traversal.
4. Make the `WalkParallel::visit()` method return the `ParallelVisitorBuilder` argument after completion.

# Result
The `.run()` and `.visit()` methods' signatures were modified, **but this should make them strictly more general**. I've removed a few `Box::new()` calls to demonstrate how this works, but the code will still compile with the `Box::new()` calls. The newly added example script looks like:
```rust
fn walk_dir(
    dir: impl AsRef<Path>,
) -> Result<BTreeSet<PathBuf>, ignore::Error> {
    ignore::WalkBuilder::new(dir)
        .build_parallel()
        .visit(VisitorBuilder::new())
        // This is an impl method on the returned VisitorBuilder:
        .into_result()
}

fn main() {
    println!("success: {:?}", walk_dir(".").unwrap());
    println!("err: {:?}", walk_dir("asdf"));
}
```

Because of the more expansive API which allows the builder to be consumed by the `visit()` call, it is now clear from a glance that the result of the successful first crawl will be printed to stdout, before the failing second crawl is started at all. It is also clear that if the failing second crawl is removed, there will be no deadlock, because we were able to have the builder take ownership of its data and return a result on the stack instead of creating extraneous `Arc` and `mpsc::channel` instances.

It is my belief that this approach is much easier to follow, and actually fulfills the expectations of the existing docstring for `WalkParallel::visit()`.

---

_Renamed from "ignore: avoid boxing in ParallelVisitor{,Builder} with an associated type" to "ignore: fix confusing lifetimes in ParallelVisitor{,Builder} with an associated type instead of boxing" by @cosmicexplorer on 2025-03-06 09:48_

---

_Renamed from "ignore: fix confusing lifetimes in ParallelVisitor{,Builder} with an associated type instead of boxing" to "ignore: fix confusing lifetimes in ParallelVisitor{,Builder} with an associated type" by @cosmicexplorer on 2025-03-06 09:48_

---

_Comment by @cosmicexplorer on 2025-03-06 11:17_

I believe I have just figured out how to allow the builder to return scoped data (on the stack) by having `WalkParallel::visit()` return the provided builder argument. This makes it possible for a `ParallelBuilderVisitor` implementation to produce a really slick synchronous API if desired, by consuming the builder returned by the `visit()` method (this is not required, and fully asynchronous/channel-based message passing still works, without any changes to the code--this is *strictly more general*). The added example script demonstrates how returning the builder argument can be used in a high-level API.

---

_Label `icebox` added by @BurntSushi on 2025-08-17 21:11_

---

_Comment by @BurntSushi on 2025-08-17 21:11_

I appreciate your work here, but I'm not prepared to be making semver incompatible changes to `ignore`.

I 100% agree with you that the current API is weird and awkward and confusing. I very much dislike it. And many other things about `ignore`. Unfortunately, it has been in this state for a very long time while still being "good enough." A terrible case of local optima.

I have hopes of re-building `ignore` (and `globset`) from first principles some day. Arguably, I shouldn't let perfect be the enemy of the good and accept smaller revisions like this. But that has its own costs, such as inducing churn in the ecosystem. Essentially, my bar for semver incompatible changes at this point has to include a grander vision for the future.

I tagged this as `icebox` so that it's something I can revisit for inspiration when rethinking the `ignore` API.

---

_Closed by @BurntSushi on 2025-08-17 21:11_

---
