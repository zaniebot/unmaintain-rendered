```yaml
number: 2075
title: "Files committed to a git repo that also match `.gitignore` are not searched"
type: issue
state: closed
author: infokiller
labels:
  - duplicate
assignees: []
created_at: 2021-11-17T10:37:40Z
updated_at: 2021-11-17T14:22:22Z
url: https://github.com/BurntSushi/ripgrep/issues/2075
synced_at: 2026-01-12T16:13:24Z
```

# Files committed to a git repo that also match `.gitignore` are not searched

---

_@infokiller_

#### What version of ripgrep are you using?

Replace this text with the output of `rg --version`.

```
ripgrep 13.0.0 (rev 7ec2fd51ba)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

Github binary release.

#### What operating system are you using ripgrep on?

Linux 5.14.16-hardened1-1-hardened

#### Describe your bug.

ripgrep doesn't show files committed to a git repo if t hey are also ignored. `git ls-files` works correctly.
Seems related to #778

#### What are the steps to reproduce the behavior?

```sh
build_oci_img () {
  docker build "$@" - <<'EOF'
  FROM rust:1.56.1-slim-bullseye
  RUN apt-get update && apt-get install -y --no-install-recommends git curl bash
  RUN curl -fsSL https://github.com/BurntSushi/ripgrep/releases/download/13.0.0/ripgrep_13.0.0_amd64.deb -o ripgrep.deb && dpkg -i ripgrep.deb
  # Required for git init to work
  RUN git config --global user.email test@mail.com && git config --global user.name test
  WORKDIR /repo
  RUN git -c init.defaultBranch=master init
EOF
}

docker run --rm -i --entrypoint=bash "$(build_oci_img -q)" <<-'EOF'
  set -x
  git --version
  rg --version
  touch file
  git add file
  git commit -m test
  git ls-files
  rg --files
  echo file > .gitignore
  git ls-files
  rg --debug --files
EOF
```

#### What is the actual behavior?
```
> rg --debug --files
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 1 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1741: ignoring ./.gitignore: Ignore(IgnoreMatch(Hidden))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1741: ignoring ./.git: Ignore(IgnoreMatch(Hidden))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1741: ignoring ./file: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "file", actual: "**/file", is_whitelist: false, is_only_dir: false })))
```

#### What is the expected behavior?

Show the files if they are part of the git repo like `git ls-files does`, even if they match git ignore.

---

_Comment by @BurntSushi on 2021-11-17 14:22_

Dupe of #1530.

---

_Closed by @BurntSushi on 2021-11-17 14:22_

---

_Label `duplicate` added by @BurntSushi on 2021-11-17 14:22_

---
