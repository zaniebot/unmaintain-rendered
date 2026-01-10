```yaml
number: 18714
title: "[ty] Add more benchmarks"
type: pull_request
state: merged
author: MichaReiser
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: micha/real-world-benchmarks
created_at: 2025-06-17T07:13:25Z
updated_at: 2025-06-20T18:07:01Z
url: https://github.com/astral-sh/ruff/pull/18714
synced_at: 2026-01-10T18:39:08Z
```

# [ty] Add more benchmarks

---

_Pull request opened by @MichaReiser on 2025-06-17 07:13_

## Summary

This PR adds some larger mypy primer projects to our benchmarks. Smaller projects run on codespeed's instrumented runners. Larger projects run on codespeed's walltime runner because they take too long on the instrumented runner. 

I had to use divan instead of criterion for the walltime runner because criterion requires at least 10 iterations for each benchmark, which is too long for our use case. Divan allows us to use arbitrarily few iterations (okay, at least one). I picked a sample of 3x1 (3 iterations), which completes in a reasonable time and hopefully is stable enough not to be noisy. One added advantage of using the walltime runner is that we can measure the end-to-end performance, including file discovery.

Leaving the smaller projects run on instrumented runners has the advantage that the results will be more precise (and we're charged less).

The smaller projects are picked arbitrarily. I'm happy to change them if someone has other suggestions. 

I picked the larger projects mainly on @AlexWaygood suggestions (thank you!)

* altair: That's one I picked. It was pointed out in https://github.com/astral-sh/ty/issues/240 that it makes heavy use of typed dict
* colour-science: very large unions
* Pydantic: Pydantic creates large unions of TypedDicts, which can often be slow to type-check
* Hydra-zen and sympy: Extensive use of generics.
* Pandas: A lot of very, very large classes with a lot of implicit instance attributes.

Both benchmark jobs now complete after roughly 10 minutes (compared to 6min before).  

For the new wall-time benchmarks, see https://codspeed.io/astral-sh/ruff/branches/micha%2Freal-world-benchmarks

Part of https://github.com/astral-sh/ty/issues/240

---

_Label `ty` added by @MichaReiser on 2025-06-17 07:13_

---

_Comment by @github-actions[bot] on 2025-06-17 07:16_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @github-actions[bot] on 2025-06-17 07:27_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Renamed from "Add real world benchmarks" to "[tuAdd real world benchmarks" by @MichaReiser on 2025-06-17 11:32_

---

_Renamed from "[tuAdd real world benchmarks" to "[ty] Add larger benchmarks" by @MichaReiser on 2025-06-17 11:32_

---

_Label `testing` added by @MichaReiser on 2025-06-17 11:32_

---

_Renamed from "[ty] Add larger benchmarks" to "[ty] Add more benchmarks" by @MichaReiser on 2025-06-17 11:34_

---

_Marked ready for review by @MichaReiser on 2025-06-17 16:53_

---

_Review requested from @carljm by @MichaReiser on 2025-06-17 16:53_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-06-17 16:53_

---

_Review requested from @sharkdp by @MichaReiser on 2025-06-17 16:53_

---

_Review requested from @dcreager by @MichaReiser on 2025-06-17 16:53_

---

_@MichaReiser reviewed on 2025-06-17 16:58_

---

_Review comment by @MichaReiser on `crates/ruff_benchmark/Cargo.toml`:79 on 2025-06-17 16:58_

```suggestion
# Enables the benchmark that should only run with codspeed's instrumented runner
instrumented = [
    "criterion",
    "ruff_linter",
    "ruff_python_formatter",
    "ruff_python_parser",
    "ruff_python_trivia",
    "ty_project",
]
```

---

_@MichaReiser reviewed on 2025-06-17 16:59_

---

_Review comment by @MichaReiser on `crates/ruff_benchmark/benches/ty.rs`:371 on 2025-06-17 16:59_

```suggestion
```

---

_@MichaReiser reviewed on 2025-06-17 17:00_

---

_Review comment by @MichaReiser on `crates/ruff_benchmark/benches/ty.rs`:415 on 2025-06-17 17:00_

```suggestion
```

---

_@MichaReiser reviewed on 2025-06-17 17:00_

---

_Review comment by @MichaReiser on `crates/ruff_benchmark/benches/ty.rs`:430 on 2025-06-17 17:00_

```suggestion
```

---

_@MichaReiser reviewed on 2025-06-17 17:00_

---

_Review comment by @MichaReiser on `crates/ruff_benchmark/benches/ty.rs`:445 on 2025-06-17 17:00_

```suggestion
```

---

_Review comment by @MichaReiser on `crates/ruff_benchmark/benches/ty_walltime.rs`:19 on 2025-06-17 17:00_

```suggestion
    // ╰─ colour_science 153.7 ms       │ 2.177 s       │ 2.106 s       │ 1.921 s       │ 10      │ 10
```

---

_@MichaReiser reviewed on 2025-06-17 17:00_

---

_@MichaReiser reviewed on 2025-06-17 17:01_

---

_Review comment by @MichaReiser on `crates/ruff_benchmark/benches/ty_walltime.rs`:38 on 2025-06-17 17:01_

```suggestion
```

---

_@MichaReiser reviewed on 2025-06-17 17:01_

---

_Review comment by @MichaReiser on `crates/ruff_benchmark/benches/ty.rs`:358 on 2025-06-17 17:01_

```suggestion
```

---

_@MichaReiser reviewed on 2025-06-17 17:01_

---

_Review comment by @MichaReiser on `crates/ruff_benchmark/benches/ty_walltime.rs`:54 on 2025-06-17 17:01_

```suggestion
```

---

_Review comment by @AlexWaygood on `crates/ruff_benchmark/benches/ty_walltime.rs`:19 on 2025-06-17 17:05_

unclosed code fence :-)

````suggestion
    // if we run the benchmarks multi-threaded:
    // ```
    // ty_walltime        fastest       │ slowest       │ median        │ mean          │ samples │ iters
    // ╰─ colour_science 153.7 ms       │ 2.177 s       │ 2.106 s       │ 1.921 s       │ 10      │ 10
    // ```
````

---

_@AlexWaygood reviewed on 2025-06-17 17:05_

---

_Review comment by @sharkdp on `crates/ruff_benchmark/benches/ty.rs`:414 on 2025-06-18 09:30_

Why do we have this? As some kind of low-fidelity filter that we do "actual type checking work" on the project? I'm not opposed to it, but it might require updating the thresholds from time to time?

---

_Review comment by @sharkdp on `crates/ruff_benchmark/src/real_world_projects.rs`:205 on 2025-06-18 09:34_

Does this save time? Or could it maybe be omitted entirely, assuming that the subsequent checkout will be fast anyway?

---

_Review comment by @sharkdp on `crates/ruff_benchmark/src/real_world_projects.rs`:220 on 2025-06-18 09:37_

Minor stylistic suggestion: I think you could write these kinds of tests using the `ensure!` macro:
```suggestion
    anyhow::ensure!(output.status.success(),
        "Git checkout of commit {} failed: {}",
        commit,
        String::from_utf8_lossy(&output.stderr)
    );
```

---

_Review comment by @sharkdp on `crates/ruff_benchmark/src/real_world_projects.rs`:296 on 2025-06-18 09:40_

Minor: I think I find this more distracting than helpful?
```suggestion
    // Create an isolated virtual environment for installing the project's dependencies
```

---

_Review comment by @sharkdp on `crates/ruff_benchmark/src/real_world_projects.rs`:301 on 2025-06-18 09:42_

I think you could also call `uv venv` with the `--allow-existing` option instead of checking the existence

---

_Review comment by @sharkdp on `crates/ruff_benchmark/src/real_world_projects.rs`:117 on 2025-06-18 09:48_

Minor naming suggestion: I used `InstalledProject` for the same concept in a related tool. "setup" has the drawback that the past-tense form is the same, so it's not clear if that project has already been set up, or if it's referring to the actual project setup.

---

_@sharkdp approved on 2025-06-18 09:49_

Very much looking forward to this — thank you!

I didn't really review the project selection as such. Having this in place is the important part. Iterating on which projects we want to use is probably a continuous process.

---

_Review comment by @MichaReiser on `crates/ruff_benchmark/benches/ty.rs`:414 on 2025-06-18 09:56_

Yeah, it's more a gap-stop to ensure the dependencies are correctly installed and we actually check any files (although that would probably visible now :)) 

---

_@MichaReiser reviewed on 2025-06-18 09:56_

---

_Comment by @AlexWaygood on 2025-06-18 10:06_

> I picked the larger projects mainly on @AlexWaygood suggestions (thank you!)

I was quite confused for a while because I couldn't see where these larger projects were being reported on codspeed, but I eventually figured out that you need to click the "wall time" tab here:

<img width="1189" alt="Screenshot 2025-06-18 at 11 02 40" src="https://github.com/user-attachments/assets/31313492-11bf-4037-9715-cf9379ec4784" />

I assume if we significantly regress one of the walltime benchmarks, codspeed will post a comment on the PR to tell us off about it, the same as it does with the instrumented benchmarks?

I don't feel like I have a good understanding of what the practical difference is between using a walltime runner and using an instrumented runner, but I trust you that this is the correct choice for the larger projects :-)

---

_Comment by @AlexWaygood on 2025-06-18 10:14_

> The smaller projects are picked arbitrarily. I'm happy to change them if someone has other suggestions.

They seem reasonable to me (definitely good enough to land now). anyio and attrs are both very popular projects, and I'm pretty sure hydra-zen has caused performance issues for type checkers in the past!

@hauntsaninja or @erictraut -- I don't suppose either of you have suggestions for smaller projects that have caused performance issues for mypy or pyright, which would be good for us to include in our benchmark suite? 

---

_Comment by @sharkdp on 2025-06-18 10:33_

> I don't feel like I have a good understanding of what the practical difference is between using a walltime runner and using an instrumented runner, but I trust you that this is the correct choice for the larger projects :-)

My understanding is the following. The benchmarks that we ran on codspeed so far were "instrumented". What this means is that codspeed runs the executable using valgrind (i.e. on a virtual CPU). Instead of measuring actual time, valgrind counts CPU instructions and then converts them to a time (using some arbitrary but fixed clock speed). This approach has the benefit that it's noise-free, and the benchmark only needs to run once (but on a virtual CPU, which is much much slower than on an actual CPU). The big disadvantage is that you don't see the real impact of IO operations. I believe codspeed makes some assumptions about the time it takes for various syscalls to complete (how long does a `read` operation take if it reads x bytes, …), but this is just guesswork.

Walltime runners use much less magic. They execute the benchmark on an actual CPU and really measure wall clock time. This process is inherently noisy, even if they certainly have measures to reduce noise. That's why the benchmark should ideally run multiple times to gather some statistics. The disadvantage here is that this is harder to measure accurately and reliably. The advantage is that it includes IO operations and reflects the actual runtime that a user would see more directly.

---

_Comment by @MichaReiser on 2025-06-18 11:14_

Thank's @sharkdp for the very detailed comparison. In this case, the only reason for using walltime is because the instrumented runners are simply too slow. At the other hand, having some wall-time runners is nice (and e.g revealed a salsa performance issue when using multiple dbs).

---

_@MichaReiser reviewed on 2025-06-18 11:15_

---

_Review comment by @MichaReiser on `crates/ruff_benchmark/benches/ty.rs`:414 on 2025-06-18 11:15_

I also think that it will be useful when bumping the commit. A sudden increase could indicate that the dependencies need to be updated.

---

_Review comment by @MichaReiser on `crates/ruff_benchmark/src/real_world_projects.rs`:205 on 2025-06-18 11:15_

Probably not. I suspect that Claude tried to be fancy here

---

_@MichaReiser reviewed on 2025-06-18 11:15_

---

_Comment by @MichaReiser on 2025-06-18 11:28_

I'll go ahead and merge this. We can easily change the projects in folllow up PRs and I'm also curious to see how stable/flaky the benchmarks are (which practice will show)

---

_Merged by @MichaReiser on 2025-06-18 11:41_

---

_Closed by @MichaReiser on 2025-06-18 11:41_

---

_Branch deleted on 2025-06-18 11:41_

---

_Comment by @MichaReiser on 2025-06-18 11:41_

The CI error seems unrelated to this PR.

---

_Comment by @hauntsaninja on 2025-06-20 18:01_

I don't know about smaller, but you could try some of:

colour-science/colour
home-assistant/core
pandas-dev/pandas
sympy/sympy
pydantic/pydantic
Rapptz/discord.py
FasterSpeeding/Tanjun
python/mypy

---

_Comment by @AlexWaygood on 2025-06-20 18:06_

> FasterSpeeding/Tanjun

Ah yeah, this one in particular might add some coverage that we don't currently have IIRC @MichaReiser -- they do a lot of stuff with `ParamSpec`s IIRC

---
