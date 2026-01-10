---
number: 2679
title: Support for platform independent lockfile and SHA-pinning
type: issue
state: closed
author: DetachHead
labels:
  - enhancement
assignees: []
created_at: 2024-03-26T23:03:38Z
updated_at: 2024-08-30T20:40:29Z
url: https://github.com/astral-sh/uv/issues/2679
synced_at: 2026-01-10T01:23:20Z
---

# Support for platform independent lockfile and SHA-pinning

---

_Issue opened by @DetachHead on 2024-03-26 23:03_

re-raising https://github.com/astral-sh/rye/issues/881

> Hi there, thank you for the great effort put into this amazing project.
> 
> I'm asking whether you plan in the future (maybe when `uv` support won't no longer be experimental) to overcome the [current limitations](https://rye-up.com/guide/sync/#limitations) in order to being able to:
> 
>     * Generate a platform-indipendent lockfile
> 
>     * Pin the dependency version by SHA. You can already do that with pip-tools (option `--generate-hashes`), but apparently this is not available inside the rye CLI
> 
>     * Check for outdated dependencies (like you can do with pip: `pip list --outdated`)



---

_Referenced in [astral-sh/rye#881](../../astral-sh/rye/issues/881.md) on 2024-03-26 23:03_

---

_Referenced in [DetachHead/pytest-robotframework#251](../../DetachHead/pytest-robotframework/pulls/251.md) on 2024-03-26 23:03_

---

_Renamed from "Support for platform indipendent lockfile and SHA-pinning" to "Support for platform independent lockfile and SHA-pinning" by @AlexWaygood on 2024-03-26 23:11_

---

_Comment by @KotlinIsland on 2024-03-26 23:28_

Poetry and PDM support this feature, I think that it is essential for managing any kind of project that will be developed/run on different platforms.

---

_Comment by @charliermarsh on 2024-03-27 00:15_

Yes, we absolutely plan to support this.

---

_Label `enhancement` added by @charliermarsh on 2024-03-27 14:59_

---

_Comment by @considerate on 2024-03-28 19:51_

@charliermarsh Are you planning on creating a new format or do you plan to export to `poetry.lock` or `pdm.lock`?

I would personally be very grateful if you would target `poetry.lock` since this enables interoperability with nix using [poetry2nix](https://github.com/nix-community/poetry2nix).

---

_Referenced in [astral-sh/uv#1870](../../astral-sh/uv/issues/1870.md) on 2024-04-10 14:15_

---

_Assigned to @BurntSushi by @BurntSushi on 2024-04-10 14:16_

---

_Referenced in [astral-sh/uv#2142](../../astral-sh/uv/issues/2142.md) on 2024-04-15 18:31_

---

_Comment by @inoa-jboliveira on 2024-04-15 20:12_

Right now I am having to generate the dependencies for each environment (windows, linux, mac) on different machines when running `uv pip compile` and it is time consuming plus very difficult for developers to change any dependency.

Instead of platform independent lock file, I believe it could be simpler to just allow uv to receive the platform parameters and build the file to the specified platform. 

Something like 
```
uv pip compile --target windows-x86_64
uv pip compile --target linux-i686
up pip compile --target mac-aarch64
```

---

_Comment by @considerate on 2024-04-16 01:00_

I think the suggestion by @inoa-jboliveira would be a great intermediate step to getting cross-platform support for lock files. I would like to suggest to using the first two parts of the [target triplet](https://www.gnu.org/software/autoconf/manual/autoconf-2.68/html_node/Specifying-Target-Triplets.html ) (i.e `cpu-vendor` rather than the above `vendor-cpu`).

That would yield:
```
uv pip compile --target x86_64-windows
uv pip compile --target i686-pc-linux
up pip compile --target aarch64-darwin
```




---

_Comment by @charliermarsh on 2024-04-16 01:10_

Yeah we've considered / are considering such designs but we're unlikely to implement anything like that until we've made a holistic decision on how we want to handle cross-platform locking.


---

_Comment by @charliermarsh on 2024-04-16 01:10_

For what it's worth, there's extensive discussion around similar ideas here: https://discuss.python.org/t/lock-files-again-but-this-time-w-sdists/46593/1.

---

_Comment by @charliermarsh on 2024-04-16 01:20_

(I believe we could support a `--target` thing pretty easily. It turns out that a target triple isn't nearly enough to fully specify a "Python platform" given the markers that Python exposes in dependency resolution, but we would probably pick some "prototypical" set of environment markers based on the triple. The resolution wouldn't be guaranteed to work on all `x86_64-linux` machines, since e.g. the glibc version could differ, but in reality people are already relying on those kinds of resolutions being portable given that they're locking on one machine and using the files on others.)

---

_Comment by @inoa-jboliveira on 2024-04-16 02:12_

Not only it should be much simpler to support a target environment (since you kinda do that right now selecting the current environment) but it does not restrict you from later on having multiple targets so files can be portable cross platform.

In terms of adoption of uv, I believe it is better to provide the functionality and later improve it than to be a blocker for many people. I'm pushing hard to replace all my tooling stack with uv on a large project. 

---

_Comment by @jbcpollak on 2024-04-16 17:33_

a `--target` solution would be a good interim solution for our usecase.

---

_Comment by @charliermarsh on 2024-04-16 17:33_

Yeah I may hack on it when I have some time.

---

_Comment by @paco-sevilla on 2024-04-17 13:07_

Hi all!
FWIW, I wanted to mention that at the company where I work, we use [pip-compile-cross-platform](https://pypi.org/project/pip-compile-cross-platform/) to get a single `requirements.txt` file that works on all platforms.
It uses `poetry` in the background, so it's extremely slow...

It would be **awesome** if `uv` could generate an output like that (instead of a `requirements.txt` targeting a single platform).

---

_Comment by @charliermarsh on 2024-04-18 03:06_

I've prototyped this here: https://github.com/astral-sh/uv/pull/3111

---

_Comment by @charliermarsh on 2024-04-19 02:58_

The next version will support `--platform` on `uv pip compile`: https://github.com/astral-sh/uv/pull/3111.

---

_Comment by @charliermarsh on 2024-04-19 02:58_

I'm not going to close this though since the broader task here remains.

---

_Comment by @inoa-jboliveira on 2024-04-19 16:48_

@charliermarsh Thank you, looks very nice! Is it going to be released soon?

---

_Comment by @charliermarsh on 2024-04-19 17:17_

It will probably go out today.

---

_Comment by @hauntsaninja on 2024-04-21 21:37_

https://github.com/astral-sh/uv/pull/3111 is awesome and makes getting a "good enough" cross-platform lock file trivial.

Dinky little example script:

```python
import argparse
import os
import subprocess
import sys
import tempfile
from pathlib import Path

from packaging.markers import Marker
from packaging.requirements import Requirement


def parse_requirements_txt(req_file: str) -> list[str]:
    def strip_comments(s: str) -> str:
        try:
            return s[: s.index("#")].strip()
        except ValueError:
            return s.strip()

    entries = []
    with open(req_file) as f:
        for line in f:
            entry = strip_comments(line)
            if entry:
                entries.append(entry)
    return entries


def marker_to_uv(key: str, value: str) -> list[str]:
    if key == "sys_platform":
        if value == "linux":
            uv_platform = "linux"
        elif value == "darwin":
            uv_platform = "macos"
        elif value == "win32":
            uv_platform = "windows"
        else:
            raise ValueError(f"Unknown sys_platform {value}")
        return ["--python-platform", uv_platform]
    if key == "python_version":
        return ["--python-version", value]
    raise ValueError(f"Cannot convert marker {key} to uv input")


Environment = dict[str, str]


def env_to_marker(environment: Environment) -> Marker:
    return Marker(" and ".join(f"({k} == {repr(v)})" for k, v in environment.items()))


def cross_environment_lock(src_file: Path, environment_matrix: list[Environment]) -> str:
    cmd = ["uv", "pip", "compile", "--python", sys.executable, "--no-header", "--no-annotate"]
    with tempfile.TemporaryDirectory() as tmpdir:
        joined: dict[Requirement, list[Environment]] = {}
        for i, environment in enumerate(environment_matrix):
            out_file = os.path.join(tmpdir, src_file.stem + "." + str(i))
            env_cmd = cmd + [src_file, "--output-file", out_file]
            for key, value in environment.items():
                env_cmd.extend(marker_to_uv(key, value))
            subprocess.check_call(env_cmd, stdout=subprocess.DEVNULL)

            for r in parse_requirements_txt(out_file):
                joined.setdefault(Requirement(r), []).append(environment)

        common = [r for r, envs in joined.items() if len(envs) == len(environment_matrix)]
        cross_environment = [
            (r, envs) for r, envs in joined.items() if len(envs) != len(environment_matrix)
        ]
        cross_environment.sort(key=lambda r: r[0].name)

        output = [str(r) for r in common]
        for req, environments in cross_environment:
            req = Requirement(str(req))  # make a copy
            joint_marker = Marker(" or ".join(f"({env_to_marker(env)})" for env in environments))
            if req.marker is None:
                req.marker = joint_marker
            else:
                # Note that uv currently doesn't preserve markers, so this branch is unlikely
                # https://github.com/astral-sh/uv/issues/1429
                req.marker._markers = [req.marker._markers, "and", joint_marker._markers]
            output.append(str(req))

        return "\n".join(output)


def main() -> None:
    parser = argparse.ArgumentParser()
    parser.add_argument("src_file")
    parser.add_argument("--output-file")
    args = parser.parse_args()

    output = cross_environment_lock(
        Path(args.src_file),
        [
            {"sys_platform": "linux"},
            {"sys_platform": "darwin", "python_version": "3.10"},
        ],
    )
    print(output)
    if args.output_file:
        with open(args.output_file, "w") as f:
            f.write(output)


if __name__ == "__main__":
    main()
```

---

_Comment by @charliermarsh on 2024-04-21 21:49_

Oh wow, that's cool. So you do multiple resolutions, then merge the `requirements.txt` and gate the entries from each resolution with a marker for that platform?

---

_Comment by @hauntsaninja on 2024-04-21 22:50_

Yup, uv's fast enough that resolving multiple times is totally fine.

Unsolicited thoughts on what I want from a lock file:

1. Restrict to a finite set of files
1a. The finite set of files should be reasonably minimal
1b. Finite here is defined as fixed / close-ended (should refer to the same set of files tomorrow)
2. Installing in the same marker environment always gets me the same result (modulo insane sdists)
3. Should be guaranteed to work in marker environments I explicitly ask for
4. Has a reasonable chance of generalising to other unknown marker environments
4a. I get a good error if running installing to some other marker environment but it didn't generalise and the resolution is wrong

With the above:
- Assuming you extend the script to support hashes, you get 1,1a,1b,2.
- You get 3 for `sys_platform` and `python_version` markers (which are the most important two). I guess also maybe `platform_machine` marker can work
- You get some of 4, but it could be better, especially for `python_version`. Implementing https://github.com/astral-sh/uv/issues/1429 would also help
- You get errors for 4a in some cases, but it's not perfect
- ...which is overall pretty great!

The best way I see to get 4/4a is you probably want something more like "record the input requirements, use the lock file as a source of constraints, at install time re-resolve the input requirements using the lock file constraints". uv exposes almost all the building blocks you'd need, I think the only thing missing is a few missing features from constraint files (e.g. interactions between constraints and hashes, express an "or" constraint, lmk if details are useful)

---

_Comment by @considerate on 2024-04-22 04:23_

@hauntsaninja In addition to the above list of nice properties to have in a lock file I'd like to add:

5. The lock file should store enough information so that no access to any PyPI index needs to be performed to determine where to find the package and which file corresponds to what hash.
     - One way of supporting this would be to add URL annotations to the lock file like described in
        #3034
     - This is important to guarantee reproducible builds. Without this information, a build now relies on the state of the remote PyPI server at time of building to figure out which hash the final package should resolve to.
 
6. The lock file should (optionally) be stored in a format that doesn't require specialized parsing to figure out the structure of the lock file
     - This would imply defining a format using a commonly available serialization format such as JSON or TOML

---

_Referenced in [astral-sh/uv#3333](../../astral-sh/uv/issues/3333.md) on 2024-05-06 07:48_

---

_Referenced in [astral-sh/uv#3672](../../astral-sh/uv/issues/3672.md) on 2024-05-20 17:07_

---

_Referenced in [aspect-build/rules_py#345](../../aspect-build/rules_py/issues/345.md) on 2024-06-03 17:00_

---

_Comment by @brettcannon on 2024-06-12 18:54_

> Unsolicited thoughts on what I want from a lock file:
> 
> 1. Restrict to a finite set of files
>    1a. The finite set of files should be reasonably minimal
>    1b. Finite here is defined as fixed / close-ended (should refer to the same set of files tomorrow)
> 2. Installing in the same marker environment always gets me the same result (modulo insane sdists)
> 3. Should be guaranteed to work in marker environments I explicitly ask for
> 4. Has a reasonable chance of generalising to other unknown marker environments
>    4a. I get a good error if running installing to some other marker environment but it didn't generalise and the resolution is wrong

That should all be supported in the PEP I have coming (just got back from parental leave, but the draft PEP is written and just waiting on me to write a PoC which requires adapting some pre-existing code; the draft was also sent to @charliermarsh on discuss.python.org privately in March). I will be posting to https://discuss.python.org/c/packaging/14 once the PEP is ready.

---

_Referenced in [astral-sh/uv#4331](../../astral-sh/uv/issues/4331.md) on 2024-06-14 18:43_

---

_Comment by @zanieb on 2024-06-19 00:40_

For those following, https://github.com/astral-sh/uv/issues/3347 is tracking the lock file work in progress.

---

_Referenced in [astral-sh/uv#4447](../../astral-sh/uv/issues/4447.md) on 2024-06-22 14:38_

---

_Comment by @charliermarsh on 2024-06-26 21:52_

In `v0.2.16`, `uv pip compile` supports a `--universal` flag.

It's conceptually similar to [`pip-compile-cross-platform`](https://pypi.org/project/pip-compile-cross-platform/): we perform a "universal" resolution (like Poetry), then write the output to a `requirements.txt` with platform marker annotations as necessary -- so the resulting `requirements.txt` should work on any platform, assuming resolution succeeds.


---

_Comment by @charliermarsh on 2024-06-26 21:53_

`--universal` uses the same resolver that powers our `uv lock` command (coming soon).

It's definitely experimental! And part of the thinking here (in addition to shipping something useful in the `uv pip compile` interface) is that it will make it easier for folks to help us stress test the resolver. Would appreciate any feedback (via new issues).


---

_Referenced in [astral-sh/rye#1056](../../astral-sh/rye/issues/1056.md) on 2024-06-28 19:18_

---

_Referenced in [astral-sh/uv#5835](../../astral-sh/uv/issues/5835.md) on 2024-08-06 21:49_

---

_Comment by @T-256 on 2024-08-20 19:51_

Since uv 0.3.0 stabilized `uv lock`, should close this issue?

---

_Comment by @zanieb on 2024-08-20 19:56_

Yes, thanks!

---

_Closed by @zanieb on 2024-08-20 19:56_

---

_Comment by @danielgafni on 2024-08-30 20:25_

Hey, just noticed README.md still has this section: 

> Unlike Poetry and PDM, uv does not yet produce a machine-agnostic lockfile (#2679).

---

_Comment by @charliermarsh on 2024-08-30 20:31_

@danielgafni -- Where are you seeing that? Are you looking at an old version perhaps?

---

_Comment by @danielgafni on 2024-08-30 20:40_

Oh damn... I'm on my phone so can't really track it down but this must be the case! 😄 sorry for the false alarm 

---
