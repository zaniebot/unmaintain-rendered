```yaml
number: 775
title: Names of missing required arguments are always colorized
type: issue
state: closed
author: dotdash
labels: []
assignees: []
created_at: 2016-12-12T16:56:31Z
updated_at: 2018-08-02T03:29:58Z
url: https://github.com/clap-rs/clap/issues/775
synced_at: 2026-01-12T16:14:09Z
```

# Names of missing required arguments are always colorized

---

_@dotdash_

### Affected Version of clap

2.19.2

### Expected Behavior Summary

Coloring is only applied when output is sent to a terminal.

### Actual Behavior Summary

The name of the missing required argument is always colorized, giving messed up output e.g. capturing output into vim's quickfix buffer.

```
|| [cargo run --bin render]                                                                                                                  
|| unused variable: `matches`, #[warn(unused_variables)] on by default                                                                       
||    |                                                                                                                                      
|| 33 |     let matches = App::new("My  app")                                                                                                
||    |         ^^^^^^^                                                                                                                      
||-                                                                                                                                          
||      Running `target/debug/render`                                                                                                        
|| The following required arguments were not provided:                                                                                       
||     ^[[1;31m<CONFIG>^[[0m                                                                                                                 
||-                                                                                                                                          
|| USAGE:                                                                                                                                    
||     render <CONFIG> <WIDTH> <HEIGHT>                                                                                                      
||-                                                                                                                                          
|| For more information try --help                                                                                                           
|| [Finished in 1 seconds with code 1]              
```


---

_Comment by @kbknapp on 2016-12-18 20:21_

Is this with using [`AppSettings::ColorNever`](https://docs.rs/clap/2.19.2/clap/enum.AppSettings.html#variant.ColorNever)?

---

_Label `T: RFC / question` added by @kbknapp on 2016-12-18 20:21_

---

_Comment by @dotdash on 2016-12-19 15:04_

It's not, but even with it, that one thing is still colored.

---

_Comment by @dotdash on 2016-12-19 15:27_

OK, this is because the closure in `validate_required` directly uses `Format::Error` instead of the `Colorizer` and thus unconditonally emits ANSI color codes. The quick fix would be to create a `Colorizer` there, and using its `error()` method instead. Is that an acceptable approach (I'm not familiar with the code base)?

---

_Comment by @kbknapp on 2016-12-19 15:48_

Great catch! Yeah that would be an acceptable fix. I'm on mobile right now, so if you'd like to do the PR feel free :wink:

---

_Referenced in [clap-rs/clap#781](../../clap-rs/clap/pulls/781.md) on 2016-12-19 15:54_

---

_Closed by @kbknapp on 2016-12-19 18:47_

---
