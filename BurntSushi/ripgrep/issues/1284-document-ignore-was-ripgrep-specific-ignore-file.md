```yaml
number: 1284
title: "Document .ignore (was: ripgrep-specific ignore file)"
type: issue
state: closed
author: SimonSapin
labels:
  - question
  - doc
assignees: []
created_at: 2019-05-24T06:38:09Z
updated_at: 2019-08-01T21:46:32Z
url: https://github.com/BurntSushi/ripgrep/issues/1284
synced_at: 2026-01-12T16:13:23Z
```

# Document .ignore (was: ripgrep-specific ignore file)

---

_@SimonSapin_

Only after I finished typing in full a feature request (preserved below) did I find out about `.ignore` files by reading other issues suggested by GitHub.

* https://github.com/BurntSushi/ripgrep/blob/d1e4d28f3/README.md only mentions `.gitignore` and the `ignore` crate
* https://github.com/BurntSushi/ripgrep/blob/d1e4d28f3/ignore/README.md mentions `.ignore` exactly once
* To be fair, `rg --help` does mention `.ignore` multiple times.

Since that name is hard to look up in a search engine, I’m not sure what other tools also consider `.ignore` by default and whether I *wouldn’t* want that for some use cases. What other tools do you know of that do?

What do you think could be ways to more prominently document `.ignore`?

----

<details>
<summary>Feature request for <code>.rgignore</code></summary>

#### What version of ripgrep are you using?

```
ripgrep 11.0.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

`cargo install`

#### What operating system are you using ripgrep on?

Ubuntu 18.04

#### Describe your question, feature request, or bug.

Some code repositories contain generated files (with long lines) that should be excluded from being searched by ripgrep. Currently this can be done by adding a pattern to `.gitignore`, but this of course also makes the files ignored by git.

It would be nice to have a file, possibly named `.rgignore`, that lists patterns to be ignored by ripgrep but not by version control systems.

#### If this is a bug, what are the steps to reproduce the behavior?

```
git clone https://github.com/rust-lang/rust
cd rust
rg ptr::NonNull
```

#### If this is a bug, what is the actual behavior?

The console is flooded with the entire contents of `src/tools/rls/rls-analysis/test_data/rust-analysis/libcore-a5db6a3445116c08.json`, which is a 3 MB single-line JSON file.

#### If this is a bug, what is the expected behavior?

The repository maintainers should have a way to specify that ``src/tools/rls/rls-analysis/test_data/rust-analysis/*.json` should be excluded from ripgrep results by default.

</details>

---

_Comment by @BurntSushi on 2019-05-24 10:35_

It's also described in the guide: https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#automatic-filtering

We could change `.gitignore` to `.gitignore`/`.ignore`/`.rgignore` in this section? https://github.com/BurntSushi/ripgrep/blob/d1e4d28f3/README.md#why-should-i-use-ripgrep Otherwise I'm not sure. The README is just meant to give a high level overview.

---

_Label `question` added by @BurntSushi on 2019-05-24 10:35_

---

_Label `doc` added by @BurntSushi on 2019-05-24 10:35_

---

_Comment by @SimonSapin on 2019-05-24 10:41_

TIL about the guide, thanks!

Yes, I think that change would be good.

---

_Closed by @BurntSushi on 2019-08-01 21:46_

---
