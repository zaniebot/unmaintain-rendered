```yaml
number: 216
title: Get number of matched entries for each directory
type: issue
state: closed
author: jacwah
labels:
  - question
assignees: []
created_at: 2016-11-03T13:57:44Z
updated_at: 2016-11-19T14:32:26Z
url: https://github.com/BurntSushi/ripgrep/issues/216
synced_at: 2026-01-12T18:23:11Z
```

# Get number of matched entries for each directory

---

_@jacwah_

I'm creating a `tree`-like utility (https://github.com/jacwah/ntree) and the `ignore` crate seems really useful for implementing it. There is however an issue: to render the tree, the program needs to know the number of matched entries in each directory, or at least somehow query `Walk` for each entry to see if there are more entries left in the same directory.

Pseudocode examples below to illustrate what I mean:
```
for entry in walk {
    if entry.is_dir() {
        process_dir(entry.path(), entry.get_num_entries());
    }
    // ...
}

for entry in walk {
    process_entry(entry.path(), entry.parent_has_more_entries();
}

for entry in walk {
    process_entry(entry.path(), entry.peek_next_sibling().is_some();
}
```

Is there currently a way to do this with the `ignore` crate I've missed? If not, are you willing to include such a feature? I'm available for discussion and implementation work :)

Thanks!

---

_Comment by @BurntSushi on 2016-11-03 14:19_

Thanks for reaching out! My first reaction is: why do you need to know the number of matched entries in each directory? (It isn't obvious to me from looking at the output of `tree`, but perhaps I am missing something!)

Assuming you do in fact need it...

I think we should simplify the problem first. Irrespective of the `ignore` crate, can you achieve what you want using the `walkdir` crate? I kind of suspect not, since `walkdir` does a _depth first_ traversal of the directory tree as opposed to breadth first. This is pretty important, because it means your resource (memory + file descriptors) use scales with the depth of the tree. In a breadth first traversal, you need to keep around a lot more state.

There's really no way to adapt a depth first search to a breadth first search without making sacrifices like "store the entire tree in memory." So I think you're kind of hosed in that regard.

I think the only path forward here is to implement a new breadth first recursive iterator, which would have to live in this crate, since you'll really want to make use of internal APIs like the one found in `ignore/src/dir.rs`.


---

_Comment by @jacwah on 2016-11-03 14:49_

Thanks for answering so quickly!

Take a look at this output:

```
.
├── a
├── b
│   ├── 1
│   ├── 2
│   └── 3
├── c
└── d
```

To render the lines of the contents of `b`, the program needs to know that there are in fact more entries of `.` to come to continue drawing the leftmost vertical line.

Thanks also for the primer on depth first vs breadth first! I'll be sure to poke around the source to see whether me getting something working is feasible or not. Is ok if I contact you with further questions here? (or maybe through some other medium if you prefer)


---

_Comment by @BurntSushi on 2016-11-03 14:58_

If you can deal with only needing to look at whether there is a "next" element or not, then you should be able to use a peekable iterator, no? You can turn any iterator into a [`peekable`](https://doc.rust-lang.org/std/iter/trait.Iterator.html#method.peekable) iterator. The next element should also give you the _depth_ of the entry, which I imagine might be useful too.


---

_Comment by @BurntSushi on 2016-11-03 15:08_

Oh, and yes, asking more questions here is encouraged. Please do!

You can also find me on most of the Rust IRC channels with the same `burntsushi` handle. However, I don't log on to IRC during work. Prime time for me tends to be 4-9PM EST and potentially large chunks of time on the weekend, but that can vary.


---

_Comment by @BurntSushi on 2016-11-03 15:15_

With `peekable`, you can look at the next entry, and if one exists and its depth is less than the current entry, then you know you can use a `terminal_line` instead of a `branched_line`. Does that sound right?


---

_Comment by @jacwah on 2016-11-03 16:01_

Just peeking the next entry is not enough. Consider the following output:

```
.
├── a
├── b
│   ├── 1
│   ├── 2
│   └── 3
├── c
└── d
    ├── 1
    ├── 2
    └── 3
```

To know whether to terminate the line from `.` at `d` or not, the program has to peek at the (optional) next sibling, not just the next element.


---

_Comment by @BurntSushi on 2016-11-03 16:13_

Ah, I see. Then I think you're forced to use breadth first search unfortunately. (Or build an alternative depth first search that does the right amount of peeking you need.)


---

_Label `question` added by @BurntSushi on 2016-11-03 20:53_

---

_Comment by @BurntSushi on 2016-11-19 14:32_

@jacwah I'm going to close this because I'm not sure there's anything actionable here and I personally don't have the bandwidth to support this use case. I might be amenable to maintaining one. If you'd like to discuss an implementation plan, feel free to open a new issue (or just comment in this one and we can re-open it). Thanks!


---

_Closed by @BurntSushi on 2016-11-19 14:32_

---
