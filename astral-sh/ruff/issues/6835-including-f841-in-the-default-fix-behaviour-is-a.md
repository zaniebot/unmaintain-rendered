```yaml
number: 6835
title: Including F841 in the default --fix behaviour is a gigantic footgun
type: issue
state: closed
author: ssokolow
labels:
  - fixes
  - cli
assignees: []
created_at: 2023-08-24T04:23:03Z
updated_at: 2023-08-24T21:32:18Z
url: https://github.com/astral-sh/ruff/issues/6835
synced_at: 2026-01-10T11:09:49Z
```

# Including F841 in the default --fix behaviour is a gigantic footgun

---

_Issue opened by @ssokolow on 2023-08-24 04:23_

I have a habit of writing my code from the top down and saving part-way through to trigger things like MyPy at a time when they won't be distracting. After installing Ruff and integrating it into my Vim+ALE-based setup that triggers everything (including autofix/autoformat) on save (thank you, rustfmt, for bringing my habits into the 21st century), I started getting confusing errors.

After a couple of times, I realized I wasn't at risk of dementia, but that Ruff was deleting assignments that I hadn't used **yet**... sometimes at the worst possible time for derailing my memory of how I planned things to work.

Beyond that, while it's very much a "do not rely on this!" thing in the Python language spec, because, in practice, CPython only uses garbage collection to break cycles and uses reference counting for non-cyclical data structures, dropping memory *does* have Rust-style determinism in CPython,  it *does* alter the drop order in the same way it would in Rust, and [Hyrum's law](https://www.hyrumslaw.com/) *does* apply...

...which means the fixer can alter the semantics of the program in ways other than "pulled the rug out from under the train of thought". Take a look at how ruff alters the observed behaviour of this code:

```python
class Foo:
    def __init__(self, msg):
        self.msg = msg
    def __del__(self):
        print(self.msg)

def main():
    print("Yo Ho Ho")
    ham = Foo("bottle of rum")
    print("and a")

main()
```

```sh
ssokolow@monolith ~ % python3 demo.py
Yo Ho Ho
and a
bottle of rum
ssokolow@monolith ~ % ruff --fix demo.py
Found 1 error (1 fixed, 0 remaining).
ssokolow@monolith ~ % python3 demo.py   
Yo Ho Ho
bottle of rum
and a
```

I think that, if `--fix` support is offered for F841, it shouldn't be part of the set of fixes that's enabled by default when a new user (or their IDE/editor integration plugin) just blindly runs `ruff --fix`.

---

_Comment by @charliermarsh on 2023-08-24 13:58_

I think this will be solved once we respect suggested fixes on the CLI, since these are already "suggested" and not "automatic" fixes \cc @zanieb 

---

_Label `autofix` added by @charliermarsh on 2023-08-24 13:58_

---

_Label `cli` added by @charliermarsh on 2023-08-24 13:58_

---

_Comment by @charliermarsh on 2023-08-24 13:59_

I'm going to merge with https://github.com/astral-sh/ruff/issues/4185 for that reason.

---

_Closed by @charliermarsh on 2023-08-24 13:59_

---

_Comment by @charliermarsh on 2023-08-24 13:59_

Thank you for reporting and sorry that it's been inconvenient for you. If you want, you _can_ turn this off by marking it as [`unfixable`](https://beta.ruff.rs/docs/settings/#unfixable).

---

_Comment by @ssokolow on 2023-08-24 14:12_

Thanks.

I'll probably want to look up how to get ALE to specify `--unfixable` instead. I can't be dependant on PyQt or Django in *all* my projects and, as Rust's ecosystem matures and the allure of strong compile-time correctness beckons, I continue to push Python+MyPy further and further into the "throwaway single-file script with no project folder" niche.

---

_Comment by @ssokolow on 2023-08-24 21:32_

For the record, it's `let g:ale_python_ruff_options = '--unfixable=F841'` in your `.vimrc`.

---
