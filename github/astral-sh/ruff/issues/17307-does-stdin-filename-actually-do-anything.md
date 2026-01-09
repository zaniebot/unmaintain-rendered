---
number: 17307
title: "Does `--stdin-filename` actually do anything?"
type: issue
state: closed
author: fosskers
labels:
  - question
assignees: []
created_at: 2025-04-09T09:27:12Z
updated_at: 2025-04-28T08:32:20Z
url: https://github.com/astral-sh/ruff/issues/17307
synced_at: 2026-01-07T13:12:16-06:00
---

# Does `--stdin-filename` actually do anything?

---

_Issue opened by @fosskers on 2025-04-09 09:27_

### Question

In configuring `ruff` for use in my editor (Emacs w/ apheleia), I've noticed that specifying the command as follows works without issue:
```
ruff format --stdin-filename foo --line-length=120
```
The file to be saved is piped in through stdin, and it doesn't seem to matter what I set `--stdin-filename` to, it can be anything, and the reformatting still works. If that's the case, what is that flag for? Note that without it, `ruff` ignores stdin and just formats the whole directory.

### Version

0.11.4

---

_Label `question` added by @fosskers on 2025-04-09 09:27_

---

_Comment by @sharkdp on 2025-04-09 10:11_

> and it doesn't seem to matter what I set `--stdin-filename` to, it can be anything, and the reformatting still works. If that's the case, what is that flag for?

It is used internally as a placeholder for the "file" that Ruff reads from STDIN. For example, if there would be an error, you might see that filename being used:
```
▶ echo "x=" | ruff format --stdin-filename foo.py
error: Failed to parse foo.py:1:3: Expected an expression
```

Setting it to the filename that you're piping in from your editor can be useful because it will generate correct references (and potentially clickable links, depending on your terminal) in those diagnostic messages.

---

_Comment by @MichaReiser on 2025-04-09 10:19_

The filename is also used to infer the file type. E.g. a file ending in `.ipynb` is treated as a notebook and a file ending in `.pyi` is a stub file. Different rules apply to those files.

---

_Comment by @fosskers on 2025-04-09 11:00_

Is there a world where piping to stdin "just works" without having to specify that flag? This behaviour confused me for quite some time today.

---

_Comment by @MichaReiser on 2025-04-09 11:20_

I think it should work if you use `ruff format -` which also enables piping. Calling `ruff format` without `-` or `--stdin-filename` doesn't enable stdin mode. Is there a reliable way to detect whether a program is piping input?

---

_Comment by @fosskers on 2025-04-09 12:36_

A lot of the classic Unix tools seem to, so there must be a way.

---

_Comment by @Skylion007 on 2025-04-09 15:30_

We can detect if the stdin input to ruff is connected to a TTY with [rust's stdlib](https://doc.rust-lang.org/beta/std/io/trait.IsTerminal.html):
```rust
use std::io::{self, IsTerminal, Read};

fn main() {
    let stdin = io::stdin();

    if stdin.is_terminal() {
        println!("No piped input detected. Stdin is from a terminal.");
    } else {
        println!("Piped input detected. Reading from stdin...");

        let mut input = String::new();
        stdin.read_to_string(&mut input).unwrap();
        println!("Received input:\n{}", input);
    }
}
```
or via atty crate on older rust versions, I think our min rust version is high enough though. Bonus: std's version should work on Windows too (although it uses heuristics to determine which mode it is running in there).

---

_Comment by @sharkdp on 2025-04-09 15:40_

No, I don't think atty checks are relevant here. They can be used to check whether stdin is attached to a *interactive* terminal or not. But they can't be used to check whether "the user intends to pass something on stdin". The only implicit thing that you could check is whether or not stdin is open or closed. But it is extremely rare for stdin to be closed. You would basically have to do something like `ruff … < /dev/null`. So that can't be used as a heuristic.

So the only way to potentially support this would be to *explicitly* say that the "no positional argument" call of "ruff check" should read from stdin. But I don't think that is an option. The "no positional argument" call currently has a (arguably) much more useful default, which is to check the current directory.

So I don't think there's anything that can be done here. Users will need to pass `-` explicitly if they are in the (rare) use case where they want to pass something on stdin.

> A lot of the classic Unix tools seem to, so there must be a way.

Sure, `cat` reads from stdin by default if you don't pass an argument. But there's no other reasonable default behavior for `cat`.

---

_Comment by @Skylion007 on 2025-04-09 15:47_

Is there a case where stdin is open on a non-interactive session and user doesn't intend to pass input though? Or at least we could throw a warning if users have an open stdin in a non-interactive input AND files specified? Is there a valid situation where use wants that?

IE. We should warn maybe if user have files specified in a non-interactive terminal and is piping to std input?

Issue seems to be that either files should be specified, or stdin should be open, but not both.

---

_Comment by @sharkdp on 2025-04-09 16:21_

> Is there a case where stdin is open on a non-interactive session and user doesn't intend to pass input though?

That's what I tried to explain. stdin is almost always open, unless you close it explicitly. And it should be! You can do `cat 1.txt - 3.txt > combined.txt`, and `cat` will read from those two files, but also read from stdin. Or a program could suddenly decide to ask you to enter something in an interactive prompt.

So when you do `ruff check path/to/explicit/file.py`, stdin is open. There's nothing preventing `ruff` from also reading from stdin in that case.

> Or at least we could throw a warning if users have an open stdin in a non-interactive input AND files specified?

No, that would be a terrible UX.

> IE. We should warn maybe if user have files specified in a non-interactive terminal and is piping to std input?

I don't think that would be a good idea. You might run `ruff check` from a script or some other environment where stdin is not a TTY. It shouldn't behave different in those cases.


---

_Closed by @MichaReiser on 2025-04-28 08:32_

---

_Referenced in [astral-sh/ruff#20460](../../astral-sh/ruff/issues/20460.md) on 2025-09-18 03:26_

---
