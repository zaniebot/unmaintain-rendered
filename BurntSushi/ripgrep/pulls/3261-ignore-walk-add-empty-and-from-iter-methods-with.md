```yaml
number: 3261
title: "ignore/walk: add 'empty' and 'from_iter' methods, with tests"
type: pull_request
state: open
author: jar-of-salt
labels: []
assignees: []
base: master
head: new-walk-constructors
created_at: 2026-01-09T03:15:11Z
updated_at: 2026-01-09T03:16:46Z
url: https://github.com/BurntSushi/ripgrep/pull/3261
synced_at: 2026-01-12T18:23:15Z
```

# ignore/walk: add 'empty' and 'from_iter' methods, with tests

---

_@jar-of-salt_

## Motivation

Closes #1761

Goal is to provide an expanded interface for creating `WalkBuilder` instances, specifically to avoid the pattern of having to call `add` repeatedly when you already know you have several paths you want to traverse.

E.g. courtesy of @ssokolow in the above issue
```rust
if let Some(path1) = opts.inpath.pop() {
    let mut builder = WalkBuilder::new(path1);
    for path in opts.inpath {
        builder.add(path);
    }
    for result in builder.build() {
        // TODO: Process each file
    }
}
```

Now you can just proceed as follows:
```rust
let builder = WalkBuilder::from_iter(opts.inpath);
```

## Details

I have added the following constructor methods for `WalkBuilder`:
- `empty`: produces an empty walk, which, when iterated before any calls to `add`, produces exactly zero items
- `from_iter`: produces a walk with the paths from the given iterator. If called with an empty iterator, produces the same result as `empty`

---
