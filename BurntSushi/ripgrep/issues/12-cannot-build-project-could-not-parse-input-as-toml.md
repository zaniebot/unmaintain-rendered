```yaml
number: 12
title: "Cannot build project: could not parse input as TOML"
type: issue
state: closed
author: lava
labels: []
assignees: []
created_at: 2016-09-23T13:09:40Z
updated_at: 2016-09-23T16:43:28Z
url: https://github.com/BurntSushi/ripgrep/issues/12
synced_at: 2026-01-12T18:23:11Z
```

# Cannot build project: could not parse input as TOML

---

_@lava_

On a fresh checkout, on ubuntu xenial:

```
➜  ripgrep git:(master) ./compile              
failed to parse manifest at `/usr/local/src/ripgrep/Cargo.toml`

Caused by:
  could not parse input as TOML
Cargo.toml:42:9 expected a key but found an empty string
Cargo.toml:42:9-42:10 expected `.`, but found `'`
```

On a related note, since I don't know anything about rust, is there support for doing out-of-tree builds?


---

_Comment by @BurntSushi on 2016-09-23 13:25_

What version of Rust do you have? Ripgrep needs 1.9 or newer. Just
guessing, but it looks like you have an older version.

Also, the README has the build instructions.

On Sep 23, 2016 9:09 AM, "Benno Evers" notifications@github.com wrote:

> On a fresh checkout, on ubuntu xenial:
> 
> ➜  ripgrep git:(master) ./compile
> failed to parse manifest at `/usr/local/src/ripgrep/Cargo.toml`
> 
> Caused by:
>   could not parse input as TOML
> Cargo.toml:42:9 expected a key but found an empty string
> Cargo.toml:42:9-42:10 expected `.`, but found `'`
> 
> On a related note, since I don't know anything about rust, is there
> support for doing out-of-tree builds?
> 
> —
> You are receiving this because you are subscribed to this thread.
> Reply to this email directly, view it on GitHub
> https://github.com/BurntSushi/ripgrep/issues/12, or mute the thread
> https://github.com/notifications/unsubscribe-auth/AAb34oL2Gf8BecxGWPXZqBesc_mkwPCnks5qs8-UgaJpZM4KE9aP
> .


---

_Comment by @lava on 2016-09-23 13:35_

You're correct, my version of rustc seems to be too old:

```
➜  ripgrep git:(master) apt-cache policy rustc
rustc:
  Installed: 1.7.0+dfsg1-1
  Candidate: 1.7.0+dfsg1-1
  Version table:
 *** 1.7.0+dfsg1-1 500
        500 http://archive.ubuntu.com/ubuntu xenial/universe amd64 Packages
        100 /var/lib/dpkg/status
```

That will make it quite...challenging to build a package for ubuntu ;p

I don't see out-of-tree builds being mentioned in the README.


---

_Closed by @lava on 2016-09-23 13:35_

---

_Comment by @BurntSushi on 2016-09-23 14:27_

Could you say a bit more by what you need for "out-of-tree" builds? The standard way to build Rust code is with `cargo`.


---

_Comment by @lava on 2016-09-23 16:43_

As I said, I don't really know a lot about rust, but my usual way of building an open-source project looks like this

```
cd ~/build/ripgrep
~/src/ripgrep/configure --some-weird args
make
```

or

```
cd ~/build/ripgrep
cmake ~/src/ripgrep/ -DWEIRD_OPTION=SOME_VALUE
make
```

I believe this is more or less standard, as it avoids cluttering the source tree with build artifacts, and allows to build multiple configurations from the same source tree. The build instructions in the README seem to be for an in-tree build, so I was wondering if it is possible to do an out-of-tree build.


---
