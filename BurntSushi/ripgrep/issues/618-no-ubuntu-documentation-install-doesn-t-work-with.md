```yaml
number: 618
title: "No Ubuntu Documentation?  Install doesn't work with `cargo` either"
type: issue
state: closed
author: niftylettuce
labels: []
assignees: []
created_at: 2017-09-30T21:43:01Z
updated_at: 2018-04-23T20:20:58Z
url: https://github.com/BurntSushi/ripgrep/issues/618
synced_at: 2026-01-12T16:13:22Z
```

# No Ubuntu Documentation?  Install doesn't work with `cargo` either

---

_@niftylettuce_

It seems quite odd that you'd have OS X, Windows, but not Ubuntu instructions for installation?

This leaves end user digging through GitHub issues to find nearly no docs other than a recommended `cargo` install.  I don't think anyone wants to compile from source either.

Am I wrong or is there no clear way to install on Ubuntu?

Also, you make a note about using `curl https://sh.rustup.rs -sSf | sh` via <https://github.com/BurntSushi/ripgrep/issues/10#issuecomment-301243991>, but that doesn't work either if the user already has rust installed on the OS.  Also attempting to uninstall Rust is a nightmare, see <https://www.reddit.com/r/rust/comments/5yk0hc/does_cleanly_uninstalling_rust_on_linux_have_to/>.

This thread in general is so polluted with comments and confusion as well <https://github.com/BurntSushi/ripgrep/issues/10>.

Also, if we tell users to use `cargo` to install it, perhaps that should be in the `README` in a clearly defined "Ubuntu Install "section?.  There's a comment here to do that, but it's from January... <https://github.com/BurntSushi/ripgrep/issues/313#issuecomment-271984111>

Related: https://github.com/BurntSushi/ripgrep/issues/355

Full output of `cargo` error I received:

I'm installing on Ubuntu 14.04.5 LTS (on a stock Digital Ocean droplet).  I had a bug trying to use `cargo` to install `ripgrep`.

```sh
sudo cargo install ripgrep
    Updating registry `https://github.com/rust-lang/crates.io-index`
   Compiling utf8-ranges v1.0.0
   Compiling ansi_term v0.9.0
   Compiling bytecount v0.1.7
   Compiling lazy_static v0.2.9
   Compiling crossbeam v0.2.10
   Compiling vec_map v0.8.0
   Compiling void v1.0.2
   Compiling unicode-width v0.1.4
   Compiling regex-syntax v0.4.1
   Compiling libc v0.2.31
   Compiling same-file v0.1.3
   Compiling walkdir v1.0.7
   Compiling log v0.3.8
   Compiling memchr v1.0.1
   Compiling atty v0.2.3
   Compiling env_logger v0.4.3
   Compiling strsim v0.6.0
   Compiling termcolor v0.3.3
   Compiling unreachable v1.0.0
   Compiling thread_local v0.3.4
   Compiling term_size v0.3.0
   Compiling cfg-if v0.1.2
   Compiling encoding_rs v0.6.11
   Compiling textwrap v0.8.0
   Compiling num_cpus v1.7.0

   Compiling bitflags v0.9.1
   Compiling clap v2.26.2
   Compiling memmap v0.5.2
   Compiling fnv v1.0.5
   Compiling aho-corasick v0.6.3
   Compiling regex v0.2.2
   Compiling grep v0.1.6
   Compiling globset v0.2.0
   Compiling ignore v0.2.2
   Compiling ripgrep v0.6.0
error: linking with `cc` failed: exit code: 1
  |
  = note: "cc" "-Wl,--as-needed" "-Wl,-z,noexecstack" "-m64" "-L" "/usr/lib/rustlib/x86_64-unknown-linux-gnu/lib" "/tmp/cargo-install.g4egpqZk0Ufd/release/build/ripgrep-bc6ad105e676df2b/build_script_build-bc6ad105e676df2b.0.o" "-o" "/tmp/cargo-install.g4egpqZk0Ufd/release/build/ripgrep-bc6ad105e676df2b/build_script_build-bc6ad105e676df2b" "-Wl,--gc-sections" "-pie" "-Wl,-O1" "-nodefaultlibs" "-L" "/tmp/cargo-install.g4egpqZk0Ufd/release/deps" "-L" "/usr/lib/rustlib/x86_64-unknown-linux-gnu/lib" "-Wl,-Bstatic" "-Wl,-Bdynamic" "/tmp/cargo-install.g4egpqZk0Ufd/release/deps/libclap-a5de31793dbfafe8.rlib" "/tmp/cargo-install.g4egpqZk0Ufd/release/deps/libatty-b6841561eaa9990b.rlib" "/tmp/cargo-install.g4egpqZk0Ufd/release/deps/libstrsim-9194d0a0d94f76d9.rlib" "/tmp/cargo-install.g4egpqZk0Ufd/release/deps/libbitflags-e3c5d2be25b5a586.rlib" "/tmp/cargo-install.g4egpqZk0Ufd/release/deps/liblazy_static-8823bb1423f1ad41.rlib" "/tmp/cargo-install.g4egpqZk0Ufd/release/deps/libansi_term-3ad49c05b79382a2.rlib" "/tmp/cargo-install.g4egpqZk0Ufd/release/deps/libvec_map-220b10ad4267d2a8.rlib" "/tmp/cargo-install.g4egpqZk0Ufd/release/deps/libtextwrap-20686c873a7ca803.rlib" "/tmp/cargo-install.g4egpqZk0Ufd/release/deps/libterm_size-72593745bbe4ef3e.rlib" "/tmp/cargo-install.g4egpqZk0Ufd/release/deps/liblibc-69a7df4df6b083eb.rlib" "/tmp/cargo-install.g4egpqZk0Ufd/release/deps/libunicode_width-ac36c60662f8c788.rlib" "/usr/lib/rustlib/x86_64-unknown-linux-gnu/lib/libstd-570da8f8.rlib" "/usr/lib/rustlib/x86_64-unknown-linux-gnu/lib/libpanic_unwind-570da8f8.rlib" "/usr/lib/rustlib/x86_64-unknown-linux-gnu/lib/libunwind-570da8f8.rlib" "/usr/lib/rustlib/x86_64-unknown-linux-gnu/lib/librand-570da8f8.rlib" "/usr/lib/rustlib/x86_64-unknown-linux-gnu/lib/libcollections-570da8f8.rlib" "/usr/lib/rustlib/x86_64-unknown-linux-gnu/lib/liballoc-570da8f8.rlib" "/usr/lib/rustlib/x86_64-unknown-linux-gnu/lib/liballoc_jemalloc-570da8f8.rlib" "/usr/lib/rustlib/x86_64-unknown-linux-gnu/lib/liblibc-570da8f8.rlib" "/usr/lib/rustlib/x86_64-unknown-linux-gnu/lib/libstd_unicode-570da8f8.rlib" "/usr/lib/rustlib/x86_64-unknown-linux-gnu/lib/libcore-570da8f8.rlib" "/usr/lib/rustlib/x86_64-unknown-linux-gnu/lib/libcompiler_builtins-570da8f8.rlib" "-l" "util" "-l" "dl" "-l" "pthread" "-l" "gcc_s" "-l" "pthread" "-l" "c" "-l" "m" "-l" "rt" "-l" "util"
  = note: /usr/bin/ld: /usr/lib/rustlib/x86_64-unknown-linux-gnu/lib/liballoc_jemalloc-570da8f8.rlib(jemalloc.pic.o): unrecognized relocation (0x2a) in section `.text.unlikely.malloc_conf_init'
/usr/bin/ld: final link failed: Bad value
collect2: error: ld returned 1 exit status


error: aborting due to previous error

error: failed to compile `ripgrep v0.6.0`, intermediate artifacts can be found at `/tmp/cargo-install.g4egpqZk0Ufd`
```

---

_Comment by @niftylettuce on 2017-09-30 21:53_

Also I figured out how to get rust reinstalled...

```sh
sudo apt-get -y remove rustc
sudo apt-y autoremove
sudo -i
curl https://sh.rustup.rs -sSf | s
```

Then `sudo apt-get install cargo && sudo cargo install ripgrep` again.

Reference: https://www.howtodojo.com/2017/09/install-rust-ubuntu-16-04/

Although this still yields an error:

```sh
sudo cargo install ripgrep
    Updating registry `https://github.com/rust-lang/crates.io-index`
   Compiling regex-syntax v0.4.1
   Compiling fnv v1.0.5
   Compiling cfg-if v0.1.2
   Compiling strsim v0.6.0
   Compiling encoding_rs v0.6.11
   Compiling crossbeam v0.2.10
   Compiling ansi_term v0.9.0
   Compiling bitflags v0.9.1
   Compiling lazy_static v0.2.9
   Compiling termcolor v0.3.3
   Compiling vec_map v0.8.0
   Compiling bytecount v0.1.7
   Compiling libc v0.2.31
   Compiling utf8-ranges v1.0.0
   Compiling same-file v0.1.3
   Compiling walkdir v1.0.7
   Compiling term_size v0.3.0
   Compiling atty v0.2.3
   Compiling memmap v0.5.2
   Compiling memchr v1.0.1
   Compiling aho-corasick v0.6.3
   Compiling void v1.0.2
   Compiling unreachable v1.0.0
   Compiling thread_local v0.3.4
   Compiling num_cpus v1.7.0
   Compiling log v0.3.8
   Compiling env_logger v0.4.3
   Compiling unicode-width v0.1.4
   Compiling textwrap v0.8.0
   Compiling clap v2.26.2
   Compiling regex v0.2.2
   Compiling grep v0.1.6
   Compiling globset v0.2.0
   Compiling ignore v0.2.2

   Compiling ripgrep v0.6.0
error: linking with `cc` failed: exit code: 1
  |
  = note: "cc" "-Wl,--as-needed" "-Wl,-z,noexecstack" "-m64" "-L" "/usr/lib/rustlib/x86_64-unknown-linux-gnu/lib" "/tmp/cargo-install.gsxfisZF01P9/release/build/ripgrep-bc6ad105e676df2b/build_script_build-bc6ad105e676df2b.0.o" "-o" "/tmp/cargo-install.gsxfisZF01P9/release/build/ripgrep-bc6ad105e676df2b/build_script_build-bc6ad105e676df2b" "-Wl,--gc-sections" "-pie" "-Wl,-O1" "-nodefaultlibs" "-L" "/tmp/cargo-install.gsxfisZF01P9/release/deps" "-L" "/usr/lib/rustlib/x86_64-unknown-linux-gnu/lib" "-Wl,-Bstatic" "-Wl,-Bdynamic" "/tmp/cargo-install.gsxfisZF01P9/release/deps/libclap-a5de31793dbfafe8.rlib" "/tmp/cargo-install.gsxfisZF01P9/release/deps/libatty-b6841561eaa9990b.rlib" "/tmp/cargo-install.gsxfisZF01P9/release/deps/libstrsim-9194d0a0d94f76d9.rlib" "/tmp/cargo-install.gsxfisZF01P9/release/deps/libbitflags-e3c5d2be25b5a586.rlib" "/tmp/cargo-install.gsxfisZF01P9/release/deps/liblazy_static-8823bb1423f1ad41.rlib" "/tmp/cargo-install.gsxfisZF01P9/release/deps/libansi_term-3ad49c05b79382a2.rlib" "/tmp/cargo-install.gsxfisZF01P9/release/deps/libvec_map-220b10ad4267d2a8.rlib" "/tmp/cargo-install.gsxfisZF01P9/release/deps/libtextwrap-20686c873a7ca803.rlib" "/tmp/cargo-install.gsxfisZF01P9/release/deps/libterm_size-72593745bbe4ef3e.rlib" "/tmp/cargo-install.gsxfisZF01P9/release/deps/liblibc-69a7df4df6b083eb.rlib" "/tmp/cargo-install.gsxfisZF01P9/release/deps/libunicode_width-ac36c60662f8c788.rlib" "/usr/lib/rustlib/x86_64-unknown-linux-gnu/lib/libstd-570da8f8.rlib" "/usr/lib/rustlib/x86_64-unknown-linux-gnu/lib/libpanic_unwind-570da8f8.rlib" "/usr/lib/rustlib/x86_64-unknown-linux-gnu/lib/libunwind-570da8f8.rlib" "/usr/lib/rustlib/x86_64-unknown-linux-gnu/lib/librand-570da8f8.rlib" "/usr/lib/rustlib/x86_64-unknown-linux-gnu/lib/libcollections-570da8f8.rlib" "/usr/lib/rustlib/x86_64-unknown-linux-gnu/lib/liballoc-570da8f8.rlib" "/usr/lib/rustlib/x86_64-unknown-linux-gnu/lib/liballoc_jemalloc-570da8f8.rlib" "/usr/lib/rustlib/x86_64-unknown-linux-gnu/lib/liblibc-570da8f8.rlib" "/usr/lib/rustlib/x86_64-unknown-linux-gnu/lib/libstd_unicode-570da8f8.rlib" "/usr/lib/rustlib/x86_64-unknown-linux-gnu/lib/libcore-570da8f8.rlib" "/usr/lib/rustlib/x86_64-unknown-linux-gnu/lib/libcompiler_builtins-570da8f8.rlib" "-l" "util" "-l" "dl" "-l" "pthread" "-l" "gcc_s" "-l" "pthread" "-l" "c" "-l" "m" "-l" "rt" "-l" "util"
  = note: /usr/bin/ld: /usr/lib/rustlib/x86_64-unknown-linux-gnu/lib/liballoc_jemalloc-570da8f8.rlib(jemalloc.pic.o): unrecognized relocation (0x2a) in section `.text.unlikely.malloc_conf_init'
/usr/bin/ld: final link failed: Bad value
collect2: error: ld returned 1 exit status


error: aborting due to previous error

error: failed to compile `ripgrep v0.6.0`, intermediate artifacts can be found at `/tmp/cargo-install.gsxfisZF01P9`

Caused by:
  Could not compile `ripgrep`.
```

---

_Comment by @BurntSushi on 2017-09-30 23:55_

I don't know what's going on with your compile, but you should make sure
you have a new enough version of Rust.

Have you tried downloading a precompiled binary that the README mentions?

To answer your question, there are no Ubuntu install instructions because
I'm not aware of any Ubuntu package repos that provide ripgrep.

On Sep 30, 2017 5:53 PM, "niftylettuce" <notifications@github.com> wrote:

> Also I figured out how to get rust reinstalled...
>
> sudo apt-get -y remove rustc
> sudo apt-y autoremove
> sudo -i
> curl https://sh.rustup.rs -sSf | s
>
> Then sudo apt-get install cargo && sudo cargo install ripgrep again.
>
> Reference: https://www.howtodojo.com/2017/09/install-rust-ubuntu-16-04/
>
> Although this still yields an error:
>
> sudo cargo install ripgrep
>     Updating registry `https://github.com/rust-lang/crates.io-index`
>    Compiling regex-syntax v0.4.1
>    Compiling fnv v1.0.5
>    Compiling cfg-if v0.1.2
>    Compiling strsim v0.6.0
>    Compiling encoding_rs v0.6.11
>    Compiling crossbeam v0.2.10
>    Compiling ansi_term v0.9.0
>    Compiling bitflags v0.9.1
>    Compiling lazy_static v0.2.9
>    Compiling termcolor v0.3.3
>    Compiling vec_map v0.8.0
>    Compiling bytecount v0.1.7
>    Compiling libc v0.2.31
>    Compiling utf8-ranges v1.0.0
>    Compiling same-file v0.1.3
>    Compiling walkdir v1.0.7
>    Compiling term_size v0.3.0
>    Compiling atty v0.2.3
>    Compiling memmap v0.5.2
>    Compiling memchr v1.0.1
>    Compiling aho-corasick v0.6.3
>    Compiling void v1.0.2
>    Compiling unreachable v1.0.0
>    Compiling thread_local v0.3.4
>    Compiling num_cpus v1.7.0
>    Compiling log v0.3.8
>    Compiling env_logger v0.4.3
>    Compiling unicode-width v0.1.4
>    Compiling textwrap v0.8.0
>    Compiling clap v2.26.2
>    Compiling regex v0.2.2
>    Compiling grep v0.1.6
>    Compiling globset v0.2.0
>    Compiling ignore v0.2.2
>
>    Compiling ripgrep v0.6.0
> error: linking with `cc` failed: exit code: 1
>   |
>   = note: "cc" "-Wl,--as-needed" "-Wl,-z,noexecstack" "-m64" "-L" "/usr/lib/rustlib/x86_64-unknown-linux-gnu/lib" "/tmp/cargo-install.gsxfisZF01P9/release/build/ripgrep-bc6ad105e676df2b/build_script_build-bc6ad105e676df2b.0.o" "-o" "/tmp/cargo-install.gsxfisZF01P9/release/build/ripgrep-bc6ad105e676df2b/build_script_build-bc6ad105e676df2b" "-Wl,--gc-sections" "-pie" "-Wl,-O1" "-nodefaultlibs" "-L" "/tmp/cargo-install.gsxfisZF01P9/release/deps" "-L" "/usr/lib/rustlib/x86_64-unknown-linux-gnu/lib" "-Wl,-Bstatic" "-Wl,-Bdynamic" "/tmp/cargo-install.gsxfisZF01P9/release/deps/libclap-a5de31793dbfafe8.rlib" "/tmp/cargo-install.gsxfisZF01P9/release/deps/libatty-b6841561eaa9990b.rlib" "/tmp/cargo-install.gsxfisZF01P9/release/deps/libstrsim-9194d0a0d94f76d9.rlib" "/tmp/cargo-install.gsxfisZF01P9/release/deps/libbitflags-e3c5d2be25b5a586.rlib" "/tmp/cargo-install.gsxfisZF01P9/release/deps/liblazy_static-8823bb1423f1ad41.rlib" "/tmp/cargo-install.gsxfisZF01P9/release/deps/libansi_term-3ad49c05b79382a2.rlib" "/tmp/cargo-install.gsxfisZF01P9/release/deps/libvec_map-220b10ad4267d2a8.rlib" "/tmp/cargo-install.gsxfisZF01P9/release/deps/libtextwrap-20686c873a7ca803.rlib" "/tmp/cargo-install.gsxfisZF01P9/release/deps/libterm_size-72593745bbe4ef3e.rlib" "/tmp/cargo-install.gsxfisZF01P9/release/deps/liblibc-69a7df4df6b083eb.rlib" "/tmp/cargo-install.gsxfisZF01P9/release/deps/libunicode_width-ac36c60662f8c788.rlib" "/usr/lib/rustlib/x86_64-unknown-linux-gnu/lib/libstd-570da8f8.rlib" "/usr/lib/rustlib/x86_64-unknown-linux-gnu/lib/libpanic_unwind-570da8f8.rlib" "/usr/lib/rustlib/x86_64-unknown-linux-gnu/lib/libunwind-570da8f8.rlib" "/usr/lib/rustlib/x86_64-unknown-linux-gnu/lib/librand-570da8f8.rlib" "/usr/lib/rustlib/x86_64-unknown-linux-gnu/lib/libcollections-570da8f8.rlib" "/usr/lib/rustlib/x86_64-unknown-linux-gnu/lib/liballoc-570da8f8.rlib" "/usr/lib/rustlib/x86_64-unknown-linux-gnu/lib/liballoc_jemalloc-570da8f8.rlib" "/usr/lib/rustlib/x86_64-unknown-linux-gnu/lib/liblibc-570da8f8.rlib" "/usr/lib/rustlib/x86_64-unknown-linux-gnu/lib/libstd_unicode-570da8f8.rlib" "/usr/lib/rustlib/x86_64-unknown-linux-gnu/lib/libcore-570da8f8.rlib" "/usr/lib/rustlib/x86_64-unknown-linux-gnu/lib/libcompiler_builtins-570da8f8.rlib" "-l" "util" "-l" "dl" "-l" "pthread" "-l" "gcc_s" "-l" "pthread" "-l" "c" "-l" "m" "-l" "rt" "-l" "util"
>   = note: /usr/bin/ld: /usr/lib/rustlib/x86_64-unknown-linux-gnu/lib/liballoc_jemalloc-570da8f8.rlib(jemalloc.pic.o): unrecognized relocation (0x2a) in section `.text.unlikely.malloc_conf_init'/usr/bin/ld: final link failed: Bad valuecollect2: error: ld returned 1 exit statuserror: aborting due to previous errorerror: failed to compile `ripgrep v0.6.0`, intermediate artifacts can be found at `/tmp/cargo-install.gsxfisZF01P9`Caused by:  Could not compile `ripgrep`.
>
> —
> You are receiving this because you are subscribed to this thread.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/618#issuecomment-333338475>,
> or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34hx1l9_RXeduw7l1SpT7xKkrgMbxks5snrhygaJpZM4Pps-z>
> .
>


---

_Comment by @niftylettuce on 2017-10-01 00:32_

I used `ack` instead.  This was way too complicated to try to install.  Just debugging alone took over an hour.

---

_Comment by @BurntSushi on 2017-10-01 00:51_

The README explains how to get precompiled binaries. Otherwise there's not much else to be done with this issue. Thanks for the report.

---

_Closed by @BurntSushi on 2017-10-01 00:51_

---

_Comment by @niftylettuce on 2017-10-01 00:52_

Why are you closing this? It's still an issue.  There's zero Ubuntu docs in
README.  This is probably a huge percentage of users...

On Sep 30, 2017 8:51 PM, "Andrew Gallant" <notifications@github.com> wrote:

> The README explains how to get precompiled binaries. Otherwise there's not
> much else to be done with this issue. Thanks for the report.
>
> —
> You are receiving this because you authored the thread.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/618#issuecomment-333345776>,
> or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAf7hYLMqSGwi6oCqhUNQ7Bc72XccBr3ks5snuITgaJpZM4Pps-z>
> .
>


---

_Comment by @BurntSushi on 2017-10-01 00:58_

I already explained why there aren't Ubuntu docs. I also pointed out at least twice that there are docs pointing to precompiled binaries, which should work on any Linux distro. Adding ripgrep to more package repos (including Ubuntu) is tracked by #10, which is exactly when I'll add Ubuntu docs. Until then, there's nothing Ubuntu specific about installing ripgrep.

---

_Comment by @niftylettuce on 2017-10-01 01:01_

You should at least make a mention of Ubuntu in the Readme.  CMD+F "Ubuntu" is one of the first things power-users do.

---

_Comment by @dieggsy on 2017-10-01 01:01_

FWIW, Ubuntu 17.04 here, `cargo install ripgrep` works just fine for me. I think rust was just installed with the `curl` command on the rust-lang website. (I use nix to install these days though, but thought I'd check).

---

_Comment by @haakenlid on 2017-10-21 00:40_

It's pretty straightforward to install in Ubuntu. Here's a simple bash script for downloading the binary with wget and installing the man pages and bash completions.

```bash
#!/bin/bash
[[ $UID == 0 ]] || { echo "run as sudo to install"; exit 1; }
REPO="https://github.com/BurntSushi/ripgrep/releases/download/"
RELEASE="0.6.0/ripgrep-0.6.0-x86_64-unknown-linux-musl.tar.gz"

TMPDIR=$(mktemp -d)
cd $TMPDIR
wget -O - ${REPO}${RELEASE} | tar zxf - --strip-component=1
mv rg /usr/local/bin/
mv rg.1 /usr/local/share/man/man1/
mv complete/rg.bash-completion /usr/share/bash-completion/completions/rg
mandb
```

Edit: I've updated this script, with the current directory structure of the unzipped download and getting the latest release number from the github api, as suggested by grolongo in the next message. I use `sed` instead of `jq` to extract the tag value from the json response, since sed is installed in every linux/unix distro.

```bash
#!/bin/bash
[[ $UID == 0 ]] || { echo "run as sudo to install"; exit 1; }
REPO="BurntSushi/ripgrep"
LATEST=$(curl -sSL "https://api.github.com/repos/${REPO}/releases/latest"\
  | sed -n '/tag_name/s/[^0-9.]//gp')
FILE="${LATEST}/ripgrep-${LATEST}-x86_64-unknown-linux-musl.tar.gz"
DOWNLOAD="https://github.com/${REPO}/releases/download/${FILE}"
TMPDIR=$(mktemp -d)

cd $TMPDIR
wget --show-progress -q -O - ${DOWNLOAD} | tar zxf - --strip-component=1
cp rg /usr/local/bin/
cp doc/rg.1 /usr/local/share/man/man1/
cp complete/rg.bash /usr/share/bash-completion/completions/rg
mandb -q
```

---

_Comment by @grolongo on 2017-12-08 01:28_

@haakenlid you could even make it better by grabbing the latest version number like this:
```
RG_LATEST=$(curl -sSL "https://api.github.com/repos/BurntSushi/ripgrep/releases/latest" | jq --raw-output .tag_name)
RELEASE="${RG_LATEST}/ripgrep-${RG_LATEST}-x86_64-unknown-linux-musl.tar.gz"
```
This way you don't have to manually edit your script each time a new release is out.

---

_Comment by @niftylettuce on 2018-01-22 20:09_

I've updated @haakenlid answer in combination with yours @grolongo.  For any users wishing to install ripgrep (latest version) on Ubuntu (and can't use `snap` yet) you can do so by:

1. Install the `jq` package via `sudo apt-get install -y jq`
2. Run this command to install the latest version of ripgrep:

```sh
curl -o- https://gist.githubusercontent.com/niftylettuce/a9f0a293289eb7274516bf2cb0455796/raw/8491560fd53c98ee85a2f9916000167190113b52/install-ripgrep-on-ubuntu.sh | sudo bash
```

> Install script is located at: https://gist.github.com/niftylettuce/a9f0a293289eb7274516bf2cb0455796

---
