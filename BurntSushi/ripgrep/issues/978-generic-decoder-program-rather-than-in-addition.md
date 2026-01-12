```yaml
number: 978
title: Generic decoder program rather than/in addition to extension-based decompression
type: issue
state: closed
author: c-blake
labels:
  - enhancement
  - question
assignees: []
created_at: 2018-07-12T15:38:10Z
updated_at: 2018-07-21T21:25:13Z
url: https://github.com/BurntSushi/ripgrep/issues/978
synced_at: 2026-01-12T16:13:22Z
```

# Generic decoder program rather than/in addition to extension-based decompression

---

_@c-blake_

#### What version of ripgrep are you using?

ripgrep 0.8.1
-SIMD -AVX

#### How did you install ripgrep?

emerge sys-apps/ripgrep

#### What operating system are you using ripgrep on?

Gentoo linux (kernel 4.17.5); Rust version 1.27.0, but none of these questions should be relevant to this feature suggestion/request.

#### Describe your question, feature request, or bug.

In a recursive searching problem setting, one often has many types of (usually binary) files one would like to search through the "decoded" text of.  Here decoding is something that may even be idiosyncratic to a specific user on a specific system. For example, these may not only be compressed files - other encodings are possible/common such as PDF files where a text layer is extractable via a program like ``pdftotext`` in the poppler package or ``mutool draw`` or some similar tool.

``ripgrep`` already does a better job than most greps here, but it could be much better.  Besides an ever growing/ultimately unknowable set of desired encodings/translations, file name extensions are not the most reliable way to detect file types.  The ``file`` program or its ``libmagic`` are more complete, but is quite slow and even it may omit classes of file a user wants to use.  Further, the transformation that counts as decoding could be idiosyncratic.  I give a concrete example with unidiffs below.

In light of both of the above considerations, the most flexible/robust approach is akin to what ``less`` does with its ``LESSOPEN`` environment variable - allow a user specified program to both classify and decode files that ``ripgrep`` cannot.  The dowside of this relative to the current ``ripgrep`` filename extension oriented approach is a "double or triple ``exec``" cost, but there are many upsides -- total generality, no shared library dependencies and so on.  { Many might not even pay attention to these extra fork/exec costs.  For example, many programs use ``system(3)`` rather than fork and exec which already incurs an extra ``exec``.  These days ``gunzip`` is even a shell script wrapper causing an extra exec of /bin/sh.  But in a recursive grep context the costs surely add up. }

To be concrete consider something like this script which a hypothetical new ``GREP_OPEN`` or ``RIPGREP_OPEN`` variable could point to:
```sh
#!/bin/sh
case "$RIPGREP_INPUT" in
  *.gz) exec gzip -dc ;;                      # input name not needed for gzip..
  *.pdf) exec pdftotext "$RIPGREP_INPUT" - ;; #..but MIGHT be. So, ripgrep exports it
  *) exec cat ;; # unencoded
esac
```
Another possibility using ``file`` is:
```sh
#!/bin/sh
case "$(file -)" in
  *unified diff output*) exec strip-unidiff-context ;;
  *.zs) exec pzstd -cdqp8 ;;
esac
```
where this second example assumes a few things - namely, ``ripgrep`` sets up stdin for the forked child and ``file -`` will do an ``lseek`` backward _post classification_ so that ``pzstd`` sees the _full_ input.   Also, it assumes a ``pzstd`` program is installed and some script/sed script/whatever program to, just as an example, strip the context from a unidiff so that ``ripgrep`` _only searches through actually changing_ text.  Note as part of this example that people often don't just name unified diff output ``foo.patch``, but sometimes ``foo.diff`` or other things.  The file type is almost always recognized from contents not filename extension.  The regular ``file`` does not do the seek, by the way, but a trivial backward compatible patch repairs that situation:
```patch
diff -ruw file-5.32/src/file.c file-5.32-cb/src/file.c
--- file-5.32/src/file.c        2018-04-05 12:47:37.522707019 -0400
+++ file-5.32-cb/src/file.c     2018-04-05 12:49:45.406702873 -0400
@@ -518,6 +518,9 @@

        type = magic_file(ms, std_in ? NULL : inname);

+        if (std_in)         /* Could be context where an invoking script may..*/
+            rewind(stdin);  /*..want to "exec gzip -dc" based on our output */
+
        if (type == NULL) {
                (void)printf("ERROR: %s%c", magic_error(ms), c);
                return 1;
```
The ``/bin/sh-case-file`` approach can be a little slow mostly because file takes like 1 ms per file which can be a lot if files are small and IO is fast.  I personally have a little statically linked C program I used for this which looks at magic numbers and re-exec appropriate decoder/transformers all with more like 50 microsecond overhead (plus decoding/transforming time, of course).  Of course, you also want all your decoding C programs to be statically linked for minimum overhead.  In point of fact, a static C dispatcher + static decoder are already much lower overhead than the overhead from the current likely deployment of ``ripgrep`` doing filename extension dispatcher -> dynamically linked decoders.

Anyway, the actual work to implement this in ``ripgrep`` is probably quite minor and the result is something much more general/powerful than the statically encoded list of filename extensions.  I have a personally patched GNU grep that does this and it is very useful to search through PDF collections of papers in various subdirectories and so on, just as one example use case.  I'm sure there are many more -- limited only by one's imagination, and the diversity/entropy of one's collections of files. :-)

It might be nice as an optimization to allow the "binary file" detection to trigger use of ``RIPGREP_OPEN`` so that it is only invoked when actually needed, although that would block the use case of transforming ordinary text files such as the unidiff patch file example.

---

_Comment by @BurntSushi on 2018-07-12 16:05_

This is an interesting request! Thanks for the detailed description!

I can definitely see how this could come in handy, and given its generality, I think I'd be comfortable with something like this in ripgrep. If someone wanted to add this, I'd be happy to work through most of the details in a PR.

I do have a few constraints/concerns:

* Whatever path this takes, the implementation must be simple. I don't see any reason why this shouldn't be simple, particularly given the fact that ripgrep is already shelling out to programs such as gzip.
* The stdin handling looks a little tricky, but seems doable?
* I think I'd prefer to leave binary file detection out of this. In particular, binary detection happens _during_ search because it cannot happen any other way. That means binary detection could get tripped if the last byte in a file is `NUL`, which would then divert it to the `RIPGREP_OPEN` handler, and then you wind up with all sorts of strange things such as potentially printing results twice.
* I'd rather that this functionality be introduced via a command line flag, e.g., `--open-with`, such that we can leave `RIPGREP_CONFIG_PATH` as the only ripgrep-specific environment variable that impacts its execution.
* Finally, if this doesn't work out for whatever reason in ripgrep proper, libripgrep will be a thing soon, and someone could just write their own program with whatever semantics they desire.

---

_Label `enhancement` added by @BurntSushi on 2018-07-12 16:05_

---

_Label `question` added by @BurntSushi on 2018-07-12 16:05_

---

_Comment by @c-blake on 2018-07-12 16:39_

Totally fair point, binary detection-wise.  Oh well.  :-)

Environment variable vs. command option for activation and various namings are certainly just a matter of taste.  Exporting a (local to/interpreted by all children) environment variable like ``RIPGREP_INPUT`` or ``RIPGREP_INPUT_FILE`` or ``RIPGREP_INPUT_PATH`` or whatever for the input file is quite helpful for inflexible decoder programs that want a path (like my ``pdftotext`` example).  It is possible to pass that data other ways like with a magic token like ``"%s"`` or ``"%path"`` or ``"%input%"`` or something in the argument to ``--open-with`` that gets a pathname.

An environment variable for the child is a simpler interface (IMO).  What I do with my little static C preprocessor is optionally take the name of an environment variable which is then the name of the input file.  So, all you need is a ``getenv(argv[that]``).  This is a nice protocol in that the little static preprocessor program/script can be used by callers other than ``ripgrep`` while preserving the "optionality" of any pathname at all.  It's rare that names are required and file descriptors are less race condition prone.  I.e., once ``ripgrep`` has opened the file, it can (if doing preprocessing) fork/hand it off to the decoder and even if it is deleted by some parallel 'rm' there is no race.

Also, to not mandate an extra ``/bin/sh`` ``exec``, the argument to ``--open-with`` (or maybe ``--preprocessor``?) should be split/tokenized by ``ripgrep`` and ``vfork`` & ``exec`` should be used directly.

The process creation/file descriptor handling I use in C is:
```C
int fds[2];
if (pipe (fds) == -1) die ("cannot create a pipe");
switch (pid = vfork ()) { /* vfork much faster but MUST exec|_exit */
  case 0:             /* child */
    dup2 (desc, 0);   /* stdin = desc on file itself */
    dup2 (fds[1], 1); /* stdout = write end of pipe [1] */
    close (fds[1]);   /* do not need extra handle on pipe */
    close (fds[0]);   /* close read end of pipe [0] */
    if (filename)
      setenv ("RIPGREP_INPUT", filename, 1);
    execvp (preproc[0], preproc); /* become a preprocessor */
    fprintf (stderr, "%s: %s\n", preproc[0], strerror (errno));
    _exit (1);
  default:            /* parent */
    close (fds[1]);   /* close write end of pipe[1] */
    close (desc);     /* close desc on file itself */
    desc = fds[0];    /* replace desc value with read end of pipe [0] */
    break;
  case -1:            /* vfork failed => resource exhaustion => die */
    die ("cannot vfork");
}
```
The only "out of band" set up for that is that ``desc`` is the previously opened file descriptor (a local variable) and ``preproc`` char *[] is set up at program start-up based on iff ``--preproc`` was given.  This snippet just replaces ``desc`` with a handle to the child's output.  ``vfork`` really is faster in a 1000s of files problem setting (just less kernel work to do with shared libs/etc.), but you can time such differences and swap in/out easily enough.

Beyond this you just need the option parsing and reaping the children the same way as however the current ``gzip`` stuff works.  I haven't read to see if you check ``WIFEXITED`` vs ``WIFSIGNALED`` or what you do with sessions/process groups and so on, but whatever it is you do (or _need to do_) with ``gzip`` will be the same.

In truth, I know very little Rust and I only just discovered ``ripgrep`` today or I would just do a PR myself.  It really should be simpler _in toto_ than the current ``-z`` stuff, which I guess you may or may not want to retain for backward compatibility.

---

_Comment by @BurntSushi on 2018-07-12 16:49_

I'm trying to follow everything you're saying, but you appear to be very deep in the weeds. :-) Which is a good thing, but it's hard for me to follow. In particular, talking about `fork` specifically is very Unix specific, and ripgrep needs to work on Windows, so it might be better to just think about this at a bit of a higher level. I'll just say more about what I'm thinking, particularly with respect to the interface that ripgrep makes available:

* ripgrep adds a new flag, `--open-with`.
* This new flag specifies an executable name.
* When this flag is given, ripgrep calls this executable with either 0 or 1 arguments. When given 0 arguments, ripgrep will pipe its stdin contents to the child process. When given 1 argument, it will be a file path. In both cases, ripgrep will read stdout from the given executable instead of reading the file directly.

That's it. No parsing of shell commands. No environment variables.

> which I guess you may or may not want to retain for backward compatibility

Indeed, but not just for backward compatibility. The generality of your proposal comes with a cost: the need for another program. There are giant wins to be had for supporting common cases by default, which is ultimately the point of the ultra specific `-z/--search-zip` flag.

---

_Comment by @c-blake on 2018-07-12 17:11_

Ah.  Sorry.  I never program on Windows, but I wanted to be specific about file descriptor operations since you seemed concerned about that complexity.

The problem with your third bullet is that the preprocessor is what classifies the file and dispatches.  Hence the child knows whether the decoder program needs or does not need a pathname.  So, ``ripgrep`` will never know that.  We want ``ripgrep`` to always invoke the preprocessor the same way.  It's just sometimes the decoder will actually need that pathname information, but most of the time it will not.  ``pdftotext`` is the only example I've found so far where I've needed it.  It may never need it in many user's environments/use cases.  So, it seemed nice to me to not burden such users with even knowing about it which is sort of how that environment variable to pass that info approach works.  It's also nice to have a simpler protocol, though. :-)

If you want to simplify that protocol then it wouldn't be so bad to always just pass one argument, the path.  Many users would probably use that argument -- racing unnecessarily (though the ``readdir`` -> ``open`` race will _always_ be there) or using it to dispatch off of filename extensions instead of, say, magic numbers.  Its presence would not block better usages as long as stdin is still set up for the child, though.  So, I'm fine with just always exec [ DISPATCHERDECODER, PATH ].

And, yeah, ``-z`` will always be one-``exec`` faster than a dispatching-decoder preprocessor.  So, that's a valid optimization.  (I was personally rather amazed to discover ``gunzip`` is a shell wrapper these days instead of a hard/symbolic link, though I imagine that came from difficulty in determining the name under which a program was invoked - but these are _other_ weeds.  Sorry.).

---

_Comment by @c-blake on 2018-07-12 17:16_

I don't know about Rust interfaces for spawning kids, but from a "less-Unixy" point of view, what you need is to either A) regular-mode: open a file (regular mode) or B) preprocess mode: open a pipe to a child, with the child's stdin connected to a file and the child's stdout the replacement for the file you would have used in A).  That really shouldn't be hard.

---

_Comment by @BurntSushi on 2018-07-12 17:31_

@c-blake Yeah Rust's standard library has a decent high level cross platform API for that sort of stuff.

I'm honestly not that worried about the extra forks here, nor am I worried about the racing. I am more than OK with saying that if you need to care about those sorts of things, then you should just go out and write your own program, which will be a feasible thing Real Soon Now. :-)

---

_Comment by @c-blake on 2018-07-12 17:35_

Looking at your ``ripgrep`` code for ``let cmd = process::Command::new(decompression_cmd.cmd)``, it looks like you can just do some ``.stdin()`` clause on your ``process::Command::new()``, passing it maybe an open file object or a path.  You actually could do that instead of the ``.arg(path)`` for all the decompressors you currently support..and probably all in the future, too.  I think the original ``uncompress`` set the "tone" for decoders accepting stdin.  Like I said, it's pretty rare the path is actually needed.

Whether you want to pretty-ify/generalize your Rust code for shelling out to either a decompressor or a general decoder..Well, that is another question.  Compared to just adding a new command option and some new code to handle it, that is a bigger change..probably one best left to the primary author. (Or author of a whole new program using ``libripgrep``.)

---

_Comment by @BurntSushi on 2018-07-12 17:52_

@c-blake Understood. If and when I get to refactoring that code, I'll see about adding this functionality. But I have no idea when that will happen.

---

_Comment by @c-blake on 2018-07-12 18:01_

Ok.  That's totally fair.  If and when I learn enough Rust to do a good PR, I'll just add a new CLI option, always both pass the 1 path arg to the kid _and_ attach ``stdin``, and add in some new ``process::Command::new(decoder_cmd)`` without new internal abstractions/re-factoring.  It seems like we've converged on that as a spec.

But someone (anyone?) else might well get to it first.  I bet the simple version of this is like a 15 minute job for experienced Rustaceans..or at least less than the time to read this thread, most likely.  Maybe more with proper testing.  :-)

---

_Comment by @phiresky on 2018-07-13 15:54_

I was just looking for a way to get `pdfgrep` to not be extremely slow (due to the text extraction), and I realized that getting ripgrep to handle arbitrarly preprocessed input would be the best solution.

The only problem I can think of is printing the page numbers: pdfgrep prints them the same as `grep` would line numbers, but for searching we probably don't want to dump all text in a page into a single line. Doing this correctly / generically would need something like sourcemaps, but even without this the feature would be very useful.

Searching a whole directory structure of pdf files (esp. lecture slides) is something I do all the time, and it would be great if ripgrep would support this :). Any kind of delay from executing processes is irrelevant here, since just parsing the pdf usually already takes 300ms+.

---

_Comment by @phiresky on 2018-07-13 15:55_

Actually, nevermind: line number mappings can fairly easily be accomplished by just prefixing every line with the current page number of the pdf document.

---

_Closed by @BurntSushi on 2018-07-21 21:25_

---
