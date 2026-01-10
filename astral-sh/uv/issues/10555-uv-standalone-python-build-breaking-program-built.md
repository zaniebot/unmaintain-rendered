```yaml
number: 10555
title: uv Standalone Python Build Breaking Program Built with Nuitka
type: issue
state: closed
author: nazdridoy
labels: []
assignees: []
created_at: 2025-01-13T07:20:44Z
updated_at: 2025-09-30T13:29:40Z
url: https://github.com/astral-sh/uv/issues/10555
synced_at: 2026-01-10T03:23:53Z
```

# uv Standalone Python Build Breaking Program Built with Nuitka

---

_Issue opened by @nazdridoy on 2025-01-13 07:20_



Issue with Nuitka Standalone Build Using `uv` Virtual Environment on Python 3.13.1

**Description:**

I am encountering issues when building a standalone executable using Nuitka within a virtual environment created by `uv` for Python 3.13.1. The same process works fine with the system Python but fails when using the `uv` standalone build.

**Steps to Reproduce:**

1. Initialize a project and virtual environment with `uv` using system Python:
    ```bash
    uv init
    uv venv --python /usr/bin/python
    uv add pygame nuitka
    env LDFLAGS="-L/path/to/libatomic.a -latomic" uv run python -m nuitka --standalone snake.py
    ```
    <details>
	<summary><strong>Nuitka Build Log</strong></summary>

    ```bash
	Nuitka-Options: Used command line options: --standalone snake.py
	Nuitka-Options: Detected static libpython to exist, consider '--static-libpython=yes' for better performance, but errors may happen.
	Nuitka: Starting Python compilation with Nuitka '2.5.9' on Python '3.13' commercial grade 'not installed'.
	Nuitka-Plugins:anti-bloat: Not including '_json' automatically in order to avoid bloat, but this may cause: may slow down by using fallback
	Nuitka-Plugins:anti-bloat: implementation.
	Nuitka-Plugins:anti-bloat: Not including '_bisect' automatically in order to avoid bloat, but this may cause: may slow down by using fallback
	Nuitka-Plugins:anti-bloat: implementation.
	Nuitka: Completed Python level compilation and optimization.
	Nuitka: Generating source code for C backend compiler.
	Nuitka: Running data composer tool for optimal constant value handling.              
	Nuitka: Running C compilation via Scons.
	Nuitka-Scons: Backend C compiler: gcc (gcc 14.2.1).
	Nuitka-Scons: Scons: Inherited LDFLAGS='-L/path/to/libatomic.a -latomic' variable.
	Nuitka-Scons: Backend linking program with 30 files (no progress information available for this stage).
	Nuitka-Scons:WARNING: You are not using ccache, re-compilation of identical code will be slower than necessary. Use your OS package manager to
	Nuitka-Scons:WARNING: install it.
	Nuitka-Plugins:data-files: Included data file 'pygame/freesansbold.ttf' due to package data for 'pygame'.
	Nuitka-Plugins:data-files: Included data file 'pygame/pygame_icon.bmp' due to package data for 'pygame'.
	Nuitka: Keeping build directory 'snake.build'.
	Nuitka: Successfully created 'snake.dist/snake.bin'.
	❯ snake.dist/snake.bin
	pygame 2.6.1 (SDL 2.28.4, Python 3.13.1)
	Hello from the pygame community. https://www.pygame.org/contribute.html
	Nuitka: A segmentation fault has occurred. This is highly unusual and can
	have multiple reasons. Please check https://nuitka.net/info/segfault.html
	for solutions.
    ````

    </details>
 
2. The above works fine, but when using `uv` standalone Python build:
    ```bash
    uv init
    uv venv --python 3.13.1
    uv add pygame nuitka
    env LDFLAGS="-L/path/to/libatomic.a -latomic" uv run python -m nuitka --standalone snake.py
    ```
	<details>
	<summary><strong>Nuitka Build Log</strong></summary>

    ````
        Nuitka-Options: Used command line options: --standalone snake.py
	Nuitka: Starting Python compilation with Nuitka '2.5.9' on Python '3.13' commercial grade 'not installed'.
	Nuitka: Completed Python level compilation and optimization.                            
	Nuitka: Generating source code for C backend compiler.
	Nuitka: Running data composer tool for optimal constant value handling.              
	Nuitka: Running C compilation via Scons.
	Nuitka-Scons: Backend C compiler: gcc (gcc 14.2.1).
	Nuitka-Scons: Scons: Inherited LDFLAGS='-L/path/to/libatomic.a -latomic' variable.
	Nuitka-Scons: Backend linking program with 30 files (no progress information available for this stage).
	Nuitka-Scons:WARNING: You are not using ccache, re-compilation of identical code will be slower than necessary. Use your OS package manager to
	Nuitka-Scons:WARNING: install it.
	Nuitka-Plugins:data-files: Included data file 'pygame/freesansbold.ttf' due to package data for 'pygame'.
	Nuitka-Plugins:data-files: Included data file 'pygame/pygame_icon.bmp' due to package data for 'pygame'.
	Nuitka: Keeping build directory 'snake.build'.
	Nuitka: Successfully created 'snake.dist/snake.bin'.
	❯ snake.dist/snake.bin
	Traceback (most recent call last):
	  File "/home/user/USER/Workspace/TEMP/miscProjects/snake/snake.dist/inspect.py", line 146, in <module>
	  File "/home/user/USER/Workspace/TEMP/miscProjects/snake/snake.dist/dis.py", line 8, in <module>
	  File "/home/user/USER/Workspace/TEMP/miscProjects/snake/snake.dist/opcode.py", line 12, in <module>
	ModuleNotFoundError: No module named '_opcode'
    ```

    </details>
    
    Running the generated binary results in:
    ```
    Traceback (most recent call last):
      File "/path/to/snake/snake.dist/inspect.py", line 146, in <module>
      File "/path/to/snake/snake.dist/dis.py", line 8, in <module>
      File "/path/to/snake/snake.dist/opcode.py", line 12, in <module>
    ModuleNotFoundError: No module named '_opcode'
    ```

**Additional Information:**

- Operating System: Arch Linux
- Python Version: 3.13.1 (system and `uv` standalone build)
- Nuitka Version: uv 0.5.18 (27d1bad55 2025-01-11)
- Issue seems similar to [Nuitka issue #3111](https://github.com/Nuitka/Nuitka/issues/3111#issuecomment-2536438277)

Please let me know if additional information or logs are required. 


---

_Comment by @zanieb on 2025-01-13 20:33_

Where is `_opcode` supposed to come from? Unfortunately, I don't have time to dig into Nuitka's build system so we'll need help diagnosing the cause of the difference.

---

_Comment by @nazdridoy on 2025-01-13 20:57_

_opcode module is a built-in module in Python.

maybe @kayhayen can help us understand the core issue. he is probably familiar with similar issues

---

_Comment by @kayhayen on 2025-01-15 13:05_

Feel free to make a proper report in Nuitka issue tracker, I cannot guess the details. Your Python needing extra LDFLAGS is not a good sign of it being a supported flavor, but I will always be open to adding more.

---

_Comment by @nazdridoy on 2025-01-15 13:20_


@kayhayen

In Arch Linux, libatomic.a (static libraries) is not available. 
This is why I had to use LDFLAGS to make Nuitka work in both cases

```bash
LDFLAGS="-L/path/to/libatomic.a -latomic"
```

anyway, I've mentioned you here to know if you have any comment on why the pre-built Python binary from uv doesn't work with Nuitka.


---

_Comment by @kayhayen on 2025-01-15 13:59_

I see, if there will be a report in Nuitka, it will be dealt with, for now I don't really want to guess about steps to reproduce, etc. so please fill out the issue template and lets handle it there, I don't see this being an uv issue or myself tracking this outside of Nuitka tracker.

---

_Comment by @zanieb on 2025-01-15 14:53_

I think standard Python distributions probably dynamically link libatomic so it would automatically be included in your LDFLAGS but python-build-standalone does not.

Thanks for the replies @kayhayen! Ping me if you think I can help.

---

_Comment by @WilliamStam on 2025-09-30 09:45_

im having this issue now on red hat. 

```
Backend C:  94% 1424/1520 [19:47<00:19,  4.88file/s]
Backend C:  94% 1425/1520 [19:47<00:24,  3.91file/s]
Backend C:  94% 1426/1520 [19:48<00:35,  2.62file/s]
Backend C:  94% 1427/1520 [19:48<00:29,  3.18file/s]
Backend C:  94% 1428/1520 [19:49<00:28,  3.27file/s]
Backend C:  94% 1429/1520 [19:49<00:2
/usr/bin/ld: cannot find -l:libatomic.a
/usr/bin/ld: cannot find -l:libatomic.a
collect2: error: ld returned 1 exit status
scons: *** [/home/munsys_william__bk/.cache/act/877863619d5228fc/hostexecutor/build/main.dist/argus-redhat.bin] Error 1
FATAL: Failed unexpectedly in Scons C backend compilation.
Nuitka:WARNING:     Complex topic! More information can be found at https://nuitka.net/info/scons-backend-failure.html
Nuitka-Reports: Compilation crash report written to file 'nuitka-crash-report.xml'.
  ❌  Failure - Main Build with nuitka
exit status 1
```

(this is run through a gitea actions but it fails regardless)

for those that get this issue after my reading you can install libatomic `sudo dnf install libatomic` and then set 

```
env LDFLAGS="-L/usr/lib/gcc/x86_64-redhat-linux/8/32/libatomic.a -latomic" uv run python -m nuitka src/main.py
```

maybe your results will be better than mine

```
name: Build main on redhat
run-name: Building
on:
  push:
    branches:
      - wip

jobs:
  Build:
    runs-on: redhat
    steps:
      - name: Checkout Codebase
        uses: actions/checkout@v5

      - name: Meta
        id: meta
        run: |
          echo REPO_NAME=$(echo ${GITHUB_REPOSITORY} | awk -F"/" '{print $2}') >> $GITHUB_OUTPUT
          echo REPO_VERSION=$(git describe --tags --always | sed 's/^v//') >> $GITHUB_OUTPUT      
          echo PACKAGE_VERSION=$(uvx --from=toml-cli toml get --toml-path=pyproject.toml project.version) >> $GITHUB_OUTPUT      
          echo PACKAGE_NAME=$(uvx --from=toml-cli toml get --toml-path=pyproject.toml project.name) >> $GITHUB_OUTPUT      

      - name: Testing
        run: |
          echo "REPO_NAME: ${{ steps.meta.outputs.REPO_NAME }}"
          echo "REPO_VERSION: ${{ steps.meta.outputs.REPO_VERSION }}"
          echo "PACKAGE_VERSION: ${{ steps.meta.outputs.PACKAGE_VERSION }}"
          echo "PACKAGE_NAME: ${{ steps.meta.outputs.PACKAGE_NAME }}"


      - name: Install uv
        uses: astral-sh/setup-uv@v6

      - name: Set up Python
        run: uv python install


      - name: Install the project
        run: uv sync --locked --all-extras --dev

      - name: Build with nuitka
        run: |
          env LDFLAGS="-L/usr/lib/gcc/x86_64-redhat-linux/8/32/libatomic.a -latomic" uv run python -m nuitka src/main.py \
            --standalone \
            --onefile \
            --msvc=latest \
            --windows-icon-from-ico=src/static/favicon.ico \
            --include-module=app \
            --include-module=passlib \
            --include-module=passlib.handlers.pbkdf2 \
            --output-filename=argus-redhat \
            --output-dir=build \
            --include-data-dir=src/static/=static/ \
            --include-data-dir=src/templates/=templates/
          
      - name: Release
        uses: akkuman/gitea-release-action@v1
        with:
          files: |-
            ./build/**
          name: '${{ steps.meta.outputs.PACKAGE_VERSION }}'
          tag_name: '${{ steps.meta.outputs.PACKAGE_VERSION }}'
```




---

_Comment by @zanieb on 2025-09-30 13:29_

Let's track this in https://github.com/astral-sh/python-build-standalone/issues/616 instead — it links to a couple Nuitka issues and explains the cause of the problem.

---

_Closed by @zanieb on 2025-09-30 13:29_

---
