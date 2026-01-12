```yaml
number: 13026
title: Add basic red knot benchmark
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: knot-mypy-pyright-benchmarks
created_at: 2024-08-21T09:09:12Z
updated_at: 2024-08-23T06:59:41Z
url: https://github.com/astral-sh/ruff/pull/13026
synced_at: 2026-01-12T15:55:42Z
```

# Add basic red knot benchmark

---

_@MichaReiser_

## Summary

This PR adds a basic benchmark runner for red knot. These benchmarks are different from codspeed in that they benchmark
entire projects. They are not micro-benchmarks. 

The benchmarks aren't representive! Don't draw any conclusion from them today. 
Red Knot only implements a tiny tiny subset of Mypy's and Pyright's functionality. 

This work is inspired by [mypy primer](https://github.com/hauntsaninja/mypy_primer/) but it serves 
a different purpose. We may want to add Red Knot to mypy primer when we have a more complete feature set (and a published binary)

## Test Plan

Ran the benchmarks for each project and verified the diagnostics I saw:

```shell
uv run benchmark -p black --mypy --verbose
```

I'm a bit surprised that I e.g. see an error for Black. I would appreciate it if someone could verify that the diagnostics are reasonable.



---

_@MichaReiser reviewed on 2024-08-21 09:10_

---

_Review comment by @MichaReiser on `scripts/knot_benchmark/src/benchmark/cases.py`:26 on 2024-08-21 09:10_

Not sure if we should remove this one again. I don't have an incremental benchmark yet but the idea would be to switch between two revisions. 

---

_Label `red-knot` added by @MichaReiser on 2024-08-21 09:13_

---

_Comment by @github-actions[bot] on 2024-08-21 09:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:236 on 2024-08-21 09:49_

(It would be good if we could at least add `// TODO` inline comments to these. There are some places where it will be "correct" for us to infer something as `Unknown`, but these are not that -- here we're just using `Unknown` as a placeholder until we implement the correct logic.)

---

_Review comment by @AlexWaygood on `scripts/knot_benchmark/README.md`:16 on 2024-08-21 09:51_

```suggestion
Red Knot only implements a tiny fraction of Mypy's and Pyright's functionality,
so the benchmarks aren't in any way a fair comparison today. However,
they'll become more meaningful as we build out more type checking features in Red Knot.
```

---

_Review comment by @AlexWaygood on `scripts/knot_benchmark/pyproject.toml`:20 on 2024-08-21 09:52_

just because I know this will annoy you ;)

```suggestion
    "E501",  # We use ruff format
```

---

_Review comment by @AlexWaygood on `scripts/knot_benchmark/src/benchmark/__init__.py`:69 on 2024-08-21 09:56_

You need to be quite careful with the `shlex` module as it might not produce strings that work on Windows. There are some wrapper functions in `mypy_primer` to deal with this problem. See discussion in https://github.com/hauntsaninja/mypy_primer/pull/99.

I think it would also be fine for now to keep this as it is and just put in the "Known limitations" section of the README that we haven't tried running this script on Windows yet

---

_Review comment by @AlexWaygood on `scripts/knot_benchmark/src/benchmark/cases.py`:64 on 2024-08-21 10:04_

```suggestion
        if path is None:
            path = Path(__file__) / "../../../../../target/release/red_knot"
            self.path = path.resolve()
        else:
            self.path = path
```

---

_Review comment by @AlexWaygood on `scripts/knot_benchmark/src/benchmark/cases.py`:89 on 2024-08-21 10:08_

`project.include` is typed as being `list[str]`, so `project.include[0]` will always be truthy unless it's the empty string `""`. Why are we specifically checking that `project.include[0]` isn't an empty string? Seems kinda unlikely to me that it would be

---

_Review comment by @AlexWaygood on `scripts/knot_benchmark/src/benchmark/cases.py`:99 on 2024-08-21 10:08_

nit

```suggestion
class Mypy(Tool):
```

---

_Review comment by @AlexWaygood on `scripts/knot_benchmark/src/benchmark/cases.py`:187 on 2024-08-21 10:11_

it's generally considered more idiomatic to use the `/` operator when working with pathlib. I almost never see the `joinpath` method being used 😆

```suggestion
        return self.bin / "{name}{extension}"
```

---

_Review comment by @AlexWaygood on `scripts/knot_benchmark/src/benchmark/cases.py`:183 on 2024-08-21 10:11_

```suggestion
        return self.path / bin_dir
```

---

_Review comment by @AlexWaygood on `scripts/knot_benchmark/src/benchmark/cases.py`:203 on 2024-08-21 10:14_

By default, all stdout and stderr is printed to the terminal when running subprocesses via `subprocess.run()`. So this will result in stderr being printed to the terminal twice if the subprocess fails. If you want to suppress stderr from the subprocess unless it fails, I'd do this:

```suggestion
        try:
            subprocess.run(command, cwd=parent, check=True, text=True, capture_output=True)
        except subprocess.CalledProcessError as e:
            print(e.stderr)
            raise
```

---

_Review comment by @AlexWaygood on `scripts/knot_benchmark/src/benchmark/cases.py`:193 on 2024-08-21 10:15_

```suggestion
        return Venv(root / "venv")
```

---

_Review comment by @AlexWaygood on `scripts/knot_benchmark/src/benchmark/cases.py`:223 on 2024-08-21 10:15_

(Same comment as above regarding `subprocess.run()`)

---

_Review comment by @AlexWaygood on `scripts/knot_benchmark/src/benchmark/projects.py`:31 on 2024-08-21 10:17_

```suggestion
        # Don't do anything if `checkout_dir` is a local path 
        if os.path.exists(os.path.join(checkout_dir, ".git")):
```

---

_@AlexWaygood reviewed on 2024-08-21 10:21_

Some comments from reading through the code

---

_@MichaReiser reviewed on 2024-08-21 10:23_

---

_Review comment by @MichaReiser on `scripts/knot_benchmark/pyproject.toml`:20 on 2024-08-21 10:23_

My toml formatter doesn't like two spaces and neither do i 😅

---

_@MichaReiser reviewed on 2024-08-21 10:24_

---

_Review comment by @MichaReiser on `scripts/knot_benchmark/src/benchmark/__init__.py`:69 on 2024-08-21 10:24_

Hmm. I copied that from uv. But yes, mentioning it in the readme sounds like a good idea

---

_Review comment by @MichaReiser on `scripts/knot_benchmark/src/benchmark/cases.py`:89 on 2024-08-21 10:25_

I guess that's me writing JS. I assume python throws if it is an out of bounds access?

---

_@MichaReiser reviewed on 2024-08-21 10:25_

---

_@AlexWaygood reviewed on 2024-08-21 10:26_

---

_Review comment by @AlexWaygood on `scripts/knot_benchmark/src/benchmark/cases.py`:89 on 2024-08-21 10:26_

Yeah, `IndexError` if it's an empty list and you try to index into it. And there's not `.get()` method for lists like there is in Rust, sadly

---

_Review comment by @MichaReiser on `scripts/knot_benchmark/src/benchmark/cases.py`:187 on 2024-08-21 10:26_

Yeah, not a huge fan of magic operator overloads. It gives the impression that these are numbers. That's why I intentionally used the method but I'm open to changing it if we find this more idiomatic 

---

_@MichaReiser reviewed on 2024-08-21 10:26_

---

_@MichaReiser reviewed on 2024-08-21 10:28_

---

_Review comment by @MichaReiser on `scripts/knot_benchmark/src/benchmark/projects.py`:31 on 2024-08-21 10:28_

That's not the intention. It's to detect if the project has already been cloned 

---

_@AlexWaygood reviewed on 2024-08-21 10:29_

---

_Review comment by @AlexWaygood on `scripts/knot_benchmark/src/benchmark/projects.py`:31 on 2024-08-21 10:29_

Even more reason to add a comment then (though not the one I suggested), since I clearly misunderstood ;)

---

_Review comment by @AlexWaygood on `scripts/knot_benchmark/src/benchmark/cases.py`:187 on 2024-08-21 10:30_

Python developers will be accustomed to seeing `/` used when working with pathlib, and I think would mostly (like I was) be quite surprised to see `joinpath` being used throughout instead of `/`. But I know that not everybody on the team has a huge amount of experience with Python, so I guess there's a tradeoff there.

---

_@AlexWaygood reviewed on 2024-08-21 10:30_

---

_@AlexWaygood reviewed on 2024-08-21 10:52_

---

_Review comment by @AlexWaygood on `scripts/knot_benchmark/README.md`:10 on 2024-08-21 10:52_

```
~/dev/ruff (knot-mypy-pyright-benchmarks) % uv run benchmark                  
   Built ruff @ file:///Users/alexw/dev/ruff
Installed 1 package in 1ms
error: Failed to spawn: `benchmark`
  Caused by: No such file or directory (os error 2)
```

Do I need to `cd` into the `scripts/knot_benchmark` directory as an intermediate step?

---

_@MichaReiser reviewed on 2024-08-21 10:55_

---

_Review comment by @MichaReiser on `scripts/knot_benchmark/README.md`:10 on 2024-08-21 10:55_

Possibly

---

_@AlexWaygood reviewed on 2024-08-21 10:57_

---

_Review comment by @AlexWaygood on `scripts/knot_benchmark/src/benchmark/cases.py`:89 on 2024-08-21 10:57_

I actually got an `IndexError` when I tried running the benchmark locally, so we do need to check the length of the list before indexing into it here:

```
~/dev/ruff (knot-mypy-pyright-benchmarks)⚡ [1] % cd scripts/knot_benchmark       
~/dev/ruff/scripts/knot_benchmark (knot-mypy-pyright-benchmarks)⚡ % uv run benchmark             
Using Python 3.12.4
Creating virtualenv at: .venv
   Built knot-benchmark @ file:///Users/alexw/dev/ruff/scripts/knot_benchmark
Installed 6 packages in 12ms
black (cold)
Benchmark 1: knot
  Time (mean ± σ):      71.3 ms ±   5.3 ms    [User: 58.6 ms, System: 9.1 ms]
  Range (min … max):    67.4 ms …  87.1 ms    41 runs
 
  Warning: Ignoring non-zero exit code.
  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet system without any interferences from other programs.
 
Benchmark 2: Pyright
  Time (mean ± σ):      1.946 s ±  0.010 s    [User: 3.711 s, System: 0.131 s]
  Range (min … max):    1.930 s …  1.958 s    10 runs
 
  Warning: Ignoring non-zero exit code.
 
Benchmark 3: mypy
  Time (mean ± σ):      1.552 s ±  0.013 s    [User: 1.468 s, System: 0.081 s]
  Range (min … max):    1.539 s …  1.584 s    10 runs
 
  Warning: Ignoring non-zero exit code.
 
Summary
  knot ran
   21.78 ± 1.64 times faster than mypy
   27.30 ± 2.04 times faster than Pyright
black (warm)
Benchmark 1: mypy
  Time (mean ± σ):     314.5 ms ±   2.1 ms    [User: 271.6 ms, System: 40.1 ms]
  Range (min … max):   312.2 ms … 319.0 ms    10 runs
 
  Warning: Ignoring non-zero exit code.
 
Traceback (most recent call last):
  File "/Users/alexw/dev/ruff/scripts/knot_benchmark/.venv/bin/benchmark", line 8, in <module>
    sys.exit(main())
             ^^^^^^
  File "/Users/alexw/dev/ruff/scripts/knot_benchmark/src/benchmark/run.py", line 136, in main
    if (command := suite.command(benchmark, project, venv))
                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/alexw/dev/ruff/scripts/knot_benchmark/src/benchmark/cases.py", line 46, in command
    return self.cold_command(project, venv)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/alexw/dev/ruff/scripts/knot_benchmark/src/benchmark/cases.py", line 88, in cold_command
    if directory := project.include[0]:
                    ~~~~~~~~~~~~~~~^^^
IndexError: list index out of range
```

---

_@AlexWaygood reviewed on 2024-08-21 10:59_

---

_Review comment by @AlexWaygood on `scripts/knot_benchmark/README.md`:10 on 2024-08-21 10:59_

It ([nearly](https://github.com/astral-sh/ruff/pull/13026#discussion_r1724856235)) worked after I did that, so I think the answer is "yes" 😄

```suggestion
1. `cd` into the benchmark directory: `cd scripts/knot_benchmark`
1. Run benchmarks: `uv run benchmark`
```

---

_@MichaReiser reviewed on 2024-08-22 13:34_

---

_Review comment by @MichaReiser on `scripts/knot_benchmark/src/benchmark/cases.py`:64 on 2024-08-22 13:34_

I'm okay with the `/` stuff but I prefer the inline `or` (I'm a huge fan of single assignments)

---

_@AlexWaygood reviewed on 2024-08-22 13:36_

---

_Review comment by @AlexWaygood on `scripts/knot_benchmark/src/benchmark/cases.py`:64 on 2024-08-22 13:36_

Fair enough!

---

_Review comment by @AlexWaygood on `scripts/knot_benchmark/src/benchmark/cases.py`:139 on 2024-08-22 13:39_

wow, bizarre. Do we also need to specify `--venv=venv` along with this argument, so that it knows which subdirectory inside the `--venv-path` directory the venv is in? https://github.com/microsoft/pyright/blob/main/docs/import-resolution.md#configuring-your-python-environment

---

_@AlexWaygood reviewed on 2024-08-22 13:41_

Basically this all looks good to me in terms of structure, nothing sticks out

---

_@MichaReiser reviewed on 2024-08-22 13:49_

---

_Review comment by @MichaReiser on `scripts/knot_benchmark/src/benchmark/cases.py`:183 on 2024-08-22 13:49_

It hurts me a lot but I forfeit ;)

---

_@MichaReiser reviewed on 2024-08-22 13:53_

---

_Review comment by @MichaReiser on `scripts/knot_benchmark/src/benchmark/cases.py`:139 on 2024-08-22 13:53_

Yes we do. But there's no `venv` CLI option. So we have to create a config 

```python
    for project in projects:
        with tempfile.TemporaryDirectory() as cwd:
            cwd = Path(cwd)
            project.clone(cwd)

            venv = Venv.create(cwd)
            venv.install(project.dependencies)

            # Set the `venv` config for pyright. Pyright only respects the `--venvpath`
            # CLI option when `venv` is set in the configuration... 🤷‍♂️
            with open(cwd / "pyrightconfig.json", "w") as f:
                f.write(json.dumps(dict(venv=venv.name)))
```

---

_@AlexWaygood reviewed on 2024-08-22 13:55_

---

_Review comment by @AlexWaygood on `scripts/knot_benchmark/src/benchmark/run.py`:26 on 2024-08-22 13:55_

It's unrelated to this PR, but the output we get if you run with `--verbose` makes me think that we should possibly consider emitting paths relative to the first-party root rather than absolute paths in our diagnostic messages

<details>

```
ERROR /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpgxwknamp/src/black/strings.py:55:26: Name 'line' used when not defined.
ERROR /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpgxwknamp/src/black/strings.py:57:33: Name 'line' used when not defined.
ERROR /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpgxwknamp/src/black/strings.py:58:22: Name 'line' used when not defined.
ERROR /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpgxwknamp/src/black/strings.py:73:20: Name 'line' used when not defined.
ERROR /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpgxwknamp/src/black/strings.py:75:38: Name 'line' used when not defined.
ERROR /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpgxwknamp/src/black/strings.py:81:29: Name 'line' used when not defined.
ERROR /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpgxwknamp/src/black/strings.py:82:33: Name 'i' used when not defined.
ERROR /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpgxwknamp/src/black/strings.py:187:30: Name 'orig_quote' used when possibly not defined.
ERROR /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpgxwknamp/src/black/strings.py:192:65: Name 'new_quote' used when possibly not defined.
ERROR /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpgxwknamp/src/black/strings.py:193:67: Name 'new_quote' used when possibly not defined.
ERROR /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpgxwknamp/src/black/strings.py:194:68: Name 'orig_quote' used when possibly not defined.
ERROR /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpgxwknamp/src/black/strings.py:195:36: Name 'orig_quote' used when possibly not defined.
ERROR /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpgxwknamp/src/black/strings.py:195:55: Name 'orig_quote' used when possibly not defined.
ERROR /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpgxwknamp/src/black/strings.py:206:57: Name 'new_quote' used when possibly not defined.
ERROR /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpgxwknamp/src/black/strings.py:210:28: Name 'orig_quote' used when possibly not defined.
ERROR /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpgxwknamp/src/black/strings.py:210:46: Name 'orig_quote' used when possibly not defined.
ERROR /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpgxwknamp/src/black/strings.py:211:58: Name 'orig_quote' used when possibly not defined.
ERROR /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpgxwknamp/src/black/strings.py:212:59: Name 'new_quote' used when possibly not defined.
ERROR /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpgxwknamp/src/black/strings.py:225:28: Name 'm' used when not defined.
ERROR /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpgxwknamp/src/black/strings.py:229:8: Name 'new_quote' used when possibly not defined.
ERROR /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpgxwknamp/src/black/strings.py:237:50: Name 'orig_quote' used when possibly not defined.
ERROR /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpgxwknamp/src/black/strings.py:240:23: Name 'new_quote' used when possibly not defined.
ERROR /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpgxwknamp/src/black/strings.py:240:44: Name 'new_quote' used when possibly not defined.
ERROR /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpgxwknamp/src/black/strings.py:262:65: Name 'new_quote' used when possibly not defined.
ERROR /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpgxwknamp/src/black/strings.py:263:67: Name 'new_quote' used when possibly not defined.
ERROR /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpgxwknamp/src/black/strings.py:267:43: Name 'middle' used when not defined.
ERROR /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpgxwknamp/src/black/strings.py:277:19: Name 'middle' used when not defined.
ERROR /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpgxwknamp/src/black/strings.py:279:60: Name 'new_quote' used when possibly not defined.
ERROR /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpgxwknamp/src/black/strings.py:279:73: Name 'segment' used when not defined.
ERROR /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpgxwknamp/src/black/strings.py:280:12: Name 'segment' used when not defined.
ERROR /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpgxwknamp/src/black/strings.py:282:13: Name 'middle' used when not defined.
ERROR /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpgxwknamp/src/black/strings.py:285:62: Name 'new_quote' used when possibly not defined.
ERROR /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpgxwknamp/src/black/strings.py:288:8: Name 'new_quote' used when possibly not defined.
ERROR /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpgxwknamp/src/black/strings.py:293:29: Name 'middle' used when not defined.
ERROR /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpgxwknamp/src/black/strings.py:303:9: Name 'middle' used when not defined.
ERROR /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpgxwknamp/src/black/strings.py:305:21: Name 'new_quote' used when possibly not defined.
ERROR /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpgxwknamp/src/black/strings.py:386:28: Name 'char' used when not defined.
ERROR /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpgxwknamp/src/black/strings.py:388:20: Name 'i' used when not defined.
ERROR /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpgxwknamp/src/black/files.py:20:1: Import 'mypy_extensions' could not be resolved.
ERROR /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpgxwknamp/src/black/files.py:28:16: Import 'tomllib' could not be resolved.
ERROR /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpgxwknamp/src/black/files.py:42:12: Import 'colorama' could not be resolved.
ERROR /private/var/folders/gd/1czdxc454j15y8gns2krv8vc0000gn/T/tmpgxwknamp/src/black/files.py:433:9: Import 'colorama.initialise' could not be resolved.
```

</details>

---

_@MichaReiser reviewed on 2024-08-22 13:56_

---

_Review comment by @MichaReiser on `scripts/knot_benchmark/src/benchmark/run.py`:26 on 2024-08-22 13:56_

Yeah, we absolutely should once we have proper diagnostics. 

---

_@AlexWaygood reviewed on 2024-08-22 13:58_

---

_Review comment by @AlexWaygood on `scripts/knot_benchmark/src/benchmark/run.py`:26 on 2024-08-22 13:58_

The output from mypy and pyright with `--verbose` all looks like more or less what I'd expect, though, so that's another positive signal that this benchmark script is doing what we want it to!

---

_@AlexWaygood reviewed on 2024-08-22 14:07_

---

_Review comment by @AlexWaygood on `scripts/knot_benchmark/src/benchmark/run.py`:26 on 2024-08-22 14:07_

> It's unrelated to this PR, but the output we get if you run with `--verbose` makes me think that we should possibly consider emitting paths relative to the first-party root rather than absolute paths in our diagnostic messages

(It seems like this is what mypy does, but pyright seems to use absolute paths in its diagnostic messages, like we currently do)

---

_Review comment by @AlexWaygood on `scripts/knot_benchmark/src/benchmark/projects.py`:30 on 2024-08-22 14:23_

```suggestion
        # Skip cloning if the project has already been cloned (the script doesn't yet support updating)
```

---

_@AlexWaygood approved on 2024-08-22 14:24_

---

_@AlexWaygood reviewed on 2024-08-22 14:24_

---

_Review comment by @AlexWaygood on `scripts/knot_benchmark/src/benchmark/cases.py`:139 on 2024-08-22 14:24_

pyright's config setup is so odd...

---

_Marked ready for review by @MichaReiser on 2024-08-22 14:32_

---

_@AlexWaygood reviewed on 2024-08-22 14:34_

---

_Review comment by @AlexWaygood on `scripts/knot_benchmark/src/benchmark/projects.py`:30 on 2024-08-22 14:34_

```suggestion
        # Skip cloning if the project has already been cloned (the script doesn't yet support updating)
```

---

_Merged by @MichaReiser on 2024-08-23 06:22_

---

_Closed by @MichaReiser on 2024-08-23 06:22_

---

_Branch deleted on 2024-08-23 06:22_

---

_@hauntsaninja reviewed on 2024-08-23 06:59_

---

_Review comment by @hauntsaninja on `scripts/knot_benchmark/src/benchmark/cases.py`:204 on 2024-08-23 06:59_

Worth adding `--exclude-newer`? While you pin the commit, you don't pin the deps

---
