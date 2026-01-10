```yaml
number: 2059
title: "Proposal: Project structure"
type: issue
state: closed
author: MichaReiser
labels:
  - core
assignees: []
created_at: 2023-01-21T13:25:54Z
updated_at: 2023-04-06T17:50:07Z
url: https://github.com/astral-sh/ruff/issues/2059
synced_at: 2026-01-10T11:09:45Z
```

# Proposal: Project structure

---

_Issue opened by @MichaReiser on 2023-01-21 13:25_

Hi 

I'm new to ruff and started exploring its code base. I noticed during the exploration that I struggled with the many different folders. There's a root `src` folder, but there are Rust crates on the same level. 

I previously worked at [Rome](https://github.com/rome/tools) and found the structure intuitive (obviously, I'm very biased). Rome's folder structure is inspired by [rust-analyzer](https://github.com/rust-lang/rust-analyzer). 

* `crates`: Contains all Rust crates that are part of the shipped `ruff` product.
* `xtask`: Code for tasks like code-gen, scripts ran as part of CI, etc. 
* Tests were placed in the `tests` folder of the relevant crates OR in the `xtask` directory

Applied to ruff, the structure would change as follow:

* `src` -> `crates/ruff/src` (and other files related to the `ruff` crate)
* `flake8_to_ruff` -> `crates/flake8_to_ruff`
* `ruff_cli` -> `crates/ruff_cli`
* `ruff_dev` -> `xtask/ruff_dev`
* `ruff_macros` -> `crates/ruff_macros`

I would need to take a closer look at the (assumingly) tests in the `resources` directory to decide where to place them best.

The main idea is to reduce the top-level folders. 



---

_Comment by @charliermarsh on 2023-01-21 14:45_

Welcome :)

I'm a fan of this project structure and this proposal more broadly.

For posterity: there's a little bit of discussion around structure [here](https://github.com/charliermarsh/ruff/pull/1816#discussion_r1068261414) and I proposed something kinda similar [here](https://github.com/charliermarsh/ruff/issues/1547#issuecomment-1379110720) (both very hard to find, took me a while to search through the repo).

The `xtask` thing is interesting -- is that a common convention? Something specific to `rust-analyzer`? Right now, we have it setup such that you can run `cargo dev generate-all` to execute the `ruff_dev` binary, which feels ok from a workflow perspective, although I'm open to changing it if there's precedent from other crates.


---

_Label `core` added by @charliermarsh on 2023-01-21 14:46_

---

_Comment by @not-my-profile on 2023-01-22 04:09_

Personally I'd rather keep the crates in the top-level directory. I think it's nice to nice to see them immediately on the main repository page instead of just `crates/`.  But I can see how having `src` on a different level than the `src` directories of the other crates can be confusing ... that could be fixed by just moving `src` to `ruff/src` (and this would also let us have a different README for the library than for the main ruff project).

---

_Comment by @MichaReiser on 2023-01-22 09:32_

I found the [blog post](https://matklad.github.io/2021/08/22/large-rust-workspaces.html) from matklad explaining the motivation behind the folder structure in more detail.

> The xtask thing is interesting -- is that a common convention? 

I don't think it is but it refers to the approach that matlab describes [here](https://github.com/matklad/cargo-xtask). The idea is that you write your "scripts" in rust and use cargo to run them. That's why `cargo dev generate-all` should continue to work (it may be worth renaming the command to `codegen`). An alternative to `xtask` is [`just`](https://github.com/casey/just) which is more powerful. 

> Personally I'd rather keep the crates in the top-level directory

That should work well for now, but I'm concerned that it doesn't scale well with more crates, resulting in many top-level folders (Rome has 36 crates after 1.5 years). Many crates are desired for sane [compile times](https://matklad.github.io/2021/09/04/fast-rust-builds.html#Compilation-Model-Crates).

---

_Comment by @not-my-profile on 2023-01-22 10:17_

> I'm concerned that it doesn't scale well with more crates, resulting in many top-level folders (Rome has 36 crates after 1.5 years)

I postulate that:

1. rule implementations are the part of ruff that has, is and will be what most ruff contributors contribute to, and that
2. rule implementations are the part of ruff that will continue to be expanded and improved while the rest of ruff will stabilize over time in comparison.

I therefore believe that we should make our rule implementations easy to find by just looking at the top-level directory and was concerned that moving our rule implementations to a `crates/` directory with an ever growing number of crates would make them harder to find.

However I just had the realization that this could be addressed by simply creating a symbolic link in the top-level directory. So I'd be alright with a structure like:

```
.
├── crates/
│   ├── ruff/
│   │   └── src/
│   │       └── rules/
│   ├── ruff_cli/
│   └── ruff_dev/
└── rules -> crates/ruff/src/rules/
```

---

_Comment by @charliermarsh on 2023-04-06 17:48_

@MichaReiser - Do you think it's worth closing this for now?

---

_Comment by @MichaReiser on 2023-04-06 17:50_

Yeah. The `ruff` crate is still large but I think we can split it up incrementally. 

---

_Closed by @MichaReiser on 2023-04-06 17:50_

---
