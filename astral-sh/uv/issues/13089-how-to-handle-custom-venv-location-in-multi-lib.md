```yaml
number: 13089
title: "How to handle custom `.venv` location in multi lib repos"
type: issue
state: closed
author: andresliszt
labels:
  - question
assignees: []
created_at: 2025-04-24T13:28:49Z
updated_at: 2025-05-04T12:38:10Z
url: https://github.com/astral-sh/uv/issues/13089
synced_at: 2026-01-12T16:01:19Z
```

# How to handle custom `.venv` location in multi lib repos

---

_@andresliszt_

### Question

Hello Thanks for this amazing project!. 

```
my-project/
├── Makefile
├── py_project/
│   ├── uv.lock
│   └── Makefile
└── rs_project/
    └── Makefile
```

Each `Makefile` has command to build the respective project, and the top level `Makefile` forwards the ref to the inners. I'm having problems in my CI/CD when:

-  using the [set-up uv action](https://github.com/astral-sh/setup-uv), it creates a `.venv` in the root (`my-project`) and what I want is that the venv be at `py_project/.venv`, any `uv` command will create a new `.venv` at the right location (as desired) and I'm getting this warning:
  ```warning: 
`VIRTUAL_ENV=/home/runner/work/my-project/py_project/.venv` does not match the project environment path `.venv` and will be ignored; use `--active` to target the active environment instead
Using CPython 3.10.17
Creating virtual environment at: .venv
```

In there a way to tell uv to work inside `py_project` always? I have tried several things reading the docs but I haven't been able to do it.

This warning can be handled with the --active, but I'm having several problem with other tools that manage their own venvs such as codspeed, when uv recreates a new venv it breaks everything


------------ Command snippets --------------

```Makefile
.DEFAULT_GOAL := help

#-------------------------------------------------
# Root Makefile for my-project
#-------------------------------------------------

.PHONY: help pymoors-% moors-%

# Proxy targets into subdirectories
pymoors-%:  ## Run target in pymoors/Makefile
	@$(MAKE) -C pymoors $*

moors-%:  ## Run target in moors/Makefile
	@$(MAKE) -C moors $*
```

and the py project reduced makefile

```Makefile
.DEFAULT_GOAL := help
SOURCES      = python/pymoors tests

#-------------------------------------------------
# Environment checks & pre‑commit helpers
#-------------------------------------------------
.PHONY: .uv .pre-commit pre-commit-install pre-commit-run
.uv:     ## Check for uv
	@uv -V \
	  || echo 'Please install uv: https://docs.astral.sh/uv/getting-started/installation/'
.pre-commit:   ## Check for pre-commit
	@uv run pre-commit --version \
	  || echo 'Please install pre-commit: https://pre-commit.com/'

pre-commit-install: .pre-commit   ## Install git hooks via pre-commit
	@echo "Installing pre-commit hooks…" \
	  && uv run pre-commit install

pre-commit-run:                   ## Run all enabled hooks against all files
	@echo "Running pre-commit on all files…" \
	  && uv run pre-commit run --all-files

#-------------------------------------------------
# Python/PyO3 (pymoors)
#-------------------------------------------------
.PHONY:  lock build-dev build-release \
        lint-python lint-rust pyright \
        test test-benchmarks test-all \
        coverage docs

lock:   ## Rebuild Python lockfile
	@echo "[pymoors] rebuilding lockfile" \
	  && uv lock --upgrade

build-dev:  ## Build & install in dev mode
	@echo "[pymoors] dev build" \
	  && uv sync --group dev \
	  && uv run maturin develop --uv
```

And using it in the action as

```yaml

    steps:
      - uses: actions/checkout@v4

      - name: Install Rust stable
        uses: dtolnay/rust-toolchain@stable

      - name: Install uv
        uses: astral-sh/setup-uv@v5
        with:
          python-version: ${{ matrix.python-version }}

      - name: Build Pymoors (production)
        run: make pymoors-build-dev
```

This is the PR where I'm working on those changes : https://github.com/andresliszt/pymoors/pull/98




### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @andresliszt on 2025-04-24 13:28_

---

_Comment by @konstin on 2025-04-24 13:42_

CC @eifinger this sounds like setup-uv more than uv.

---

_Comment by @eifinger on 2025-04-24 13:46_

I literally merged https://github.com/astral-sh/setup-uv/pull/381 minutes ago 😆 

I will release v6.0.0 of `setup-uv` shortly which adds the input `working-directory`

EDIT: @andresliszt the current version of `setup-uv` automatically activates a venv when you use the `python-version` input. This won't be the case anymore in v6.0.0. Once it is released your problems should go away.

---

_Comment by @andresliszt on 2025-04-24 13:49_

Oh thank you @eifinger !!! Was doing everything, I was about to post that working directory wasn't workng. Thanks again

---

_Comment by @eifinger on 2025-04-24 14:08_

https://github.com/astral-sh/setup-uv/releases/tag/v6.0.0 is released. Let me know if this solves your errors 😃 

---

_Comment by @andresliszt on 2025-04-25 02:41_

Thank you for the quick turnaround on the new release!

I’m running into a strange issue with `uv run pip install` in my Actions workflow. Here’s a simplified version of my `benchmarks` job:

```yaml
jobs:
  benchmarks:
    name: Run benchmarks
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-python@v3
        with:
          python-version: ${{ env.DEFAULT_PYTHON }}

      - name: Install Rust (stable)
        uses: dtolnay/rust-toolchain@stable

      - name: Install uv
        uses: astral-sh/setup-uv@v6
        with:
          python-version: ${{ env.DEFAULT_PYTHON }}
          enable-cache: true
          working-directory: pymoors

      - name: Build optimized pymoors wheel
        id: build
        uses: PyO3/maturin-action@v1
        with:
          manylinux: auto
          args: --release --out dist --interpreter ${{ env.DEFAULT_PYTHON }}
          rust-toolchain: stable
          working-directory: pymoors

      - name: Capture wheel path
        id: find_wheel
        run: |
          # …debug logic to set $WHEEL_FULL…
          echo "wheel=$WHEEL_FULL" >> $GITHUB_OUTPUT

      - name: Install built wheel & inspect venv
        working-directory: pymoors
        run: |
          echo "🔍 Wheel path: ${{ steps.find_wheel.outputs.wheel }}"
          echo "🏗️ Creating virtual environment"
          uv venv

          echo "📦 Installing the wheel (no build isolation)"
          uv run pip install "${{ steps.find_wheel.outputs.wheel }}" \
            --no-build-isolation \
            --force-reinstall

      - name: Run benchmarks
        uses: CodSpeedHQ/action@v3
        with:
          working-directory: pymoors
          run: make test-benchmarks
        env:
          CODSPEED_RUNNER_MODE: "walltime"
```

**What I’m seeing**  
Despite passing the absolute wheel path, pip falls back to:

```
Building pymoors @ file:///home/runner/work/pymoors/pymoors/pymoors
…  
maturin pep517 build-wheel … --editable  
Permission denied: `/home/runner/work/pymoors/pymoors/pymoors/target/wheels` (os error 13)
```

In other words, pip never installs my wheel; it rebuilds the current directory as an editable package and hits the permission error. **I can confirm that the wheel path is passed well**.

**Temporary workaround**  
Switching to a plain Python install works, but the same permission error resurfaces later in the `Run benchmarks` step:

```yaml
- name: Install wheel via python -m pip
  working-directory: pymoors
  run: |
    echo "🔍 Wheel path: ${{ steps.find_wheel.outputs.wheel }}"
    echo "🏗️ Creating virtual environment"
    uv venv

    echo "⚙️ Bootstrapping pip"
    python -m ensurepip --upgrade
    python -m pip install --upgrade pip

    echo "📦 Installing wheel (no build isolation)"
    python -m pip install -vvv --no-build-isolation --force-reinstall \
      "file://${{ steps.find_wheel.outputs.wheel }}"
```

This installs the wheel correctly, as I said, the next step 

```yaml
  - name: Run benchmarks
    uses: CodSpeedHQ/action@v3
    with:
      working-directory: pymoors
      run: make test-benchmarks
    env:
      CODSPEED_RUNNER_MODE: "walltime"
```
step still triggers the editable build and the same permission error, where

```Makefile
test-benchmarks:  ## Run only benchmarks tests
	@echo "[pymoors] benchmarks only" \
	  && uv sync --group testing \
	  && uv run pytest tests/benchmarks --codspeed
```

---

**Questions**  
1. Is there a way to make `uv run pip install` reliably pick up my pre-built wheel without falling back to a PEP-517 rebuild?  
2. Can I configure `uv` so its pip shim respects `--no-build-isolation`?  
3. Or is there a recommended pattern for installing wheels inside a `uv venv` that avoids this?  


---

_Comment by @eifinger on 2025-04-25 11:31_

Can you try to leave out the `run` part?

```yaml
uv pip install "${{ steps.find_wheel.outputs.wheel }}" \
            --no-build-isolation \
            --force-reinstall
```

---

_Comment by @andresliszt on 2025-04-25 15:26_

@eifinger wow it does the trick `uv pip ...` works in the install wheel step, I'd like to know _why_, I think i'm going to read the docs carefully . The step where I run the benchmarks using the codspeed action is still failing. I think is not recognizing virtual env, because in the logs I can see that it attempts to install everything again.  i'll be digging into it and i'll let you know soon If can fix it.

```yaml
  - name: Run benchmarks
    uses: CodSpeedHQ/action@v3
    with:
      working-directory: pymoors
      run: make test-benchmarks
    env:
      CODSPEED_RUNNER_MODE: "walltime"

```

fails with

```
Preparing the environment
Running the benchmarks
  [pymoors] benchmarks only
  Resolved 121 packages in 0.59ms
     Building pymoors @ file:///home/runner/work/pymoors/pymoors/pymoors
  #################
  ### Omitted logs ###
  ################
  `/home/runner/work/pymoors/pymoors/pymoors/target/wheels`: Permission
denied (os error 13)

```

Probably synching first will do the work, but on main (without multiple libraries repo), I have the action working fine following the same steps.

1.  Install uv and create .venv
2. Build the wheel
3. Install the wheel
4. sync testing dependencies with uv
5. Run pytest

**EDIT**:

Steps listed above were wrong, I always did

1.  Install uv and create .venv
2. sync testing dependencies with uv
2. Build the wheel
3. Install the wheel **forcing pymoors reinstall from step 2**
5. Run pytest

Important to note that in step 2 all the dependencies are correctly installed in the created virtual env, note that pydantic's team do the same logic in their [codspeed action
](https://github.com/pydantic/pydantic-core/blob/main/.github/workflows/codspeed.yml). 

Looks like when the wheel is created first and then sync, the last doesn't recognize that the project deps were installed (my hunch some stuff related with `uv.lock`).

I was wondering if there is a flag like `--no-deps` or something to install only group dependencies and no the project, i don't like to force reinstall because in the end we build the project twice

Thanks!!

---

_Comment by @andresliszt on 2025-04-26 16:11_

@eifinger Thank you very much really for your help, I edited my comment above and could make all my actions to work. There are things with `uv run` that i still don't understand at 100% but will be reading and looking deeply in the code.

Thanks again!

---

_Closed by @charliermarsh on 2025-05-04 12:38_

---
