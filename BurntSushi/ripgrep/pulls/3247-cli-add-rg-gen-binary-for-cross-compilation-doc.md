```yaml
number: 3247
title: "cli: add rg-gen binary for cross-compilation doc generation"
type: pull_request
state: closed
author: OctopusET
labels: []
assignees: []
base: master
head: rg-gen
created_at: 2025-12-16T08:36:13Z
updated_at: 2025-12-16T14:14:30Z
url: https://github.com/BurntSushi/ripgrep/pull/3247
synced_at: 2026-01-12T18:23:15Z
```

# cli: add rg-gen binary for cross-compilation doc generation

---

_@OctopusET_

When cross-compiling ripgrep, distributions need to generate man pages and shell completions. Currently this requires running the target binary, which is not possible on the host machine.

For example, Gentoo's ebuild does this at install time:
https://github.com/gentoo/gentoo/blob/master/sys-apps/ripgrep/ripgrep-15.1.0.ebuild#L105

This fails during cross-compilation because rg is built for target, not host:
```sh
$(cargo_target_dir)/rg --generate man
```

### This PR adds rg-gen, a minimal binary that only generates docs:

```sh
# Build minimal generator for host
cargo build --bin rg-gen --no-default-features

# Or use the cargo alias
cargo rg-gen
```

```sh
# Generate docs
./rg-gen man > rg.1
./rg-gen complete-bash > rg.bash
./rg-gen complete-zsh > _rg
./rg-gen complete-fish > rg.fish
```

## Changes

- Add `cli` feature to gate CLI-only modules
- Add `rg-gen` binary that shares code with `rg`
- Add cargo alias for convenience

## Notes

- `cargo run` still runs `rg` by default
- All existing tests pass
- Output is identical to `rg --generate`
- https://github.com/OctopusET/ripgrep/tree/rg-gen-crate different approach using separate crate


---

_Comment by @BurntSushi on 2025-12-16 13:54_

I'm not sure this is really something I want to maintain. In particular, it's unclear to me what alternatives have been pursued. If you're cross-compiling, why not just build ripgrep on the host and generate docs for it? If you absolutely must run it on the target, then you can do what ripgrep's CI does and use qemu.

---

_Comment by @OctopusET on 2025-12-16 13:57_

> why not just build ripgrep on the host and generate docs for it?

It's totally possible. But what I did is, made the ripgrep able to build with only doc and *sh completion generate parts

> If you absolutely must run it on the target, then you can do what ripgrep's CI does and use qemu.

It won't run on target, it's for the host. So we can build the docs and *sh completions

---

_Comment by @OctopusET on 2025-12-16 13:59_

I just wanted to make the host binary as small as possible when we crosscompile

---

_Comment by @BurntSushi on 2025-12-16 14:10_

I don't think this is worth the effort to maintain, sorry. I don't want to deal with two binaries. Test two binaries. And have the conditional compilation cfgs all over.

---

_Closed by @BurntSushi on 2025-12-16 14:10_

---

_Comment by @OctopusET on 2025-12-16 14:14_

no worries, thank you for reviewing

---
