```yaml
number: 289
title: "Expose a public 'check' method"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/lib
created_at: 2022-09-29T22:30:31Z
updated_at: 2022-09-30T23:28:26Z
url: https://github.com/astral-sh/ruff/pull/289
synced_at: 2026-01-12T05:48:45Z
```

# Expose a public 'check' method

---

_Pull request opened by @charliermarsh on 2022-09-29 22:30_

See: #271.

---

_Comment by @charliermarsh on 2022-09-29 22:31_

@Seamooo - Is this roughly what's needed for the LSP? (Is there any way to get the LSP to pass us a path, even as a string, to include in the error messages?)

---

_@charliermarsh reviewed on 2022-09-29 22:44_

---

_Review comment by @charliermarsh on `src/lib.rs`:29 on 2022-09-29 22:44_

With this, you can do:

```rust
use anyhow::Result;
use ruff;

fn main() -> Result<()> {
    println!("{:?}", ruff::check("def f(): x = 1")?);
    Ok(())
}
```

---

_Comment by @charliermarsh on 2022-09-29 23:06_

@Seamooo - What are you planning to use to implement the LSP? https://github.com/ebkalderon/tower-lsp?

---

_Comment by @Seamooo on 2022-09-29 23:49_

the lsp can definitely pass a path, the source requirement is due to the dynamic nature of the buffer updating. I've implemented a rough version of the lsp from scratch https://github.com/Seamooo/ruffd, getting this in would enable a proof of concept.

---

_Comment by @charliermarsh on 2022-09-30 00:29_

That's awesome. And it's [here](https://github.com/Seamooo/ruffd/blob/main/ruffd-core/src/server_ops.rs#L62) that we can plug into ruff as a library?

---

_Comment by @charliermarsh on 2022-09-30 00:30_

(Did you write that implementation from scratch? Is it based off anything else?)

---

_Comment by @charliermarsh on 2022-09-30 00:33_

Two questions:

1. Should I change the method signature to include something like `path: &str` or even `path: Path`? Some of the internals like to have a `Path`, e.g., for formatting the `Message` body, but it could be worked around if it needs to be omitted.
2. How should we determine the `pyproject.toml` path here? Is there any way to base it on the user's current working directory? Or can the LSP path an absolute path to a file?


---

_Marked ready for review by @charliermarsh on 2022-09-30 00:35_

---

_Comment by @charliermarsh on 2022-09-30 00:36_

(I went ahead and did (1); if we pass the absolute `Path`, it will in turn solve (2).)

---

_Comment by @Seamooo on 2022-09-30 00:50_

> That's awesome. And it's [here](https://github.com/Seamooo/ruffd/blob/main/ruffd-core/src/server_ops.rs#L62) that we can plug into ruff as a library?

Exactly

---

_Comment by @Seamooo on 2022-09-30 00:52_

> (Did you write that implementation from scratch? Is it based off anything else?)

it's implemented from scratch from the specification, with coc-nvim as the client to verify

---

_Comment by @Seamooo on 2022-09-30 00:53_

There's potentially some ambiguity for solving the `pyproject.toml` path. The easiest would just be using the workspace root provided by the lsp client, but potentially there's cases where the workspace has nested configurations underneath it. For that case the implementation would be to recurse from the file path upwards until finding a `pyproject.toml` terminating at the workspace root. If the public method takes in `&Settings`, potentially the public api shouldn't have to account for it. This may also enable global configs outside of pyproject.toml that can create the settings type.

---

_Comment by @charliermarsh on 2022-09-30 15:30_

> For that case the implementation would be to recurse from the file path upwards until finding a pyproject.toml terminating at the workspace root.

If we pass `check` a path to a file, then `find_project_root` will automatically do this.


---

_Merged by @charliermarsh on 2022-09-30 15:30_

---

_Closed by @charliermarsh on 2022-09-30 15:30_

---

_Branch deleted on 2022-09-30 15:30_

---

_Comment by @charliermarsh on 2022-09-30 15:30_

@Seamooo - I've merged this for now. Let me know if it works for you or if we need to amend the API!

---

_Comment by @Seamooo on 2022-09-30 17:34_

![image](https://user-images.githubusercontent.com/48548685/193325446-08c3931c-6479-4a39-8b81-f660439bb7d9.png)
looking good!

---

_Comment by @charliermarsh on 2022-09-30 18:03_

Nice! That's awesome!

What does it take to enable others to use the LSP from VS Code and elsewhere?


---

_Comment by @charliermarsh on 2022-09-30 23:28_

(E.g., do we need to publish this on some sort of extension marketplace?)

---
