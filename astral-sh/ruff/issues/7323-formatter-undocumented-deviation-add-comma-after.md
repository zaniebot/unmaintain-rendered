```yaml
number: 7323
title: "Formatter undocumented deviation: add comma after single argument definition"
type: issue
state: closed
author: JonathanPlasse
labels:
  - documentation
  - formatter
assignees: []
created_at: 2023-09-12T21:20:58Z
updated_at: 2023-09-29T17:27:35Z
url: https://github.com/astral-sh/ruff/issues/7323
synced_at: 2026-01-10T11:09:49Z
```

# Formatter undocumented deviation: add comma after single argument definition

---

_Issue opened by @JonathanPlasse on 2023-09-12 21:20_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Black formatting
```python
def main(
    pickle_path: Path = typer.Argument(
        "network_messages.pickle",
        help="The path of the pickle file that will contain the network messages",
    )
) -> None:
    start_async(launch_network_watcher, pickle_path)
```
Ruff formatting
```python
def main(
    pickle_path: Path = typer.Argument(
        "network_messages.pickle",
        help="The path of the pickle file that will contain the network messages",
    ),
) -> None:
    start_async(launch_network_watcher, pickle_path)
```
Ruff 0.0.289 with line-length 100

---

_Label `formatter` added by @MichaReiser on 2023-09-13 06:01_

---

_Label `needs-decision` added by @MichaReiser on 2023-09-13 06:01_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-09-13 06:01_

---

_Comment by @MichaReiser on 2023-09-13 06:02_

Interesting find. Black does add a trailing comma if the identifier doesn't fit but not when the type annotation or default value expands over multiple lines.

```python
def main(
    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa,
) -> None:
    start_async(launch_network_watcher, pickle_path)


def main(
    pickle_path: Argument(
        "network_messages.pickle",
        help="The path of the pickle file that will contain the network messages",
    )
) -> None:
    start_async(launch_network_watcher, pickle_path)
```

This feels a bit inconsistent to me. We'll have to narrow down if this is intentional or not.

---

_Comment by @charliermarsh on 2023-09-13 16:16_

I ran into this at one point and I believe I came to the conclusion that it was unintentional and that the inconsistencies were hard to understand.

---

_Comment by @charliermarsh on 2023-09-13 16:16_

(I prefer the consistency of our formatting here, personally.)

---

_Comment by @charliermarsh on 2023-09-13 23:03_

I find the inconsistency here confusing:

```python
def main(
    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa,
) -> None:
    start_async(launch_network_watcher, pickle_path)


def main(
    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa=1,
) -> None:
    start_async(launch_network_watcher, pickle_path)


def main(
    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa: Argument(
        "network_messages.pickle",
        help="The path of the pickle file that will contain the network messages",
    ) = 1
) -> None:
    start_async(launch_network_watcher, pickle_path)
```

I would vote to leave as-is.

---

_Label `needs-decision` removed by @MichaReiser on 2023-09-14 06:24_

---

_Label `wontfix` added by @MichaReiser on 2023-09-14 06:25_

---

_Comment by @charliermarsh on 2023-09-27 15:11_

We're marking this as an intentional deviation. Can be closed once it's documented.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-27 18:45_

---

_Label `wontfix` removed by @MichaReiser on 2023-09-28 09:44_

---

_Label `documentation` added by @MichaReiser on 2023-09-28 09:44_

---

_Closed by @charliermarsh on 2023-09-29 17:27_

---

_Closed by @charliermarsh on 2023-09-29 17:27_

---
