---
number: 4956
title: More heap allocations than structopt
type: issue
state: closed
author: dullbananas
labels: []
assignees: []
created_at: 2023-06-08T03:03:02Z
updated_at: 2023-06-09T15:13:41Z
url: https://github.com/clap-rs/clap/issues/4956
synced_at: 2026-01-07T13:12:20-06:00
---

# More heap allocations than structopt

---

_Issue opened by @dullbananas on 2023-06-08 03:03_

I tested the heap usage of wasm-pack when using it to compile wasm_game_of_life before and after upgrading from structopt to clap 4.3. The total allocated bytes increased by 22%, and the maximum allocated bytes (t-gmax) increased by 57%.

[dhat-heap.zip](https://github.com/clap-rs/clap/files/11683642/dhat-heap.zip)

---

_Comment by @dullbananas on 2023-06-08 03:04_

Dhat files can be viewed here https://nnethercote.github.io/dh_view/dh_view.html

---

_Comment by @epage on 2023-06-08 08:22_

Could you include a reproduction case?

---

_Comment by @dullbananas on 2023-06-08 14:21_

```
git clone https://github.com/dullbananas/wasm-pack.git
cd wasm-pack
# switch to version with structopt
git checkout dc9e99c
# clone inside of wasm-pack
git clone https://github.com/rustwasm/wasm_game_of_life.git v
cd v
git checkout fc35b7c
cd ..
# run twice the first time because the very first run of wasm-pack does more heap allocations
cargo run --release --features "dhat-heap" -- build --release v
cargo run --release --features "dhat-heap" -- build --release v
# switch to version with structopt
git checkout 0692377
# this will overwrite dhat-heap.json
cargo run --release --features "dhat-heap" -- build --release v
```

---

_Comment by @epage on 2023-06-08 14:25_

Also, could you describe the end-user impact.  Is this noticeable in any way?  For example, in #4774 we are discussing the impact of clap initialization time on the full startup time of the application, affecting the more interactive experience of the user.

---

_Comment by @dullbananas on 2023-06-08 22:31_

I found no measurable difference in initialization time. To measure it, I added this code after `let args = Cli::parse()`:

```
dbg!(args); // prevent parsing from being optimized away
return Ok(());
```

And I ran this 3 times:

```
time ./target/*/release/wasm-pack build --release v
```

Structopt: 0.158s, 0.010s, 0.008s

Clap: 0.168s, 0.011s, 0.009s

I also measured the binary size.

Structopt: 12,029,348 bytes

Clap: 12,160,608 bytes (1.1% increase)

And I used `dbg!(dhat::HeapStats::get().curr_bytes)` to measure the heap usage after parsing is done.

Structopt: 1130 bytes

Clap: 978 bytes

---

_Comment by @epage on 2023-06-09 15:13_

Sounds like the end-user impact of these increased allocations is negligible which has me leaning towards closing this.  If you have a reason we should actively focus on this, let us know!

I suspect some of our work towards #2037 and #1365 might help.  We are exploring making more settings of `Command` and `Arg` runtime defined (e.g. #4833).  This will decrease the size of `Command` and `Arg` though it means there will be extra allocations for any of these runtime fields used.

#4959 would be the biggest change for reducing allocations as it will make it so we don't even instantiate `Arg`s that are never used and we wouldn't be initializing some of the fields of unused `Command`s.


---

_Closed by @epage on 2023-06-09 15:13_

---
