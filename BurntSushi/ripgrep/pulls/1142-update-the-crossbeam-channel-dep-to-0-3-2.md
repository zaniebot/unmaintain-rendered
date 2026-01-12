```yaml
number: 1142
title: Update the crossbeam-channel dep to 0.3.2
type: pull_request
state: closed
author: sylvestre
labels: []
assignees: []
base: master
head: master
created_at: 2018-12-15T10:16:19Z
updated_at: 2018-12-15T13:44:02Z
url: https://github.com/BurntSushi/ripgrep/pull/1142
synced_at: 2026-01-12T18:23:13Z
```

# Update the crossbeam-channel dep to 0.3.2

---

_@sylvestre_

Fix issue #1141 
I am still getting some warnings but the ripgrep test suite works
```

warning: unused `std::result::Result` which must be used                                                                                                   
    --> src/walk.rs:1117:13                                                                                                                                
     |                                                                                                                                                     
1117 | /             tx.send(Message::Work(Work {                                                                                                          
1118 | |                 dent: dent,                                                                                                                       
1119 | |                 ignore: self.ig_root.clone(),                                                                                                     
1120 | |                 root_device: root_device,                                                                                                         
1121 | |             }));                                                                                                                                  
     | |________________^                                                                                                                                  
     |                                                                                                                                                     
     = note: #[warn(unused_must_use)] on by default                                                                                                        
     = note: this `Result` may be an `Err` variant, which should be handled                                                                                
                                                                                                                                                           
warning: unused `std::result::Result` which must be used                                                                                                   
    --> src/walk.rs:1430:13                                                                                                                                
     |                                                                                                                                                     
1430 | /             self.tx.send(Message::Work(Work {                                                                                                     
1431 | |                 dent: dent,                                                                                                                       
1432 | |                 ignore: ig.clone(),                                                                                                               
1433 | |                 root_device: root_device,                                                                                                         
1434 | |             }));                                                                                                                                  
     | |________________^                                                                                                                                  
     |                                                                                                                                                     
     = note: this `Result` may be an `Err` variant, which should be handled                                                                                
                                                                                                                                                           
warning: unused `std::result::Result` which must be used                                                                                                   
    --> src/walk.rs:1490:29                                                                                                                                
     |                                                                                                                                                     
1490 |                             self.tx.send(Message::Quit);                                                                                            
     |                             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^                                                                                            
     |                                                                                                                                                     
     = note: this `Result` may be an `Err` variant, which should be handled                                                                                
                                                                                                                                                           
    Finished dev [unoptimized + debuginfo] target(s) in 22.43s                                                                                             
   create-stamp debian/debhelper-build-stamp

```

---

_Comment by @sylvestre on 2018-12-15 10:23_

The build issues are caused by rand.

---

_Closed by @BurntSushi on 2018-12-15 13:44_

---
