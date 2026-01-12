```yaml
number: 1469
title: Use class/function name as range for checks about names/classes/functions
type: issue
state: closed
author: not-my-profile
labels: []
assignees: []
created_at: 2022-12-30T09:11:21Z
updated_at: 2023-04-12T15:31:07Z
url: https://github.com/astral-sh/ruff/issues/1469
synced_at: 2026-01-12T15:54:41Z
```

# Use class/function name as range for checks about names/classes/functions

---

_@not-my-profile_

To my surprise many ruff checks highlight the whole class / function, for example D101 (Missing docstring in public class) and D103 (Missing docstring in public function) are currently displayed by the Ruff VS Code extension as follows:

![screenshot showing a whole class/function being displayed with yellow squiggly underlines](https://user-images.githubusercontent.com/73739153/210053304-4b82a7a9-7513-499d-aac7-24653be4a25b.png)

This makes the code quite unreadable and also doesn't let you visually recognize other errors within the class/function. So I'd prefer it to only highlight the class/function name. Looking into it we cannot currently easily fix this in ruff because [RustPython's `StmtKind` doesn't appear to make the Range of function/class names accessible](https://github.com/RustPython/RustPython/blob/3a5716da2ac26ebebe40a1de2b9bfd63b0508519/compiler/ast/src/ast_gen.rs#L49-L71):

```rust
pub enum StmtKind<U = ()> {
    FunctionDef {
        name: Ident,
        args: Box<Arguments<U>>,
        body: Vec<Stmt<U>>,
        decorator_list: Vec<Expr<U>>,
        returns: Option<Box<Expr<U>>>,
        type_comment: Option<String>,
    },
    // ...
    ClassDef {
        name: Ident,
        bases: Vec<Expr<U>>,
        keywords: Vec<Keyword<U>>,
        body: Vec<Stmt<U>>,
        decorator_list: Vec<Expr<U>>,
    },
```

(and `Ident` is just `String` as per `type Ident = String;` defined earlier)

Note that there are several more such checks which should really be using the names as their ranges e.g:

* E742 (AmbiguousClassName)
* E743 (AmbiguousFunctionName)

---

_Comment by @charliermarsh on 2022-12-30 12:20_

We actually have a dedicated method for this (`identifier_range`), it's just not being used for those checks (which is an oversight).

---

_Assigned to @charliermarsh by @charliermarsh on 2022-12-30 12:23_

---

_Closed by @charliermarsh on 2022-12-30 12:39_

---

_Comment by @charliermarsh on 2022-12-30 12:39_

I fixed a bunch of these but please let me know if you run into others.

---

_Comment by @not-my-profile on 2022-12-31 07:19_

D417 (Missing argument descriptions in the docstring) also highlights the whole function ... ideally it would only highlight the docstring.

---

_Comment by @balsa-sarenac on 2023-02-27 17:16_

@charliermarsh  PLR0913 (too-many-arguments) also highlights the whole function, it would be nice to only have it highlight the function name. Not sure if this was worth opening a new issue, so I've put it here.

---

_Comment by @charliermarsh on 2023-02-27 17:18_

Thanks, will fix!

---

_Comment by @balsa-sarenac on 2023-04-12 15:12_

@charliermarsh PT004 also highlights the whole function, it would be nice if it would highlight only the function name.

---

_Comment by @charliermarsh on 2023-04-12 15:28_

Thanks, fixed :)

---

_Comment by @balsa-sarenac on 2023-04-12 15:31_

Thank you for the quick fixes! 🙇🏻 

---
