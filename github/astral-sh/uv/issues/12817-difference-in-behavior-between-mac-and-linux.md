---
number: 12817
title: Difference in behavior between mac and linux
type: issue
state: closed
author: natemcintosh
labels:
  - bug
assignees: []
created_at: 2025-04-10T18:08:21Z
updated_at: 2025-04-10T18:15:39Z
url: https://github.com/astral-sh/uv/issues/12817
synced_at: 2026-01-07T13:12:18-06:00
---

# Difference in behavior between mac and linux

---

_Issue opened by @natemcintosh on 2025-04-10 18:08_

### Summary

When asking for the help message from a script, I get two different responses on mac and linux.

```sh
# Make a new app
mkdir diff-behaviors
cd diff-behaviors
uv init --app .
```
Then in `main.py`, paste the following:
```python
# main.py
if __name__ == "__main__":
    from argparse import ArgumentParser

    parser = ArgumentParser(description="A simple command-line tool.")
    parser.add_argument("--greet", type=str, help="Greet a person by name.")
    args = parser.parse_args()
    if args.greet:
        print(f"Hello, {args.greet}!")
    else:
        print("Hello, World!")

```
- Running `uv run main.py` works as expected
- Running `uv run main.py --greet Nate` works as expected
- Running `uv run main.py -h` (or `uv run main.py --help`) **produces different results**.

On two Linux machines I have access to
```sh
$ uv run main.py -h
Run a command or script

Usage:
  > uv run {flags} ...(args) 

Flags:
  --extra <string>: Include optional dependencies from the specified extra name
  --all-extras: Include all optional dependencies
  --no-extra <string>: Exclude the specified optional dependencies, if `--all-extras` is supplied
  # And many more lines after this
```
But running on my mac, I get
```sh
$ uv run main.py -h
usage: main.py [-h] [--greet GREET]

A simple command line tool.

options:
  -h, --help    show this help message and exit
  --greet GREET Name of the person to greet.
```


### Platform

Darwin 24.4.0 arm64 and Linux 6.13.9-200.fc41.x86_64 x86_64 GNU/Linux

### Version

uv 0.6.14

### Python version

Python 3.13.3

---

_Label `bug` added by @natemcintosh on 2025-04-10 18:08_

---

_Comment by @natemcintosh on 2025-04-10 18:15_

Actually, I've just discovered that this may be a [nushell](https://www.nushell.sh/) issue. Running the above, but in bash, produces the expected results.

Interestingly, while I created an analogous test with poetry, which produced the same results, except that on mac, it now does not work as expected.

---

_Closed by @natemcintosh on 2025-04-10 18:15_

---

_Referenced in [nushell/nushell#15545](../../nushell/nushell/issues/15545.md) on 2025-04-10 18:39_

---
