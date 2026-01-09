---
number: 6080
title: Non-CLI usage of uv? (From existing Rust code, or possibly from Python)
type: issue
state: closed
author: twardoch
labels: []
assignees: []
created_at: 2024-08-14T10:56:09Z
updated_at: 2024-08-14T15:05:07Z
url: https://github.com/astral-sh/uv/issues/6080
synced_at: 2026-01-07T13:12:17-06:00
---

# Non-CLI usage of uv? (From existing Rust code, or possibly from Python)

---

_Issue opened by @twardoch on 2024-08-14 10:56_

Is there a way to use "uv" not via CLI, but to build and use it "programmatically" (like a library)? 

I mean from existing Rust code, or possibly from Python (akin to micropip in Pyodide, or the undocumented usage of the "pip" package from within running Python code). 

---

_Referenced in [MeVisLab/pythonqt#231](../../MeVisLab/pythonqt/issues/231.md) on 2024-08-14 10:57_

---

_Comment by @Magic-wei on 2024-08-14 12:26_

Are you trying to make a GUI for uv? I guess you can just wrap CLI commands in python code like this:
```python
import os

result = os.system("uv pip install xxx")
print(result) # return 0 if no error

# or
result = os.popen("uv pip install xxx")
print(result.read())  # get all stdout
```

---

_Comment by @zanieb on 2024-08-14 14:41_

Previously, we've noted that our Rust APIs are not stable enough for external usage https://github.com/astral-sh/uv/issues/2133 and that we would not add Python bindings https://github.com/astral-sh/uv/issues/2794

However, we did make it possible to invoke uv from Rust https://github.com/astral-sh/uv/pull/4642 — but major caveats apply, i.e., we expect we have full control over the process and that uv is not invoked more than once per process. In most cases, you're probably better off creating a subprocess and using the CLI.

---

_Comment by @zanieb on 2024-08-14 14:43_

You can, of course, use any of our crates as git-dependencies in a Rust project just know there is no expectation of stability.

---

_Comment by @twardoch on 2024-08-14 14:56_

I'm working on a Qt5 C++ app that embeds the Python interpreter using https://github.com/MeVisLab/pythonqt and exposes the Qt C++ internals to Python this way. That solution doesn't really have a standalone CPython executable (Python.exe or so), so running pip from within the Qt app (in order to install additional packages to the Python environment) was complicated. Because the "pip" command line tool was doing "PythonExe -m pip" and that PythonExe is non-existent.

"uv" has its own executable which doesn't rely on the Python interpreter, so indeed from within the app's Python I can already do

```python
import uv
import subprocess
subprocess.run([uv.find_uv_bin(), "pip", "install"...])
```

and it kind of works. But I'm wondering if there's a way that I can compile the uv code as a lib and then use something like https://github.com/KDAB/cxx-qt to call the specific uv functionality directly from the Qt C++ code. 

Of course I can always bundle the "uv" executable for the right platform and from Qt C++ call 

```cpp

#include <QProcess>
#include <QString>
#include <QByteArray>
#include <QDebug>

void runSubprocess() {
    QProcess process;
    process.setProgram("uv.exe");
    process.setArguments(QStringList() << "arg1" << "arg2");

    // Capture both stdout and stderr
    process.setProcessChannelMode(QProcess::SeparateChannels);

    process.start();
    if (!process.waitForStarted()) {
        qDebug() << "Failed to start the process";
        return;
    }

    if (!process.waitForFinished()) {
        qDebug() << "Process timed out";
        return;
    }

    // Capture stdout
    QByteArray stdoutData = process.readAllStandardOutput();
    QString stdoutStr = QString::fromLocal8Bit(stdoutData);

    // Capture stderr
    QByteArray stderrData = process.readAllStandardError();
    QString stderrStr = QString::fromLocal8Bit(stderrData);

    qDebug() << "stdout:" << stdoutStr;
    qDebug() << "stderr:" << stderrStr;
}

```

but I wonder if there's a better way. 

---

_Comment by @twardoch on 2024-08-14 15:00_

Ok, so what I gather is that it's not advised to try use the uv "API" but instead just the CLI. That's good enough — it's still better than what we had to do before (wrestle with importlib & the private pip API). 

The fact that the "uv" CLI. app is self-sufficient (and unproblematic license-wise) is already helpful :)

---

_Comment by @zanieb on 2024-08-14 15:05_

Yeah basically it's safe to use _some_ of uv's internal APIs, but the `uv` crate which defines the functionality of the CLI is not safe to invoke unless you are going to do it _once_ and cede control of the process. We just haven't designed for it to be used as a library because it increases scope so much. The CLI itself will be much more stable than calling into Rust and hopefully the overhead is minimal compared to the actual package operations.

Thanks for sharing your use-case!

---

_Closed by @zanieb on 2024-08-14 15:05_

---
