```yaml
number: 1346
title: WalkParallel does not show unaccessible directories.
type: issue
state: closed
author: treere
labels:
  - bug
assignees: []
created_at: 2019-08-09T16:18:17Z
updated_at: 2020-02-17T22:16:39Z
url: https://github.com/BurntSushi/ripgrep/issues/1346
synced_at: 2026-01-12T16:13:23Z
```

# WalkParallel does not show unaccessible directories.

---

_@treere_

If a directory is not accessible the dta produced by WalkParallel is different from the one created by WalkDir: the unaccessible directory is not listed.

How to reproduce:
Prepare the env  (I use Linux)
```bash
# create dir 
mkdir imnotlisted
# make it unaccessible
sudo chmod 700 imnotlisted
sudo chown root:root imnotlisted 
```

Using the small programm below the parallel version
```bash
./small_program parallel imnotlisted
```
outputs:
```
Err(WithDepth { depth: 0, err: WithPath { path: "iminvisible", err: Io(Os { code: 13, kind: PermissionDenied, message: "Permission denied" }) } })
```

but the sequential version
```bash
./small_program walkdir imnotlisted
```
outputs
```
Ok(DirEntry { dent: Walkdir(DirEntry("iminvisible")), err: None })
Err(WithPath { path: "iminvisible", err: Io(Custom { kind: PermissionDenied, error: Error { depth: 0, inner: Io { path: Some("iminvisible"), err: Os { code: 13, kind: PermissionDenied, message: "Permission denied" } } } }) })
```

Which is the desired behaviour? In ripgrep this is not a problem.

-----------------
the "small program below"

```rust
extern crate crossbeam_channel as channel; 
extern crate ignore; 
 
use std::env; 
use std::thread; 
 
use ignore::WalkBuilder; 
 
fn main() { 
    let mut path = env::args().nth(1).unwrap(); 
    let mut parallel = false; 
    let (tx, rx) = channel::bounded::<Result<ignore::DirEntry,ignore::Error>>(100); 
    if path == "parallel" { 
        path = env::args().nth(2).unwrap(); 
        parallel = true; 
    } else if path == "walkdir" { 
        path = env::args().nth(2).unwrap(); 
    } 
 
    let stdout_thread = thread::spawn(move || { 
        for dent in rx { 
            println!("{:?}",dent); 
        } 
    }); 
 
    if parallel { 
        let walker = WalkBuilder::new(path).threads(6).build_parallel(); 
        walker.run(|| { 
            let tx = tx.clone(); 
            Box::new(move |result| { 
                use ignore::WalkState::*; 
 
                tx.send(result).unwrap();
                Continue
            }) 
        }); 
    } else { 
        let walker = WalkBuilder::new(path).build(); 
        for result in walker { 
            tx.send(result).unwrap(); 
        } 
    } 
    drop(tx); 
    stdout_thread.join().unwrap(); 
} 
```
(edit: fixed small program...was all lowercase)

---

_Comment by @BurntSushi on 2019-08-09 19:52_

I think the sequential version is the expected behavior, since there's no problem reading the directory entry from its parent, right? It's only descending into it that results in an error.

I'm not sure when I'll look into this, but a PR is welcome.

---

_Label `bug` added by @BurntSushi on 2019-08-09 19:53_

---

_Closed by @BurntSushi on 2020-02-17 22:16_

---
