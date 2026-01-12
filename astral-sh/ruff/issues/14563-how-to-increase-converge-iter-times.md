```yaml
number: 14563
title: How to increase converge iter times
type: issue
state: closed
author: yanyongyu
labels:
  - bug
  - fixes
assignees: []
created_at: 2024-11-24T04:56:16Z
updated_at: 2025-01-19T16:43:58Z
url: https://github.com/astral-sh/ruff/issues/14563
synced_at: 2026-01-12T15:54:54Z
```

# How to increase converge iter times

---

_@yanyongyu_

I'm trying to migrate to ruff to lint and format generated codes. But, i have encountered infinity loop error when using `ruff check --fix`. rule codes `F401, F811, I001`.

In the generated code, there are many duplicated and unsorted imports (F811, I001, etc. about 600 errors per file). Every time running the check command, **part of the errors will be fixed** with the infinity loop error `failed to converge after 100 iterations.`. Running `ruff check --fix` **three times** will fix all error.

Can i increase the converge iter times to fix all the errors by running ruff single time?

As a workaround, ruff check is executed successfully after running isort fix first. Isort will fix most of the errors and ruff will not encounter the converge iter limit.

ruff version: 0.8.0
ruff config:

```toml
[tool.ruff]
line-length = 88
target-version = "py39"

[tool.ruff.format]
line-ending = "lf"

[tool.ruff.lint]
select = [
  "F",     # Pyflakes
  "W",     # pycodestyle warnings
  "E",     # pycodestyle errors
  "I",     # isort
  "UP",    # pyupgrade
  "ASYNC", # flake8-async
  "C4",    # flake8-comprehensions
  "T10",   # flake8-debugger
  "T20",   # flake8-print
  "PYI",   # flake8-pyi
  "PT",    # flake8-pytest-style
  "Q",     # flake8-quotes
  "TID",   # flake8-tidy-imports
  "RUF",   # Ruff-specific rules
]
ignore = [
  "E402",   # module-import-not-at-top-of-file
  "UP037",  # quoted-annotation
  "RUF001", # ambiguous-unicode-character-string
  "RUF002", # ambiguous-unicode-character-docstring
  "RUF003", # ambiguous-unicode-character-comment
]

[tool.ruff.lint.isort]
force-sort-within-sections = true
extra-standard-library = ["typing_extensions"]

[tool.ruff.lint.flake8-pytest-style]
fixture-parentheses = false
mark-parentheses = false

[tool.ruff.lint.pyupgrade]
keep-runtime-typing = true
```

---

_Comment by @dylwil3 on 2024-11-24 14:09_

At the moment I don't think this is configurable by the user (it's set as a constant). 

I'd be curious to know how often this comes up. If you're generating the code and linting programmatically, is there a difference in ease of use between applying the isort lints first vs, say, setting an environment variable?

---

_Label `question` added by @dylwil3 on 2024-11-24 14:09_

---

_Comment by @yanyongyu on 2024-11-24 14:58_

Using isort before ruff lint will need to install another tool, and, ruff is much faster than isort. This is also why I tried to migrate from isort to ruff.

---

_Comment by @dylwil3 on 2024-11-24 15:12_

Sorry, to clarify I meant: does it work if you run `ruff check . --select I --select TID` and then `ruff check`?

---

_Comment by @yanyongyu on 2024-11-24 15:20_

I have tried to only select the Isort rule to lint single file. `ruff check --select I path/to/file`.

but this also reports `error: Failed to converge after 100 iterations.` Under the error log, ruff also reports the remain unfixed error (something like 123 fixed, 456 remain, 456 auto fixable). I re-run the ruff check and the error nums will decrease util there is no error remain. Every time i run the ruff check, part of the errors will be fixed.

---

_Comment by @dylwil3 on 2024-11-24 15:39_

Ok that makes sense, thank you! I'm sort of interested to understand better why `isort` completes the lint here but ruff doesn't... If you're able to give a minimal reproducible example in a separate issue that would be super useful!

But I realize that doesn't address your immediate question: I don't see an obvious reason _not_ to make the max number of iterations configurable... but this may need a few other folks to weigh in

---

_Label `question` removed by @dylwil3 on 2024-11-24 15:39_

---

_Label `configuration` added by @dylwil3 on 2024-11-24 15:39_

---

_Label `needs-decision` added by @dylwil3 on 2024-11-24 15:39_

---

_Comment by @MichaReiser on 2024-11-24 20:50_

> But I realize that doesn't address your immediate question: I don't see an obvious reason not to make the max number of iterations configurable... but this may need a few other folks to weigh in

I'm somewhat hesitant about doing that because the iteration count is mainly a safety guard against infinite loops; consider it an implementation detail. Ruff not converging is often a bug or a case where Ruff generates overlapping fixes (for whatever reason). 

 @yanyongyu could you share one of your problematic files. Having a concrete file could help us reproduce the problem and fix its root cause 

---

_Closed by @MichaReiser on 2024-11-24 20:50_

---

_Reopened by @MichaReiser on 2024-11-24 20:51_

---

_Comment by @yanyongyu on 2024-11-25 04:20_

> why isort completes the lint here but ruff doesn't...

@dylwil3 I made a mistake here. I select both F and I to lint the single file. This will cause error. Without selecting F rules (I only), it's OK now.

I'm also trying to prevent the converge limit at the generation step. Reducing the error count before ruff lint is effective.

And, here is a generated file example with the lint log:

[example.zip](https://github.com/user-attachments/files/17897030/example.zip)

the log is the output of the command: `ruff check --select I --select F --fix actions.py 2>&1`

---

_Label `needs-decision` removed by @MichaReiser on 2024-11-25 07:30_

---

_Label `bug` added by @MichaReiser on 2024-11-25 07:30_

---

_Comment by @MichaReiser on 2024-11-25 07:32_

I haven't looked at what's causing the slow-convergence but I suspect that it might be helpful to prioritize isort higher than other fixes... or in a more general sense: Give fixes with a larger text range a higher priority than more local changes. I'm unsure if we want to limit it to only do so if the ranges overlap. 

The fix sorting is implemented here https://github.com/astral-sh/ruff/blob/9f3a38d408f473df7a6b3574c5d1389bef303dd5/crates/ruff_linter/src/fix/mod.rs#L130-L157

---

_Label `configuration` removed by @MichaReiser on 2024-11-25 07:32_

---

_Label `fixes` added by @MichaReiser on 2024-11-25 07:32_

---

_Comment by @MichaReiser on 2024-11-26 09:01_

Okay. I think the main issue here isn't about sorting, but that there are too many `RedefinedWhileUnused` fixes and most of them have the same `isolation_level` (preventing Ruff from applying more than one fix per iteration. Fixing this requires to come up with a narrower isolation range

---

_Comment by @charliermarsh on 2024-12-06 02:57_

Yeah, it _is_ possible to hit the iteration limit naturally. We could consider making it larger...? Like, 1000 would not be totally unreasonable.

---

_Comment by @brupelo on 2025-01-18 20:51_

I'm not sure if this helps, but I’ve reduced the complexity of mcve.py by narrowing down the imports from pyside6 to a much smaller set, and the issue can still be reproduced with this reduced complexity. I’ll continue performing a binary search to see if I can isolate an even smaller example that highlights more easily this bug

```mcve.py



from PySide6.QtCore import (
    ClassInfo,
    MetaFunction,
    MetaSignal,
    Property,
    PyClassProperty,
    QAbstractAnimation,
    QAbstractAnimation,
    QAbstractEventDispatcher,
    QAbstractEventDispatcher,
    QAbstractItemModel,
    QAbstractItemModel,
    QAbstractListModel,
    QAbstractListModel,
    QAbstractNativeEventFilter,
    QAbstractNativeEventFilter,
    QAbstractProxyModel,
    QAbstractProxyModel,
    QAbstractTableModel,
    QAbstractTableModel,
    QAnimationGroup,
    QAnimationGroup,
    QBasicMutex,
    QBasicMutex,
    QBasicTimer,
    QBasicTimer,
    QBitArray,
    QBitArray,
    QBluetoothPermission,
    QBluetoothPermission,
    QBuffer,
    QBuffer,
    QByteArray,
    QByteArray,
    QByteArrayMatcher,
    QByteArrayMatcher,
    QCalendar,
    QCalendar,
    QCalendarPermission,
    QCalendarPermission,
    QCameraPermission,
    QCameraPermission,
    QCborArray,
    QCborArray,
    QCborError,
    QCborError,
    QCborKnownTags,
    QCborMap,
    QCborMap,
    QCborParserError,
    QCborParserError,
    QCborSimpleType,
    QCborStreamReader,
    QCborStreamReader,
    QCborStreamWriter,
    QCborStreamWriter,
    QCborStringResultByteArray,
    QCborStringResultByteArray,
    QCborStringResultString,
    QCborStringResultString,
    QCborTag,
    QCborValue,
    QCborValue,
    QChildEvent,
    QChildEvent,
    QCollator,
    QCollator,
    QCollatorSortKey,
    QCollatorSortKey,
    QCommandLineOption,
    QCommandLineOption,
    QCommandLineParser,
    QCommandLineParser,
    QConcatenateTablesProxyModel,
    QConcatenateTablesProxyModel,
    QContactsPermission,
    QContactsPermission,
    QCoreApplication,
    QCoreApplication,
    QCryptographicHash,
    QCryptographicHash,
    QDataStream,
    QDataStream,
    QDate,
    QDate,
    QDateTime,
    QDateTime,
    QDeadlineTimer,
    QDeadlineTimer,
    QDir,
    QDir,
    QDirIterator,
    QDirIterator,
    QDynamicPropertyChangeEvent,
    QDynamicPropertyChangeEvent,
    QEasingCurve,
    QEasingCurve,
    QElapsedTimer,
    QElapsedTimer,
    QEnum,
    QEvent,
    QEvent,
    QEventLoop,
    QEventLoop,
    QFactoryInterface,
    QFactoryInterface,
    QFile,
    QFile,
    QFileDevice,
    QFileDevice,
    QFileInfo,
    QFileInfo,
    QFileSelector,
    QFileSelector,
    QFileSystemWatcher,
    QFileSystemWatcher,
    QFlag,
    QFutureInterfaceBase,
    QFutureInterfaceBase,
    QGenericArgument,
    QGenericArgument,
    QGenericArgumentHolder,
    QGenericArgumentHolder,
    QGenericReturnArgument,
    QGenericReturnArgument,
    QGenericReturnArgumentHolder,
    QGenericReturnArgumentHolder,
    QHashSeed,
    QHashSeed,
    QIODevice,
    QIODevice,
    QIODeviceBase,
    QIODeviceBase,
    QIOPipe,
    QIOPipe,
    QIdentityProxyModel,
    QIdentityProxyModel,
    QIntList,
    QItemSelection,
    QItemSelection,
    QItemSelectionModel,
    QItemSelectionModel,
    QItemSelectionRange,
    QItemSelectionRange,
    QJsonArray,
    QJsonArray,
    QJsonDocument,
    QJsonDocument,
    QJsonParseError,
    QJsonParseError,
    QJsonValue,
    QJsonValue,
    QKeyCombination,
    QKeyCombination,
    QLibrary,
    QLibrary,
    QLibraryInfo,
    QLibraryInfo,
    QLine,
    QLine,
    QLineF,
    QLineF,
    QLocale,
    QLocale,
    QLocationPermission,
    QLocationPermission,
    QLockFile,
    QLockFile,
    QLoggingCategory,
    QLoggingCategory,
    QMargins,
    QMargins,
    QMarginsF,
    QMarginsF,
    QMessageAuthenticationCode,
    QMessageAuthenticationCode,
    QMessageLogContext,
    QMessageLogContext,
    QMetaClassInfo,
    QMetaClassInfo,
    QMetaEnum,
    QMetaEnum,
    QMetaMethod,
    QMetaMethod,
    QMetaObject,
    QMetaObject,
    QMetaProperty,
    QMetaProperty,
    QMetaType,
    QMetaType,
    QMicrophonePermission,
    QMicrophonePermission,
    QMimeData,
    QMimeData,
    QMimeDatabase,
    QMimeDatabase,
    QMimeType,
    QMimeType,
    QModelIndex,
    QModelIndex,
    QModelRoleData,
    QModelRoleData,
    QMutex,
    QMutex,
    QMutexLocker,
    QMutexLocker,
    QNativeIpcKey,
    QNativeIpcKey,
    QObject,
    QObject,
    QOperatingSystemVersion,
    QOperatingSystemVersion,
    QOperatingSystemVersionBase,
    QOperatingSystemVersionBase,

)
```

---

_Comment by @brupelo on 2025-01-18 21:07_

Alright, I’ve continued my research a bit. I started testing mcve.py, which initially contained all imports from pyside6 along with some duplicates. After systematically trimming it down through commits and discards in a Git repository, I’ve managed to isolate what seems to be a fairly minimal and clear test case.

```python
from a import (
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
    b,
)
```

It's a simple file with just 103 lines. As soon as you remove another repeated import, you'll encounter the error: error: Failed to converge after 100 iterations. It seems that the issue occurs regardless of whether the duplicates come from the same imports or not. I hope this helps the experts come up with a good strategy to tackle this issue gracefully. :)


---

_Comment by @brupelo on 2025-01-18 21:14_

> Yeah, it _is_ possible to hit the iteration limit naturally. We could consider making it larger...? Like, 1000 would not be totally unreasonable.

Btw... just for fun... i've just created another test case with 100000 duplicated imports and when using autoflake...

```
ptime autoflake --remove-unused-variables --in-place --remove-all-unused-imports mcve.py

Execution time: 0.738 s
```

Just saying... 😋

---

_Comment by @charliermarsh on 2025-01-18 21:37_

Right. Changing the prioritization to run the unused imports fix should make that case instant (700ms would be an eternity, I think).

---

_Comment by @brupelo on 2025-01-18 21:42_

```
import os


def generate_files(directory, num_files, template_generator):
    os.makedirs(directory, exist_ok=True)

    for i in range(num_files):
        file_name = f"file_{i + 1}.py"
        file_path = os.path.join(directory, file_name)
        with open(file_path, "w") as f:
            f.write(template_generator())
        print(f"Generated: {file_path}")

    print(f"\nSuccessfully generated {num_files} Python files in '{directory}'.")


def gen_case1(directory, num_files, num_imports):
    def template_generator():
        return "\n".join(["import b"] * num_imports)

    generate_files(directory, num_files, template_generator)


def gen_case2(directory, num_files, num_imports):
    def template_generator():
        imports = [f"b" for i in range(num_imports)]
        return f"from a import ({', '.join(imports)})"

    generate_files(directory, num_files, template_generator)


if __name__ == "__main__":
    num_files = 10
    num_imports = 10000
    gen_case1("case1", num_files, num_imports)
    gen_case2("case2", num_files, num_imports)

```

I tried with the above autogenerated craash cases with ruff and for these ones process will hang forever (i've just tried with 1 of those autogenerated files of case2, I had to kill the ruff process)... but when i try with autoflake...

```
ptime autoflake --remove-unused-variables --in-place --remove-all-unused-imports file_1.py file_2.py file_3.py file_4.py file_5.py file_6.py file_7.py file_8.py file_9.py file_10.py


Execution time: 0.604 s
```

No issue or whatsoever, autoflake will clean up the whole chunk of code without any struggle.

---

_Comment by @charliermarsh on 2025-01-18 23:06_

I cannot get autoflake to complete on `case1` -- it hangs for me.

Regardless, after recent PRs, Ruff is under 100ms in each case, so going to close this:

```
❯ hyperfine "./target/release/ruff check --fix --no-cache case1" --prepare "python gen.py" --runs 20 --warmup 10
Benchmark 1: ./target/release/ruff check --fix --no-cache case1
  Time (mean ± σ):      90.7 ms ±   2.0 ms    [User: 728.3 ms, System: 49.3 ms]
  Range (min … max):    88.7 ms …  95.2 ms    20 runs

❯ hyperfine "./target/release/ruff check --fix --no-cache case2" --prepare "python gen.py" --runs 20 --warmup 10
Benchmark 1: ./target/release/ruff check --fix --no-cache case2
  Time (mean ± σ):      46.6 ms ±   4.3 ms    [User: 267.7 ms, System: 58.3 ms]
  Range (min … max):    43.0 ms …  60.5 ms    20 runs
```

---

_Closed by @charliermarsh on 2025-01-18 23:06_

---

_Comment by @brupelo on 2025-01-19 05:34_

@charliermarsh Sorry, I just saw this one now, rebased my commit on top of latest origin/main with both your commits at

6004c8c003ee619319f0b07594d1f873d9c8f871
b8e5b95423f3506efd9250d373548b04b7e1deec

and they worked very well... I've decided to spend more time on this one and it holds now on extra scenarios, here's the script I've used:

```
from pathlib import Path


def generate_files(directory, num_files, template_generator):
    # Create directory (and parents if needed) using Path
    directory_path = Path(directory)
    directory_path.mkdir(parents=True, exist_ok=True)

    for i in range(num_files):
        file_name = f"file_{i + 1}.py"
        file_path = directory_path / file_name
        with file_path.open("w") as f:
            f.write(template_generator())
        print(f"Generated: {file_path}")

    print(f"\nSuccessfully generated {num_files} Python files in '{directory}'.")


def gen_case1(directory, num_files, num_imports):
    def template_generator():
        return "\n".join(["import b"] * num_imports)

    generate_files(directory, num_files, template_generator)


def gen_case2(directory, num_files, num_imports):
    def template_generator():
        imports = ["b" for _ in range(num_imports)]
        return f"from a import ({', '.join(imports)})"

    generate_files(directory, num_files, template_generator)


def gen_case3(directory, num_files, num_imports, blob_path):
    # Read the blob content from the provided file path
    blob = blob_path.read_text()

    def template_generator():
        return "\n".join([blob] * num_imports)

    generate_files(directory, num_files, template_generator)


if __name__ == "__main__":
    num_files = 10
    num_imports1 = 10000
    num_imports2 = 10000
    working_dir = Path(__file__).parent  # Get the directory of the current script
    data_dir = working_dir / "data"

    # Ensure the data directory exists
    data_dir.mkdir(parents=True, exist_ok=True)

    # Generate files for each case
    gen_case1("case1", num_files, num_imports1)
    gen_case2("case2", num_files, num_imports1)
    gen_case3("case3", num_files, num_imports2, data_dir / "pyside6_imports.py")

```

Added a new case3 where you can insert blobs of huge import chunks, the one I've used was the one provided in the other related issue containing a real case with all pyside6 imports and here's the results:

- case1: OK
- case2: OK
- case3: OK
   num_imports=10 / (1.89mb of source code) -> 0.17ms
   num_imports = 100 / (18.9mb of source code) -> 0.83ms
   num_imports = 1000 / (189mb of source code) -> 8.77s
   num_imports (BEAST MODE) = 10000 / (1.84gb of source code) -> system inestable, cpu overloaded, ~2min, had to kill the process

So... the only thing i'd complain (which is not a real complain) is ruff making my system unstable but that was just a very unrealistic scenario won't happen, i mean, only real cases i saw with python source code of few dozen of mbs are precompiled chunks of qt blob data but those would be the case where you'd ignore them and exclude them from linting so yeah... good job!

---

_Comment by @brupelo on 2025-01-19 16:39_

@charliermarsh I was using the new closed feature happily and I've noticed something strange... performance wise it's better than ever, no complaints about that but take a look to below case

```
from PySide6.QtWidgets import (
    QSlider,
)

from PySide6.QtCore import (
    Qt,
    Qt,
)


class TimeSliderControl(QSlider):
    def __init__(self, parent=None):
        super().__init__(Qt.Horizontal, parent)
        self.setMinimum(0)
        self.setMaximum(100)
        self.setValue(0)
```

Try to lint it, you'll end up with a wrong fix, ie: look at the Qt import being used in the constructor... but you'll notice it'll get removed. Now, if I make sure it's only imported once

```
from PySide6.QtWidgets import (
    QSlider,
)

from PySide6.QtCore import (
    Qt,
)


class TimeSliderControl(QSlider):
    def __init__(self, parent=None):
        super().__init__(Qt.Horizontal, parent)
        self.setMinimum(0)
        self.setMaximum(100)
        self.setValue(0)
```

The fix will be correct and it will be kept... I hope this little one will be a low-hanging fruit and we don't come back to square 1 performance wise, cos right now it's being really fast.

---

_Comment by @charliermarsh on 2025-01-19 16:43_

Please file a separate issue with a reproduction.

---
