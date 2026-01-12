```yaml
number: 2265
title: Double stack
type: pull_request
state: closed
author: arriven
labels:
  - gitignore
assignees: []
base: master
head: double-stack
created_at: 2022-07-23T15:15:06Z
updated_at: 2023-07-08T13:41:17Z
url: https://github.com/BurntSushi/ripgrep/pull/2265
synced_at: 2026-01-12T18:23:14Z
```

# Double stack

---

_@arriven_

Use separate stacks for files and directories and limit the size of file stack to significantly reduce memory usage. This also noticeably speeds up execution in contexts where you have more files than directories or just large/complex files (seems like a very common case).

Running `time ./target/release/rg --no-ignore test ~/ripgrep-mockdata` on a directory with 10 subdirs with 1M files each. Each file is 10Kb in size:

- Without the changes
```
58.57user 338.82system 9:46.24elapsed 67%CPU (0avgtext+0avgdata 1898516maxresident)k
273665816inputs+0outputs (76major+478622minor)pagefaults 0swaps
```
- With the changes
```
52.57user 262.83system 2:48.46elapsed 187%CPU (0avgtext+0avgdata 7000maxresident)k
245795968inputs+0outputs (110major+827minor)pagefaults 0swaps
```

Note: this PR aims to have a minimal set of required changes for the feature to work. If single producer mode is accepted I think we could further improve this code by switching to a model where we only have single producer-only thread and multiple consumer-only threads. This can be done by changing file stack to a bounded mpmc channel and changing directory stack to be single threaded (plain old vec) which would be initialized empty in consumer workers. This will require launching consumer threads before processing root paths and changing quit logic to depend on sender being dropped instead of using `num_pending` as bounded channel could block main thread when processing root paths leading to a deadlock.

Fix #1823 

---

_Comment by @BurntSushi on 2022-07-23 15:24_

This is interesting, thanks!

Could you share a script that generates the mock data directory?

Also, I would like to see this benchmarked on real world corpora. Like the Linux and chromium repos (and any other large repos).

As for single producer, the reason why ripgrep wasn't architected that way is because it seems likely to be overall slower. ripgrep benefits a lot from not just parallelizing searching, but also parallelizing gitignore matching. Won't a single producer inhibit the former?

---

_Comment by @arriven on 2022-07-23 15:39_

Sure, here's the script to generate the mock data
```
use std::fs::File;
use std::fs;
use std::io::prelude::*;
use rand::{distributions::Alphanumeric, Rng};

fn main() -> std::io::Result<()> {
    for i in 0..10 {
        let dir = format!("data{}", i);
        fs::create_dir(&dir)?;
        for j in 0..1000000 {
            let mut file = File::create(format!("{}/foo{}.txt", dir, j))?;
            let s: Vec<u8> = rand::thread_rng()
                .sample_iter(&Alphanumeric)
                .take(10000)
                .map(u8::from)
                .collect();
            file.write_all(&s)?;
            println!("{} {}", i, j);
        }
    }
    Ok(())
} 
```

Single producer will indeed inhibit gitignore matching. I'll check how it impacts the speed on real world data but seeing the results I got on mock data I have a suspicion that it will still end up being faster in this mode. That being said, for now it's still possible to have multiple producers and limit the file stack size.

---

_Comment by @arriven on 2022-07-23 17:26_

rg without changes:

- linux
```
0.80user 1.34system 0:02.17elapsed 98%CPU (0avgtext+0avgdata 11540maxresident)k
248424inputs+0outputs (0major+1871minor)pagefaults 0swaps
```
- nixpkgs
```
0.49user 0.53system 0:00.12elapsed 838%CPU (0avgtext+0avgdata 13260maxresident)k
240inputs+0outputs (1major+2253minor)pagefaults 0swaps
```
- chromium
```
3.49user 9.88system 0:04.86elapsed 274%CPU (0avgtext+0avgdata 82352maxresident)k
7647944inputs+0outputs (53major+19794minor)pagefaults 0swaps
```

rg with changes:

- linux
```
0.78user 1.18system 0:02.14elapsed 91%CPU (0avgtext+0avgdata 10652maxresident)k
7608inputs+0outputs (52major+1624minor)pagefaults 0swaps
```
- nixpkgs
```
0.50user 0.52system 0:00.12elapsed 829%CPU (0avgtext+0avgdata 9876maxresident)k
440inputs+0outputs (4major+1404minor)pagefaults 0swaps
```
- chromium
```
3.36user 6.81system 0:03.07elapsed 331%CPU (0avgtext+0avgdata 82040maxresident)k
3780536inputs+0outputs (52major+19671minor)pagefaults 0swaps
```

not really surprising as it seems like definitions for ignore is copied into all worker threads, matching filename against an in-memory container should be a lot cheaper than matching file content. It also seems like the problem is quite heavy on I/O so I'm wondering how async/green threads implementation would hold here


---

_Comment by @BurntSushi on 2022-07-23 17:34_

> not really surprising as it seems like definitions for ignore is copied into all worker threads, matching filename against an in-memory container should be a lot cheaper than matching file content.

It isn't necessarily. Gitignore files can be hundreds of lines long, where as a single file might be a couple of lines.

Chromium is a good test case though. That's an encouraging result. The MySQL repo used to have an absolutely mammoth gitignore, which is what provoked parallelizing gitignore in the first place. But IIRC, they got rid of that.

Also, wait, doesn't this PR still parallelize gitignore matching? I haven't read it in depth yet. I thought the single producer model was future work.

---

_Comment by @arriven on 2022-07-23 17:58_

This PR allows us to run some workers in consumer-only mode (we need at least one such worker or there's a minor chance of getting a deadlock in some cases). I just set it to run all but one worker in that mode because it seems to be yielding the best results. But it's also easy to make it configurable via command line.

---

_Comment by @BurntSushi on 2022-07-23 17:59_

To be honest, I'm not really grok'ing what you're saying. I think I'll just have to go over this PR very carefully. I'm not sure when I'll have time for that.

Also definitely do not want this kind of super subtle thing to be configurable via the CLI. :-)

---

_Review comment by @arriven on `crates/ignore/src/walk.rs`:1304 on 2022-07-23 18:00_

@BurntSushi this is the line that controls if the worker will have access to directories stack

---

_@arriven reviewed on 2022-07-23 18:00_

---

_Comment by @BurntSushi on 2023-07-08 13:41_

So I ran this on a huge directory tree that contains all of the crates from crates.io from a while back. This is what I got:

```
$ time rg-before 'fn matched_stripped'
rustfmt_ignore-0.4.10/src/gitignore.rs
243:    fn matched_stripped<P: AsRef<Path>>(

ignore-0.4.14/src/gitignore.rs
243:    fn matched_stripped<P: AsRef<Path>>(

real    1:38.81
user    35.317
sys     1:03.55
maxmem  352 MB
faults  20

$ time rg-after 'fn matched_stripped'
rustfmt_ignore-0.4.10/src/gitignore.rs
243:    fn matched_stripped<P: AsRef<Path>>(

ignore-0.4.14/src/gitignore.rs
243:    fn matched_stripped<P: AsRef<Path>>(

real    2:54.66
user    44.447
sys     1:21.41
maxmem  353 MB
faults  64
```

I ran each one a few times each. Memory usage and time remained roughly the same.

I'm overall a little skeptical of this change. I probably haven't given it enough thought yet, but the regression on a real world (but brutal) use case I think means I'm going to decline this for now. The other concern I have is termination. It has been notoriously tricky to get that right. I'm not saying it's wrong in this PR, but it's a risk because it's difficult to test.

I'm hoping to do an overhaul of the `ignore` crate soon, and this will definitely be one of the things I re-visit.

Thank you for the work and the ideas. :-)

---

_Closed by @BurntSushi on 2023-07-08 13:41_

---

_Label `gitignore` added by @BurntSushi on 2023-07-08 13:41_

---
