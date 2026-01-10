```yaml
number: 8433
title: Preserve symlinks when creating virtual environments
type: pull_request
state: closed
author: charliermarsh
labels:
  - breaking
assignees: []
base: tracking/050
head: charlie/preserve
created_at: 2024-10-22T00:16:50Z
updated_at: 2024-10-28T15:55:08Z
url: https://github.com/astral-sh/uv/pull/8433
synced_at: 2026-01-10T12:54:09Z
```

# Preserve symlinks when creating virtual environments

---

_Pull request opened by @charliermarsh on 2024-10-22 00:16_

## Summary

Historically, when creating a virtual environments on Unix, we used `canonicalize` to resolve all symlinks to find the "base" interpreter. This has some undesirable affects, especially for Homebrew-installed Pythons, because it means that we use the patch version of the interpreter as a base -- so if you then upgrade your Python version with Homebrew, all of your virtual environments break.

This PR modifies the behavior as follows:

- If we're not in a virtual environment (common case), just use `sys.executable` without resolving anything.
- If we're in a virtual environment, resolve symlinks until we find a non-virtual interpreter.

This leads to desired behavior for a variety of Pythons. The behaviors of various tools are collated [here](https://docs.google.com/spreadsheets/d/1Vw5ClYEjgrBJJhQiwa3cCenIA1GbcRyudYN9NwQaEcM/edit?gid=0#gid=0), with the proposed behavior on the far right. To summarize the results (AFAICT):

- `venv` uses the `sys.executable` when outside a virtual environment, and `sys._base_executable` when inside it. (This leads to non-ideal results for nested Homebrew virtual environments, as in the first column on the spreadsheet.)
- The latest `virtualenv` looks _mostly_ like current-uv (it fully resolves the executable).
- Prior versions of `virtualenv` did something else that looks more like this PR.

Closes https://github.com/astral-sh/uv/issues/1640.
Closes https://github.com/astral-sh/uv/issues/1795.


---

_Review requested from @zanieb by @charliermarsh on 2024-10-22 00:16_

---

_Review requested from @konstin by @charliermarsh on 2024-10-22 00:16_

---

_Label `breaking` added by @charliermarsh on 2024-10-22 00:17_

---

_Comment by @charliermarsh on 2024-10-22 00:20_

For posterity (and testing), I used this janky script to test various behaviors:

```python
import sys
import subprocess
import os
import tempfile

def create_venv_and_print_info(python_path):
    virtualenv = '20.25.0'
    uv = True
    cargo = False

    # Create a temporary directory for the first venv
    with tempfile.TemporaryDirectory() as temp_dir:
        # Create first venv using subprocess
        if cargo:
            subprocess.run(['../uv/target/debug/uv', 'venv', '-p', python_path, temp_dir], check=True)
        elif uv:
            subprocess.run(['uv', 'venv', '-p', python_path, temp_dir], check=True)
        elif virtualenv is None:
            subprocess.run([python_path, '-m', 'venv', temp_dir], check=True)
        else:
            subprocess.run(['uvx', f'virtualenv@{virtualenv}', '-p', python_path, temp_dir], check=True)

        # Path to the Python binary in the first venv
        venv_python = os.path.join(temp_dir, 'bin', 'python')

        # Run Python in the nested venv to get the required information
        command = f'{venv_python} -c "import sys; print(sys._base_executable); print(sys.base_prefix)"'
        result = subprocess.run(command, shell=True, capture_output=True, text=True, check=True)

        # Print nested venv information
        with open(os.path.join(temp_dir, 'pyvenv.cfg'), 'r') as f:
            for line in f:
                if line.startswith('home ='):
                    print(line.split('=')[1].strip())
                    break

        print(result.stdout.strip().split('\n')[0])
        print(result.stdout.strip().split('\n')[1])
        
        # Create a nested temporary directory for the second venv
        with tempfile.TemporaryDirectory() as nested_temp_dir:
            # Create nested venv using subprocess
            if cargo:
                subprocess.run(['../uv/target/debug/uv', 'venv', '-p', venv_python, nested_temp_dir], check=True)
            elif uv:
                subprocess.run(['uv', 'venv', '-p', venv_python, nested_temp_dir], check=True)
            elif virtualenv is None:
                subprocess.run([venv_python, '-m', 'venv', nested_temp_dir], check=True)
            else:
                subprocess.run(['uvx', f'virtualenv@{virtualenv}', '-p', venv_python, nested_temp_dir], check=True)

            # Path to the Python binary in the nested venv
            nested_venv_python = os.path.join(nested_temp_dir, 'bin', 'python')
            
            # Run Python in the nested venv to get the required information
            command = f'{nested_venv_python} -c "import sys; print(sys._base_executable); print(sys.base_prefix)"'
            result = subprocess.run(command, shell=True, capture_output=True, text=True, check=True)

            # Print nested venv information
            with open(os.path.join(nested_temp_dir, 'pyvenv.cfg'), 'r') as f:
                for line in f:
                    if line.startswith('home ='):
                        print(line.split('=')[1].strip())
                        break

            print(result.stdout.strip().split('\n')[0])
            print(result.stdout.strip().split('\n')[1])

        

if __name__ == '__main__':
    if len(sys.argv) != 2:
        print("Usage: python script.py <path_to_python_interpreter>")
        sys.exit(1)

    python_interpreter_path = sys.argv[1]
    create_venv_and_print_info(python_interpreter_path)
```

---

_Comment by @ofek on 2024-10-22 00:28_

Would the proposed solution break this person's situation? https://github.com/pypa/virtualenv/issues/2682

---

_Comment by @charliermarsh on 2024-10-22 00:34_

Probably yeah.

---

_Comment by @charliermarsh on 2024-10-22 00:36_

But I think it's the right tradeoff to have the behavior here and break that unusual setup. Per https://github.com/pypa/virtualenv/issues/2770#issuecomment-2376338499 though we could also just keep resolving while it's not a "Python environment".

---

_Comment by @ofek on 2024-10-22 00:38_

I was actually just about to link you the same recommendation 🙂 Can we please consider doing that instead?

---

_Comment by @charliermarsh on 2024-10-22 00:39_

I'm not really sure how to detect that.

---

_Comment by @ofek on 2024-10-22 00:40_

cc @pfmoore for visibility

---

_Comment by @ofek on 2024-10-22 00:45_

Ah it seems that's also what someone recommended recently on the corresponding CPython issue:

- https://github.com/python/cpython/issues/106045#issuecomment-2380588780
- https://github.com/python/cpython/pull/115237

---

_Comment by @charliermarsh on 2024-10-22 00:49_

I don't fully understand the motivation for that CPython PR. That seems to be recursively resolving symlinks regardless of whether it's a virtual environment or not. Why would that _not_ break the Homebrew case?

---

_Comment by @ofek on 2024-10-22 00:51_

Perhaps it would, I haven't looked at the code so much as I wanted to just bring it to your attention in case it helps in the implementation of Paul's idea.

---

_Comment by @charliermarsh on 2024-10-22 00:52_

I think it still would because it's recursively resolving symlinks for `home`. But maybe it solves a different problem.

---

_Comment by @charliermarsh on 2024-10-22 00:53_

(We seem to have this same issue in our test suite, we have symlinks to Pythons that aren't a full "install tree", just an executable.)

---

_Comment by @zanieb on 2024-10-22 00:57_

> (We seem to have this same issue in our test suite, we have symlinks to Pythons that aren't a full "install tree", just an executable.)

This suggests we should take on some long needed improvements to our mock Python installations in tests.

---

_Comment by @charliermarsh on 2024-10-22 00:58_

I'm not sure... I'm starting to wonder if this is really the right solution.

---

_Comment by @charliermarsh on 2024-10-22 01:00_

Like, I need to figure out how we can know that `/opt/homebrew/opt/python@3.12/bin/python3.12` is a fine place to stop, but `/Users/crmarsh/.local/share/uv/tests/.tmpgNwkV3/python/3.12/python3` is not.

---

_Comment by @charliermarsh on 2024-10-22 01:02_

E.g., we could check if `/opt/homebrew/opt/python@3.12/bin/python3.12/../../lib` exists, but like...

---

_Comment by @charliermarsh on 2024-10-22 01:06_

Other options include...

- Use `sys._base_executable` on Unix, like `venv`. We'd get the "right" behavior for Homebrew, but not with nested virtualenvs (which is maybe fine).

- Stick with what we have here, fix our test suite setup, and call "symlinks that aren't Python installations" unsupported.

- Stick with our current behavior.


---

_Comment by @ofek on 2024-10-22 01:08_

FYI not sure if UV has the same concept but Hatch won't even consider certain paths as eligible for creating virtual environments if they are not considered stable: https://github.com/pypa/hatch/blob/hatch-v1.13.0/src/hatch/env/virtual.py#L411-L430

---

_Comment by @charliermarsh on 2024-10-22 01:31_

Ah yeah, we don't have such a concept.

---

_Comment by @zanieb on 2024-10-22 01:39_

> Stick with what we have here, fix our test suite setup, and call "symlinks that aren't Python installations" unsupported.

This breaks the goals of creating a stable Python versions directory as described in [this internal design](https://www.notion.so/astral-sh/Installing-a-python-command-with-uv-11f48797e1ca8054ba80e93154b2b241?pvs=4#11f48797e1ca80a2a8bbc05c90cfd2b6). Of course, we can do whatever we want for executables we control. But I was hoping that requirement would help inform the generalized solution.

---

_Comment by @charliermarsh on 2024-10-22 01:50_

If we just want to resolve the linked issues, I think using `sys._base_executable` would do that in most cases.

---

_Review comment by @konstin on `crates/uv-tool/src/lib.rs`:359 on 2024-10-22 17:06_

nit:

```suggestion
fn as_absolute_path(path: OsString) -> Option<PathBuf> {
```

---

_@konstin approved on 2024-10-22 17:07_

This behavior sounds correct.

The PR has more commits than it should (does it need a rebase?)

---

_Comment by @zanieb on 2024-10-22 17:12_

(I recently rebased the tracking branch)

---

_@zanieb reviewed on 2024-10-22 19:40_

---

_Review comment by @zanieb on `crates/uv-tool/src/lib.rs`:359 on 2024-10-22 19:40_

(this was resolved during a rebase of #8419 merging #8048 with #8453)

---

_Comment by @charliermarsh on 2024-10-22 20:22_

This is really complicated and may need to be resolved in the standard library (if at all). I'm tempted to just emulate the standard library behavior for now (use `sys._base_executable` rather than canonicalizing).

---

_Comment by @ofek on 2024-10-22 20:50_

> I'm tempted to just emulate the standard library behavior for now (use `sys._base_executable` rather than canonicalizing).

Wouldn't maintaining the current behavior cause the least amount of breakage? Not in the sense of UV but overall use.

---

_Comment by @charliermarsh on 2024-10-22 21:16_

Do you mean, least disruption for existing users ? Or does the current behavior do something that you prefer as compared to the standard library?

---

_Comment by @ofek on 2024-10-22 21:57_

I'm just trying to understand, since there is not a great complete solution, which implementation breaks the fewest number of people based on your understanding of use cases in the wild.

---

_Comment by @pfmoore on 2024-10-22 22:39_

I've not really been following the variations on this, so I'm struggling to keep clear in my mind what's being proposed and what the various behaviours are. But my basic view is that no-one is served by having `venv`, `virtualenv` and `uv` behave differently when it comes to creating virtual environments. With that in mind, and given that `venv` is part of the stdlib, I would prefer it if:

1. `uv` and `virtualenv` did the same as `venv` does.
2. If there's a case for different behaviour, it gets proposed as a change to `venv` in the first place, with 3rd party tools changing once the core developers accept the new behaviour. I'm fine with tools implementing the new behaviour before it becomes available in the stdlib, as long as it's been *accepted* for the stdlib.

The use cases I am aware of here are described in https://github.com/pypa/virtualenv/issues/2682 and https://github.com/pypa/virtualenv/issues/2770. Virtualenv "fixed" 2682, breaking compatibility with `venv` in the process. I personally believe that was a mistake (for the reasons I state above) but https://github.com/python/cpython/issues/106045 was raised asking for the corresponding change in `venv`, and *if* that gets accepted (it hasn't yet) then the current behaviour of `virtualenv` would (IMO) be OK on the basis of being in alignment with the accepted behaviour of `venv`.

> I'm just trying to understand, since there is not a great complete solution, which implementation breaks the fewest number of people based on your understanding of use cases in the wild.

As noted, I only know of 2 cases, and I would *also* be interested if `uv` is aware of any others. As https://github.com/pypa/virtualenv/issues/2682 affects Homebrew users, I suspect that it affects more users than the other issue. But having said that, no Homebrew users had complained about the behaviour of `virtualenv` or `venv` until January of this year, suggesting that it can't be *that* common a problem (given how many people use Homebrew...)

> This is really complicated and may need to be resolved in the standard library (if at all). I'm tempted to just emulate the standard library behavior for now (use sys._base_executable rather than canonicalizing).

To be honest, that seems to me to be by far the safest option. If you do this, the only thing I'd suggest is that you keep an eye on https://github.com/python/cpython/issues/106045 - I see @konstin is active on that issue, though, so you have that covered.

One other thing I'll note. The original post here said:

> If we're in a virtual environment, resolve symlinks until we find a non-virtual interpreter.

I don't know if that's still the behaviour, but it seems like it would be broken on Windows. Windows virtual environments (at least ones created by `venv`, and I assume all others) don't use symlinks to point back to the base interpreter. Instead they use a custom launcher executable (which is undocumented, and considered an implementation detail). The *only* supported way of finding the environment's base interpreter is by reading `sys.base_prefix` (or the `home` key in `pyvenv.cfg) and locating the executable from there.

---

_Comment by @zanieb on 2024-10-22 22:47_

> But having said that, no Homebrew users had complained about the behaviour of virtualenv or venv until January of this year, suggesting that it can't be that common a problem (given how many people use Homebrew...)

I think it's very common for people to have their Homebrew virtual environments broken during patch version upgrades, e.g., there are a lot of complaints about this in pipx which I presume uses one of those two libraries for virtual environment creation. From what I understand, if it did not completely resolve the path, it'd use the minor-version directory that Homebrew constructs to avoid this problem and the user experience would be improved. I'm surprised you haven't seen complaints about this, it's a frequent pain point people highlight with Homebrew Python.

(Besides this nit, I think I'm pretty well aligned with your summary)

---

_Comment by @charliermarsh on 2024-10-22 23:04_

Thanks for chiming in @pfmoore -- I really appreciate it. I'm in agreement with your summary:

1. Ideally, we should have the same functional behavior as the standard library, at least as far as correctness goes.
2. As such, we should use `sys._base_executable` for now.

For uv users, using `sys._base_executable` will not change behavior for the vast majority of cases, but it will fix the Homebrew issues that have been reported.

I only have a few other misc. comments to offer:

> But having said that, no Homebrew users had complained about the behaviour of virtualenv or venv until January of this year, suggesting that it can't be that common a problem (given how many people use Homebrew...)

I think venv actually has the "right" behavior for Homebrew (see [sheet](https://docs.google.com/spreadsheets/d/1Vw5ClYEjgrBJJhQiwa3cCenIA1GbcRyudYN9NwQaEcM/edit?gid=0#gid=0)), in that it resolves `home` to (e.g.) `/opt/homebrew/opt/python@3.12/bin`. This matched prior versions of virtualenv, but not the most recent releases. From that perspective, uv was diverging, and the change to use `sys._base_executable` will bring it into alignment with the standard library and "fix" that issue (which was reported to us several times).

Regarding https://github.com/pypa/virtualenv/issues/2682 and https://github.com/python/cpython/issues/106045: it's true that we'll now suffer from these issues, but I have some suspicion that there's more going on there than just the `realpath` resolution, as per @konstin's comments and testing. Those also seem like minority cases.

> I don't know if that's still the behaviour, but it seems like it would be broken on Windows.

We actually already used `sys._base_executable` on Windows, and this PR didn't touch the Windows behavior. So today at least I think we're actually closer to `python -m venv` on Windows than we are on Linux.


---

_Comment by @charliermarsh on 2024-10-22 23:19_

Ok, I'm pursuing those changes in https://github.com/astral-sh/uv/pull/8481 rather than repurposing this PR.

---

_Comment by @konstin on 2024-10-23 09:58_

> As noted, I only know of 2 cases, and I would also be interested if uv is aware of any others. As https://github.com/pypa/virtualenv/issues/2682 affects Homebrew users, I suspect that it affects more users than the other issue. But having said that, no Homebrew users had complained about the behaviour of virtualenv or venv until January of this year, suggesting that it can't be that common a problem (given how many people use Homebrew...)

From personal experience with Python users, these problems are underreported. I'm for example affected by #1795, but would have never considered reporting it.

I also want to reraise https://github.com/python/cpython/issues/114476: To have consensus between tools, we need a documented API with the correct value.

---

_Comment by @charliermarsh on 2024-10-24 16:59_

In #8484, I also found that I needed to use a heuristic to workaround a limitation in the `python-build-standalone` builds (if 
you symlink one of those binaries to another location, `sys._base_executable` resolves to that new location, so `home` gets set to something that isn't a Python installation). Related: https://github.com/indygreg/python-build-standalone/issues/380.

The heuristic I have there, for now, is: if `sys._base_executable` isn't in the `scripts` path, resolve the symlink and try again. I don't know if that's entirely sound, though.


---

_Comment by @charliermarsh on 2024-10-28 15:55_

Closing in favor of https://github.com/astral-sh/uv/pull/8481.

---

_Closed by @charliermarsh on 2024-10-28 15:55_

---
