```yaml
number: 3717
title: Allow relative Python executable paths in Windows trampoline
type: pull_request
state: merged
author: ofek
labels: []
assignees: []
merged: true
base: main
head: relative-trampoline
created_at: 2024-05-21T19:11:52Z
updated_at: 2024-05-22T02:32:43Z
url: https://github.com/astral-sh/uv/pull/3717
synced_at: 2026-01-10T14:32:20Z
```

# Allow relative Python executable paths in Windows trampoline

---

_Pull request opened by @ofek on 2024-05-21 19:11_

## Summary

This is a prerequisite for https://github.com/astral-sh/uv/issues/3669

## Test Plan

Download one of the standalone distributions on Windows then use its Python to run the following script and then run the scripts it creates (only pip):

```python
from __future__ import annotations

import sys
import sysconfig
from contextlib import closing
from importlib.metadata import entry_points
from io import BytesIO
from os.path import relpath
from pathlib import Path
from tempfile import TemporaryDirectory
from zipfile import ZIP_DEFLATED, ZipFile, ZipInfo

# Change this line to your real path
LAUNCHERS_DIR = Path('C:\\Users\\ofek\\Desktop\\code\\uv\\crates\\uv-trampoline\\target\\x86_64-pc-windows-msvc\\release')
SCRIPT_TEMPLATE = """\
#!{executable}
# -*- coding: utf-8 -*-
import re
import sys
from {module} import {import_name}
if __name__ == "__main__":
    sys.argv[0] = re.sub(r"(-script\\.pyw|\\.exe)?$", "", sys.argv[0])
    sys.exit({function}())
"""


def select_entry_points(ep, group):
    return ep.select(group=group) if sys.version_info[:2] >= (3, 10) else ep.get(group, [])


def main():
    interpreters_dir = Path(sys.executable).parent
    scripts_dir = Path(sysconfig.get_path('scripts'))

    ep = entry_points()
    for group, interpreter_name, launcher_name in (
        ('console_scripts', 'python.exe', 'uv-trampoline-console.exe'),
        ('gui_scripts', 'pythonw.exe', 'uv-trampoline-gui.exe'),
    ):
        interpreter = interpreters_dir / interpreter_name
        relative_interpreter_path = relpath(interpreter, scripts_dir)
        launcher_data = (LAUNCHERS_DIR / launcher_name).read_bytes()

        for script in select_entry_points(ep, group):
            # https://github.com/astral-sh/uv/tree/main/crates/uv-trampoline#how-do-you-use-it
            with closing(BytesIO()) as buf:
                # Launcher
                buf.write(launcher_data)

                # Zipped script
                with TemporaryDirectory() as td:
                    zip_path = Path(td) / 'script.zip'
                    with ZipFile(zip_path, 'w') as zf:
                        # Ensure reproducibility
                        zip_info = ZipInfo('__main__.py', (2020, 2, 2, 0, 0, 0))
                        zip_info.external_attr = (0o644 & 0xFFFF) << 16

                        module, _, attrs = script.value.partition(':')
                        contents = SCRIPT_TEMPLATE.format(
                            executable=relative_interpreter_path,
                            module=module,
                            import_name=attrs.split('.')[0],
                            function=attrs
                        )
                        zf.writestr(zip_info, contents, compress_type=ZIP_DEFLATED)

                    buf.write(zip_path.read_bytes())

                # Interpreter path
                interpreter_path = relative_interpreter_path.encode('utf-8')
                buf.write(interpreter_path)

                # Interpreter path length
                interpreter_path_length = len(interpreter_path).to_bytes(4, 'little')
                buf.write(interpreter_path_length)

                # Magic number
                buf.write(b'UVUV')

                script_data = buf.getvalue()

            script_path = scripts_dir / f'{script.name}.exe'
            script_path.write_bytes(script_data)


if __name__ == '__main__':
    main()
```

---

_Assigned to @konstin by @zanieb on 2024-05-21 19:15_

---

_Review comment by @charliermarsh on `crates/uv-trampoline/src/bounce.rs`:278 on 2024-05-21 19:17_

Is there no `has_root` or similar that we can use here? There are lots of relative paths that won't necessarily start with this.

---

_@charliermarsh reviewed on 2024-05-21 19:17_

---

_@charliermarsh reviewed on 2024-05-21 19:17_

Can you share the bigger picture on how you plan to use or integrate this?

---

_@ofek reviewed on 2024-05-21 19:18_

---

_Review comment by @ofek on `crates/uv-trampoline/src/bounce.rs`:278 on 2024-05-21 19:18_

I've never done work without the standard library so I'm not sure what is possible.

---

_Comment by @ofek on 2024-05-21 19:19_

Sure, basically the script I put in the description I will use to finalize an entire Python installation until the mentioned feature request is implemented and I can install packages with that flag instead of running that script as a postprocessing step.

---

_Comment by @ofek on 2024-05-21 23:11_

Here is an example showing that it works:

- https://github.com/pypa/hatch/actions/runs/9182650158/job/25251925087#step:6:17
- https://github.com/pypa/hatch/actions/runs/9182650158/job/25251995468

---

_Review requested from @charliermarsh by @charliermarsh on 2024-05-21 23:25_

---

_Comment by @charliermarsh on 2024-05-21 23:25_

Thanks. I want to play around with the code a bit before merging to see if it can be generalized at all.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-21 23:25_

---

_Comment by @charliermarsh on 2024-05-21 23:48_

I will try to get to it tonight though.

---

_Comment by @charliermarsh on 2024-05-22 00:53_

I tried to invert the condition to make it a bit more general (e.g., shouldn't `./path/to/python` work too?). Would you mind re-testing?

---

_Comment by @ofek on 2024-05-22 00:59_

Just tried locally, it still works!

---

_Comment by @ofek on 2024-05-22 01:11_

- I guess that should work as well but I can't imagine a scenario in which the Python interpreter would be inside the scripts directory other than a virtual environment and I don't see a reason to pre-build one nor do I think that would work in practice. Better to be more correct just in case though 😄 
- Do these get updated automatically after a merge? https://github.com/astral-sh/uv/tree/main/crates/uv-trampoline/trampolines

---

_Comment by @charliermarsh on 2024-05-22 01:11_

I think I have to rebuild them as part of the PR.

---

_Comment by @ofek on 2024-05-22 01:39_

How do you rebuild them, locally?

---

_Comment by @charliermarsh on 2024-05-22 01:41_

I followed the cross compilation instructions in the `uv-trampoline` README. I installed `cargo-xwin`, then ran (e.g.) `cargo +nightly-2024-03-19 xwin build --release --target aarch64-pc-windows-msvc --bin`.

---

_Comment by @charliermarsh on 2024-05-22 01:41_

I'm having trouble verifying them locally though. (I can't get them to fail.)

---

_Comment by @ofek on 2024-05-22 01:42_

Fail how?

---

_Comment by @charliermarsh on 2024-05-22 01:43_

I just want to make sure they're actually being used. So I tried (e.g.) overwriting the x86 trampolies with the ARM trampolines but my commands are still working which seems wrong.

---

_Comment by @ofek on 2024-05-22 01:46_

Are you testing it with this package? https://github.com/astral-sh/uv/blob/0.1.45/crates/install-wheel-rs/src/wheel.rs#L26-L40

---

_Comment by @charliermarsh on 2024-05-22 01:47_

Yeah, although I think I'm getting it to fail as expected now.

---

_Merged by @charliermarsh on 2024-05-22 01:53_

---

_Closed by @charliermarsh on 2024-05-22 01:53_

---

_Comment by @charliermarsh on 2024-05-22 01:54_

For future reference: changing _just_ the trampolines doesn't trigger a rebuild (with `cargo run`), you need to make a change to the crate itself it seems. That's why I wasn't seeing the expected failures.

---

_Comment by @ofek on 2024-05-22 01:55_

Thank you very much!

---

_Comment by @charliermarsh on 2024-05-22 01:57_

Thanks Ofek!

---

_Comment by @charliermarsh on 2024-05-22 02:11_

Improved the README a bit: https://github.com/astral-sh/uv/pull/3731

---

_Branch deleted on 2024-05-22 02:32_

---
