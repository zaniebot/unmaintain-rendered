```yaml
number: 1588
title: Add autofix for SIM300
type: pull_request
state: merged
author: PedramNavid
labels: []
assignees: []
merged: true
base: main
head: SIM300-autofix
created_at: 2023-01-03T07:42:58Z
updated_at: 2023-01-03T12:19:09Z
url: https://github.com/astral-sh/ruff/pull/1588
synced_at: 2026-01-12T15:55:06Z
```

# Add autofix for SIM300

---

_@PedramNavid_

I think this is right! I ran it locally on the test cases and it worked, but admittedly I wasn't really sure what I was doing when creating an Expr instance or even the values I used for SourceCodeGenerator.

Here's an example of it working:

```python
> cat test.py
...
# Line 9
42 == x
'foo' == yoda
print('foo' == yoda)
print(42 == x)

def main():
    print("hello world")
    print('foo' == yoda)
```

One thing I can't figure out is why it doesn't show here that there are fixes possible. I'm expecting something like 
`4 potentially fixable with the --fix option.`

```
cargo run test.py --no-cache --select SIM300
    Finished dev [unoptimized + debuginfo] target(s) in 0.13s
     Running `target/debug/ruff test.py --no-cache --select SIM300`
test.py:9:1: SIM300 Use `42 == x` instead of `x == 42 (Yoda-conditions)`
test.py:10:1: SIM300 Use `'foo' == yoda` instead of `yoda == 'foo' (Yoda-conditions)`
test.py:11:7: SIM300 Use `'foo' == yoda` instead of `yoda == 'foo' (Yoda-conditions)`
test.py:12:7: SIM300 Use `42 == x` instead of `x == 42 (Yoda-conditions)`
test.py:16:11: SIM300 Use `'foo' == yoda` instead of `yoda == 'foo' (Yoda-conditions)`
Found 5 error(s).
```

but when I run this
```
cargo run test.py --no-cache --select SIM300 --fix
   
Finished dev [unoptimized + debuginfo] target(s) in 0.11s
     Running `target/debug/ruff test.py --no-cache --select SIM300 --fix`
Found 5 error(s) (5 fixed, 0 remaining).
```

It does fix the file. 

```
...
# Line 9
x == 42
yoda == 'foo'
print(yoda == 'foo')
print(x == 42)

def main():
    print("hello world")
    print(yoda == 'foo')
```



---

_Review comment by @charliermarsh on `src/registry.rs`:3597 on 2023-01-03 12:18_

@PedramNavid - This was the missing piece. There's a separate `fixable` "list" that marks a check as fixable in the README. (Autofix will still "work" without this, but it won't appear in the README, on the CLI, etc.)

---

_@charliermarsh reviewed on 2023-01-03 12:18_

---

_Merged by @charliermarsh on 2023-01-03 12:19_

---

_Closed by @charliermarsh on 2023-01-03 12:19_

---

_Comment by @charliermarsh on 2023-01-03 12:19_

Thanks Pedram :)

---
