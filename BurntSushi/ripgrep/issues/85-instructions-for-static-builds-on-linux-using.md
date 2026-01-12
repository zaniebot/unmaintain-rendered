```yaml
number: 85
title: Instructions for static builds on linux using master HEAD?
type: issue
state: closed
author: kaushalmodi
labels: []
assignees: []
created_at: 2016-09-25T15:25:31Z
updated_at: 2016-09-25T15:44:41Z
url: https://github.com/BurntSushi/ripgrep/issues/85
synced_at: 2026-01-12T18:23:11Z
```

# Instructions for static builds on linux using master HEAD?

---

_@kaushalmodi_

Hello,

I don't develop in rust. I installed the rust compiler just to build the latest rg binaries.

I followed this from the README.md:

```
$ git clone https://github.com/BurntSushi/ripgrep
$ cd ripgrep
$ cargo build --release
$ ./target/release/rg --version
0.1.17
```

So the installed version shows 0.1.17 ..

Is that the 0.1.17 release or is it a build using the latest HEAD of master?

Is that `rg` binary something that I can easily copy and share with other people? Or is there something else I need to do to create something like [this that you have for version 0.1.16](https://github.com/BurntSushi/ripgrep/releases/download/0.1.16/ripgrep-0.1.16-x86_64-unknown-linux-musl.tar.gz)?

Thanks!


---

_Comment by @BurntSushi on 2016-09-25 15:30_

It's the latest HEAD on master. I guess it would be better to include a commit hash in that, but I'm not quite sure how.

Also note the binary you've built probably isn't fully static. (Run `ldd` on it.) If you really want a full static build, with all the bells and whistles, you can use this:

```
RUSTFLAGS="-C target-cpu=native" rustup run nightly cargo build --target x86_64-unknown-linux-musl --release --features simd-accel
```

Note that this requires installing the [rustup tool](https://rustup.rs/) (which will manage your Rust installations and targets), and adding the `x86_64-unknown-linux-musl` target:

```
rustup target add x86_64-unknown-linux-musl
```


---

_Comment by @kaushalmodi on 2016-09-25 15:36_

> It's the latest HEAD on master. 

Thanks! I just realized that's the case because `rg --help` worked with `rg` aliased to `rg --follow` :)

> I guess it would be better to include a commit hash in that, but I'm not quite sure how.

That would be awesome! Or if the version showed something like `0.1.17dev`?

> (Run ldd on it.) 

`ldd` shows the below

```
km²~/downloads/:../ripgrep/target/release> ldd ./rg                                        09/25 11:33am
        linux-vdso.so.1 =>  (0x00007fffca9ff000)
        libdl.so.2 => /lib64/libdl.so.2 (0x00007f1a201bf000)
        libpthread.so.0 => /lib64/libpthread.so.0 (0x00007f1a1ffa2000)
        libgcc_s.so.1 => /cad/adi/apps/gnu/linux/x86_64/6/local/gcc/4.9.2/lib64/libgcc_s.so.1 (0x00007f1a1fd8c000)
        libc.so.6 => /lib64/libc.so.6 (0x00007f1a1f9f7000)
        /lib64/ld-linux-x86-64.so.2 (0x00000033e2800000)
        libm.so.6 => /lib64/libm.so.6 (0x00007f1a1f773000)
        librt.so.1 => /lib64/librt.so.1 (0x00007f1a1f56b000)
```

But as I am not a software developer, I have no idea how the `ldd` output would have looked between statically built binary and otherwise.

> Note that this requires installing the rustup tool 

Looks like I will have to pass on that for now. I am glad that I am able to build the latest binaries for now.

Thanks for the prompt replies.


---

_Closed by @kaushalmodi on 2016-09-25 15:36_

---

_Comment by @BurntSushi on 2016-09-25 15:44_

@kaushalmodi Awesome. For reference, `ldd` looks like this on a fully static executable:

```
$ curl -LO 'https://github.com/BurntSushi/ripgrep/releases/download/0.1.16/ripgrep-0.1.16-x86_64-unknown-linux-musl.tar.gz'
$ tar xf ripgrep-0.1.16-x86_64-unknown-linux-musl.tar.gz
$ cd ripgrep-0.1.16-x86_64-unknown-linux-musl
$ ldd rg
        not a dynamic executable
```


---
