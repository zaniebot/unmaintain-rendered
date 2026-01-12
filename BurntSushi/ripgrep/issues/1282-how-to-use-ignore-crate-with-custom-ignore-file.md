```yaml
number: 1282
title: How to use ignore crate with custom .ignore file
type: issue
state: closed
author: ghost
labels:
  - question
assignees: []
created_at: 2019-05-16T05:17:27Z
updated_at: 2019-05-29T17:41:19Z
url: https://github.com/BurntSushi/ripgrep/issues/1282
synced_at: 2026-01-12T16:13:23Z
```

# How to use ignore crate with custom .ignore file

---

_@ghost_

#### Version

I am using the crate `ignore` version 0.4.7 in my rust program. It was auto-installed by `cargo`.

#### System

My operating system is Arch Linux 4.19

#### Problem

I don't know how to use the ignore crate in my rust program when using a custom .ignore file even after looking at the `ignore` crate documentation.

My use case is that I want to generate a list of all filepaths underneath a given root directory (say `~/main`) but exclude certain filepaths according to patterns given in a custom .ignore file (say `main.ignore`), which resides in the `/src`.  Example ignore patterns include `code` and `**/screen_capture`, so those directories will be excluded from the walk.

The partial filesystem structure looks like this:
```
home
---> main 
        ---> images
              ---> screen_capture (should be pruned)
       ---> code (should be pruned)
```

The problem is that none of the patterns that I provide in my `main.ignore` file prunes the associated files. 

I wish there was an in-depth example that would show how to iterate through a walk that excludes files given a custom ignore file. 

I am not keen on using any existing .gitignore files as a way of excluding files because not all the directories I want to prune are git projects and even if they were, I don't necessarily want to prune the files based on their .gitignore files.

#### Code

```
let mut builder = WalkBuilder::new(root_path); // root_path is ~/main
builder.add_custom_ignore_filename("main.ignore"); // main.ignore is in /src, same directory as main.rs
for result in walker.build () {
// this is where I perform an operation across each result
}
```
I have also tried using the `add_ignore` function instead of `add_custom_ignore_filename` to no effect. No matter what, no files end up being pruned.

#### Additional Questions

How do I exclude the contents of a directory but not the directory itself? For example, I might want to exclude the contents of the `code` directory but still include the filepath for code (/home/main/code) in the results.



---

_Comment by @BurntSushi on 2019-05-16 12:51_

Thanks for the bug report. In the future, please provide a complete working example that compiles, runs and shows some output. Please also then include, precisely, what output you expect. Without these things, I have to guess. For example, you mention both `/src` and `code` directories, and you've included code that doesn't actually compile.

As it stands, I cannot reproduce your problem.

Here's my program:

```rust
use ignore::WalkBuilder;

fn main() {
    let mut builder = WalkBuilder::new("main");
    builder.add_custom_ignore_filename("main.ignore");
    for result in builder.build() {
        let dent = result.unwrap();
        println!("{}", dent.path().display());
    }
}
```

And here's my directory tree under `main`:

```
$ tree main
main
├── code
│   └── test.rs
├── images
│   └── screen_capture
└── main.ignore

3 directories, 2 files
```

And the contents of `main/main.ignore`:

```
$ cat main/main.ignore
code
**/screen_capture
```

Running the program yields:

```
$ ./target/release/ignore-1282
main
main/main.ignore
main/images
```

Which is presumably what you want. Since you **did not provide a complete reproduction**, it's not clear why things aren't working for you. There is likely some other environmental factor at play. In particular, the above is inside my `/tmp` directory, where there is no other `.gitignore` or `.ignore` file that could impact the results of directory traversal. Since you are searching in your `$HOME` directory, you might have `.gitignore` or `.ignore` files that are impacting your results.

> How do I exclude the contents of a directory but not the directory itself? For example, I might want to exclude the contents of the code directory but still include the filepath for code (/home/main/code) in the results.

By using globs? With the example above, here's the new `main.ignore` file:

```
$ cat main/main.ignore
code/*
**/screen_capture
```

And now running the program yields:

```
$ ./target/release/ignore-1282
main
main/main.ignore
main/code
main/images
```

Notice that it includes `main/code` but not `main/code/test.rs`.

---

_Label `question` added by @BurntSushi on 2019-05-16 12:51_

---

_Comment by @BurntSushi on 2019-05-16 12:52_

> I wish there was an in-depth example that would show how to iterate through a walk that excludes files given a custom ignore file.

It is very likely that the `ignore` crate will at some point go through a major overhaul, since it is sub-optimal in a number of respects. Unfortunately, the implementation is complex, so it takes time to do this. Therefore, my motivation for documenting something that is likely going to evaporate is pretty low.

You might consider other alternatives to the `ignore` crate if this is a major impediment for you.

---

_Comment by @BurntSushi on 2019-05-29 17:41_

Closing due to inactivity from @starun96 

---

_Closed by @BurntSushi on 2019-05-29 17:41_

---
