---
number: 13718
title: "Prompt interactive terminal when using `uv init` without args"
type: issue
state: open
author: tomaszbk
labels:
  - enhancement
assignees: []
created_at: 2025-05-29T14:19:17Z
updated_at: 2025-05-29T20:11:14Z
url: https://github.com/astral-sh/uv/issues/13718
synced_at: 2026-01-10T01:25:37Z
---

# Prompt interactive terminal when using `uv init` without args

---

_Issue opened by @tomaszbk on 2025-05-29 14:19_

### Summary

when creating a new project with `uv init`, if user doesn't pass args, default to an interactive terminal to select project type:
script: runs `uv init --script main.py`
application: runs `uv init --app`
application--bare: runs `uv init --bare`
library: runs `uv init --lib`
Notebook: runs `uv init --app`, `uv add ipykernel` and `touch notebook.ipynb`

could maybe add a second prompt to ask to include vcs(git) or not.

### Example

```rust
use inquire::Select;

fn main() {
    let options = vec!["Script", "Application", "Application--Bare", "Library", "Notebook"];
    let ans = Select::new("Choose proyect type:\n", options)
        .without_filtering()
        .prompt();

    match ans {
        Ok(size) => println!("You selected: {}", size),
        Err(err) => println!("There was an error: {}", err),
    }
}
```

![Image](https://github.com/user-attachments/assets/6c4c0597-0b03-4979-ac88-4c71936475e7)

---

_Label `enhancement` added by @tomaszbk on 2025-05-29 14:19_

---

_Comment by @zanieb on 2025-05-29 14:23_

Loosely a duplicate of https://github.com/astral-sh/uv/issues/6510

---

_Comment by @tomaszbk on 2025-05-29 20:11_

should I close this issue and comment this proposal in https://github.com/astral-sh/uv/issues/6510 ?

btw, I wouldn't have a problem creating the PR for both these issues, if you think they are a good idea

---
