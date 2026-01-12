```yaml
number: 1075
title: "ignore crate: Option to validate no glob matches past project directory"
type: issue
state: closed
author: killercup
labels:
  - enhancement
  - question
assignees: []
created_at: 2018-10-03T20:49:05Z
updated_at: 2019-01-27T18:09:50Z
url: https://github.com/BurntSushi/ripgrep/issues/1075
synced_at: 2026-01-12T16:13:22Z
```

# ignore crate: Option to validate no glob matches past project directory

---

_@killercup_

While talking with @ashleygwilliams about the implementation of ignore files in cargo-generate, we noticed that we currently don't have a way to validate ignore files. In our context, "validate" means "ensure that all globs make sense for cargo-generate, and to warn users on ones that can't have any effect on a project".

Concretely, a glob such as `../foo` in a `.genignore` file in the project root will not make any sense and is probably a typo we want to notify the user about. (I don't even know how ignore deals with these relative paths.)

Would it be possible to add a method to `WalkBuilder` to analyse the current internal state and emit errors when encountering a glob that is not local to one of the root paths?

---

_Comment by @BurntSushi on 2018-10-08 18:17_

I'm not sure exactly how to go about this. At a surface level, a [`Gitignore` contains a `Vec<Glob>`](https://github.com/BurntSushi/ripgrep/blob/acf226c39d79264256c0295b8381f8c7f0d74d59/ignore/src/gitignore.rs#L83), where `Glob` is already an exported type. We could add a method that returns a `&[Glob]`. A `Glob` in this context is [strictly informational](https://github.com/BurntSushi/ripgrep/blob/acf226c39d79264256c0295b8381f8c7f0d74d59/ignore/src/gitignore.rs#L25-L75), but you could use it to look for problematic patterns and warn about them. However, I'm not sure how to expose this API in a convenient way during directory traversal. It would probably mean attaching `Gitignore` values to `DirEntry`, which I don't feel too great about.

Popping up a level, a glob such as `../foo` is not actually invalid, AFAIK. The `ignore` crate does its best to adhere to the specification defined in `man gitignore` when reading `.gitignore` files (and, as a convenient consequence, `.ignore` and any other ignore-esque files). As far as I can tell, `../foo` is not specifically outlawed by `man gitignore`. With that said, I'm struggling to figure out under which conditions such a pattern would match something. On Unix at least, I'm pretty sure that `..` is not a valid directory name, such that it should never match. But I don't know what the story is on Windows. If we could make an argument that it never meaningfully matches, then I might be OK treating it as an error (in much the same way that, say, `**.c` is treated as an error today). With that said, I'd like to emphasize that I do not feel great about this; this part of ripgrep has historically been a very subtle source of bugs, and I'm hesitant to deviate from the spec. Therefore, making this into a disabled-by-default configuration knob is probably the way to go.

The `ignore` crate does already provide an API for querying errors that occur as part of parsing `gitignore` files. Specifically, a [`DirEntry` has an `error` method](https://docs.rs/ignore/0.4.4/ignore/struct.DirEntry.html#method.error) that reports errors found while parsing ignore files. So I imagine these more "paranoid" errors would surface via this method, which you could then emit as warnings.

---

_Label `enhancement` added by @BurntSushi on 2018-10-08 18:19_

---

_Label `question` added by @BurntSushi on 2018-10-08 18:19_

---

_Comment by @BurntSushi on 2019-01-27 18:09_

I'm going to close this because it's not quite clear what the next actionable step is here to me. It might make sense to implement this on a case by case basis, so that application specific logic can be used to determine whether a glob is sensible or not.

---

_Closed by @BurntSushi on 2019-01-27 18:09_

---
