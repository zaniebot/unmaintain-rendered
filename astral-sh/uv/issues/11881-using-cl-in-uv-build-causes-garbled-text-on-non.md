```yaml
number: 11881
title: Using cl in Uv build causes garbled text on non-English encoding Windows
type: issue
state: open
author: sdbds
labels:
  - windows
  - needs-mre
assignees: []
created_at: 2025-03-01T09:44:31Z
updated_at: 2025-04-22T11:27:11Z
url: https://github.com/astral-sh/uv/issues/11881
synced_at: 2026-01-12T16:00:49Z
```

# Using cl in Uv build causes garbled text on non-English encoding Windows

---

_@sdbds_

### Summary

Using cl in Uv build causes garbled text on non-English encoding Windows

python setup.py bdist_wheel is OK.

```
C:\Users\qinglongshengzhe\AppData\Local\uv\cache\sdists-v8\.tmp21jyVj\spas_sage_attn-0.1.0\csrc\qattn\qk_int_sv_f8_cuda.cu(17): fatal error C1083: �޷��򿪰����ļ�: ��../utils.cuh��: No such file or directory
[3/3] cl /showIncludes /nologo /O2 /W3 /GL /DNDEBUG /MD /MD /wd4819 /wd4251 /wd4244 /wd4267 /wd4275 /wd4018 /wd4190 /wd4624 /wd4067 /wd4068 /EHsc -IE:\Python310\lib\site-packages\torch\include -IE:\Python310\lib\site-packages\torch\include\torch\csrc\api\include -IE:\Python310\lib\site-packages\torch\include\TH -IE:\Python310\lib\site-packages\torch\include\THC -IE:\cuda\12.4\include -IE:\cudnn\v9.1\include -IE:\Python310\include -IE:\Python310\Include "-IE:\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.41.34120\include" "-IE:\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.41.34120\ATLMFC\include" "-IE:\Microsoft Visual Studio\2022\Community\VC\Auxiliary\VS\include" "-IE:\Windows Kits\10\include\10.0.22621.0\ucrt" "-IE:\Windows Kits\10\\include\10.0.22621.0\\um" "-IE:\Windows Kits\10\\include\10.0.22621.0\\shared" "-IE:\Windows Kits\10\\include\10.0.22621.0\\winrt" "-IE:\Windows Kits\10\\include\10.0.22621.0\\cppwinrt" -c C:\Users\qinglongshengzhe\AppData\Local\uv\cache\sdists-v8\.tmp21jyVj\spas_sage_attn-0.1.0\csrc\qattn\pybind.cpp /FoC:\Users\qinglongshengzhe\AppData\Local\uv\cache\sdists-v8\.tmp21jyVj\spas_sage_attn-0.1.0\build\temp.win-amd64-cpython-310\Release\csrc/qattn/pybind.obj -g -O3 -fopenmp -lgomp -std=c++17 -DENABLE_BF16 -D_GLIBCXX_USE_CXX11_ABI=0 -DTORCH_API_INCLUDE_EXTENSION_H -DTORCH_EXTENSION_NAME=_qattn -D_GLIBCXX_USE_CXX11_ABI=0 /std:c++17

ע��: �����ļ�:    E:\Python310\lib\site-packages\torch\include\torch/csrc/autograd/python_variable.h
ע��: �����ļ�:    E:\Python310\lib\site-packages\torch\include\torch/csrc/utils/python_numbers.h
ע��: �����ļ�:     E:\Python310\lib\site-packages\torch\include\torch/csrc/jit/frontend/tracer.h
ע��: �����ļ�:     E:\Python310\lib\site-packages\torch\include\torch/csrc/utils/object_ptr.h
ע��: �����ļ�:     E:\Python310\lib\site-packages\torch\include\torch/csrc/utils/tensor_numpy.h
ע��: �����ļ�:    E:\Python310\lib\site-packages\torch\include\torch/csrc/utils/python_tuples.h
C:\Users\qinglongshengzhe\AppData\Local\uv\cache\sdists-v8\.tmpaoUjlR\spas_sage_attn-0.1.0\csrc\qattn\pybind.cpp(19): fatal error C1083: �޷��򿪰����ļ�: ��attn_cuda.h��: No such file or directory
ninja: build stopped: subcommand failed.
  × Failed to build `E:\Code\SpargeAttn-for-windows\`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit code: 1)
      hint: This usually indicates a problem with the package or the build environment.
```

### Platform

Windows 11 x64

### Version

uv 0.6.3 (a0b9f22a2 2025-02-24)

### Python version

3.10

---

_Label `bug` added by @sdbds on 2025-03-01 09:44_

---

_Label `windows` added by @konstin on 2025-03-03 13:46_

---

_Comment by @konstin on 2025-03-10 11:42_

Can you share a command/setup that reproduces that problem?

---

_Label `bug` removed by @konstin on 2025-03-10 11:42_

---

_Label `needs-mre` added by @konstin on 2025-03-10 11:42_

---

_Comment by @sdbds on 2025-03-11 06:00_

> Can you share a command/setup that reproduces that problem?

this repo as sample:

https://github.com/sdbds/SpargeAttn-for-windows

`uv build --python 3.10 --no-build-isolation --verbose ./ `

```
DEBUG uv 0.6.4 (04db70662 2025-03-03)
DEBUG Searching for Python 3.10 in virtual environments, managed installations, search path, or registry
DEBUG Searching for managed installations at `E:\Roaming\uv\python`
DEBUG Skipping incompatible managed installation `cpython-3.12.8-windows-x86_64-none`
DEBUG Skipping incompatible managed installation `cpython-3.11.10-windows-x86_64-none`
DEBUG Found `cpython-3.10.11-windows-x86_64-none` at `E:\Python310\python.exe` (first executable in the search path)
DEBUG Using request timeout of 30s
Building source distribution...
DEBUG Proceeding without build isolation
DEBUG Calling `setuptools.build_meta:__legacy__.build_sdist("E:\\Code\\SpargeAttn-for-windows\\dist", {})`
running sdist
running egg_info
writing spas_sage_attn.egg-info\PKG-INFO
writing dependency_links to spas_sage_attn.egg-info\dependency_links.txt
writing top-level names to spas_sage_attn.egg-info\top_level.txt
reading manifest file 'spas_sage_attn.egg-info\SOURCES.txt'
adding license file 'LICENSE'
writing manifest file 'spas_sage_attn.egg-info\SOURCES.txt'
running check
creating spas_sage_attn-0.1.0
creating spas_sage_attn-0.1.0\csrc
creating spas_sage_attn-0.1.0\csrc\fused
creating spas_sage_attn-0.1.0\csrc\qattn
creating spas_sage_attn-0.1.0\evaluate
creating spas_sage_attn-0.1.0\evaluate\modify_model
creating spas_sage_attn-0.1.0\spas_sage_attn
creating spas_sage_attn-0.1.0\spas_sage_attn.egg-info
copying files to spas_sage_attn-0.1.0...
copying LICENSE -> spas_sage_attn-0.1.0
copying README.md -> spas_sage_attn-0.1.0
copying setup.py -> spas_sage_attn-0.1.0
copying csrc/fused\fused.cu -> spas_sage_attn-0.1.0\csrc/fused
copying csrc/fused\pybind.cpp -> spas_sage_attn-0.1.0\csrc/fused
copying csrc/qattn\pybind.cpp -> spas_sage_attn-0.1.0\csrc/qattn
copying csrc/qattn\qk_int_sv_f16_cuda.cu -> spas_sage_attn-0.1.0\csrc/qattn
copying csrc/qattn\qk_int_sv_f8_cuda.cu -> spas_sage_attn-0.1.0\csrc/qattn
copying csrc\fused\fused.cu -> spas_sage_attn-0.1.0\csrc\fused
copying csrc\fused\pybind.cpp -> spas_sage_attn-0.1.0\csrc\fused
copying csrc\qattn\pybind.cpp -> spas_sage_attn-0.1.0\csrc\qattn
copying csrc\qattn\qk_int_sv_f16_cuda.cu -> spas_sage_attn-0.1.0\csrc\qattn
copying csrc\qattn\qk_int_sv_f8_cuda.cu -> spas_sage_attn-0.1.0\csrc\qattn
copying evaluate\__init__.py -> spas_sage_attn-0.1.0\evaluate
copying evaluate\cogvideo_example.py -> spas_sage_attn-0.1.0\evaluate
copying evaluate\modify_model\__init__.py -> spas_sage_attn-0.1.0\evaluate\modify_model
copying evaluate\modify_model\modify_cogvideo.py -> spas_sage_attn-0.1.0\evaluate\modify_model
copying spas_sage_attn\__init__.py -> spas_sage_attn-0.1.0\spas_sage_attn
copying spas_sage_attn\autotune.py -> spas_sage_attn-0.1.0\spas_sage_attn
copying spas_sage_attn\core.py -> spas_sage_attn-0.1.0\spas_sage_attn
copying spas_sage_attn\quant_per_block.py -> spas_sage_attn-0.1.0\spas_sage_attn
copying spas_sage_attn\quant_per_warp_cuda.py -> spas_sage_attn-0.1.0\spas_sage_attn
copying spas_sage_attn\utils.py -> spas_sage_attn-0.1.0\spas_sage_attn
copying spas_sage_attn.egg-info\PKG-INFO -> spas_sage_attn-0.1.0\spas_sage_attn.egg-info
copying spas_sage_attn.egg-info\SOURCES.txt -> spas_sage_attn-0.1.0\spas_sage_attn.egg-info
copying spas_sage_attn.egg-info\dependency_links.txt -> spas_sage_attn-0.1.0\spas_sage_attn.egg-info
copying spas_sage_attn.egg-info\top_level.txt -> spas_sage_attn-0.1.0\spas_sage_attn.egg-info
copying spas_sage_attn.egg-info\SOURCES.txt -> spas_sage_attn-0.1.0\spas_sage_attn.egg-info
Writing spas_sage_attn-0.1.0\setup.cfg
Creating tar archive
removing 'spas_sage_attn-0.1.0' (and everything under it)
Building wheel from source distribution...
DEBUG Proceeding without build isolation
DEBUG Calling `setuptools.build_meta:__legacy__.build_wheel("E:\\Code\\SpargeAttn-for-windows\\dist", {}, None)`
running bdist_wheel
running build
running build_py
creating build
creating build\lib.win-amd64-cpython-310
creating build\lib.win-amd64-cpython-310\evaluate
copying evaluate\cogvideo_example.py -> build\lib.win-amd64-cpython-310\evaluate
copying evaluate\__init__.py -> build\lib.win-amd64-cpython-310\evaluate
creating build\lib.win-amd64-cpython-310\spas_sage_attn
copying spas_sage_attn\autotune.py -> build\lib.win-amd64-cpython-310\spas_sage_attn
copying spas_sage_attn\core.py -> build\lib.win-amd64-cpython-310\spas_sage_attn
copying spas_sage_attn\quant_per_block.py -> build\lib.win-amd64-cpython-310\spas_sage_attn
copying spas_sage_attn\quant_per_warp_cuda.py -> build\lib.win-amd64-cpython-310\spas_sage_attn
copying spas_sage_attn\utils.py -> build\lib.win-amd64-cpython-310\spas_sage_attn
copying spas_sage_attn\__init__.py -> build\lib.win-amd64-cpython-310\spas_sage_attn
creating build\lib.win-amd64-cpython-310\evaluate\modify_model
copying evaluate\modify_model\modify_cogvideo.py -> build\lib.win-amd64-cpython-310\evaluate\modify_model
copying evaluate\modify_model\__init__.py -> build\lib.win-amd64-cpython-310\evaluate\modify_model
running build_ext
E:\Python310\lib\site-packages\torch\utils\cpp_extension.py:416: UserWarning: The detected CUDA version (12.6) has a minor version mismatch with the version that was used to compile PyTorch (12.4). Most likely this shouldn't be a problem.
  warnings.warn(CUDA_MISMATCH_WARN.format(cuda_str_version, torch.version.cuda))
building 'spas_sage_attn._qattn' extension
creating C:\Users\qinglongshengzhe\AppData\Local\uv\cache\sdists-v8\.tmpRkTEpR\spas_sage_attn-0.1.0\build\temp.win-amd64-cpython-310
creating C:\Users\qinglongshengzhe\AppData\Local\uv\cache\sdists-v8\.tmpRkTEpR\spas_sage_attn-0.1.0\build\temp.win-amd64-cpython-310\Release
creating C:\Users\qinglongshengzhe\AppData\Local\uv\cache\sdists-v8\.tmpRkTEpR\spas_sage_attn-0.1.0\build\temp.win-amd64-cpython-310\Release\csrc
creating C:\Users\qinglongshengzhe\AppData\Local\uv\cache\sdists-v8\.tmpRkTEpR\spas_sage_attn-0.1.0\build\temp.win-amd64-cpython-310\Release\csrc\qattn
Emitting ninja build file C:\Users\qinglongshengzhe\AppData\Local\uv\cache\sdists-v8\.tmpRkTEpR\spas_sage_attn-0.1.0\build\temp.win-amd64-cpython-310\Release\build.ninja...
Compiling objects...
Allowing ninja to set a default number of workers... (overridable by setting the environment variable MAX_JOBS=N)
[1/3] E:\cuda\12.6\bin\nvcc --generate-dependencies-with-compile --dependency-output C:\Users\qinglongshengzhe\AppData\Local\uv\cache\sdists-v8\.tmpRkTEpR\spas_sage_attn-0.1.0\build\temp.win-amd64-cpython-310\Release\csrc/qattn/qk_int_sv_f8_cuda.obj.d -std=c++17 --use-local-env -Xcompiler /MD -Xcompiler /wd4819 -Xcompiler /wd4251 -Xcompiler /wd4244 -Xcompiler /wd4267 -Xcompiler /wd4275 -Xcompiler /wd4018 -Xcompiler /wd4190 -Xcompiler /wd4624 -Xcompiler /wd4067 -Xcompiler /wd4068 -Xcompiler /EHsc -Xcudafe --diag_suppress=base_class_has_different_dll_interface -Xcudafe --diag_suppress=field_without_dll_interface -Xcudafe --diag_suppress=dll_interface_conflict_none_assumed -Xcudafe --diag_suppress=dll_interface_conflict_dllexport_assumed -IE:\Python310\lib\site-packages\torch\include -IE:\Python310\lib\site-packages\torch\include\torch\csrc\api\include -IE:\Python310\lib\site-packages\torch\include\TH -IE:\Python310\lib\site-packages\torch\include\THC -IE:\cuda\12.6\include -IE:\cudnn\v9.6\include -IE:\Python310\include -IE:\Python310\Include "-IE:\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.41.34120\include" "-IE:\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.41.34120\ATLMFC\include" "-IE:\Microsoft Visual Studio\2022\Community\VC\Auxiliary\VS\include" "-IE:\Windows Kits\10\include\10.0.22621.0\ucrt" "-IE:\Windows Kits\10\\include\10.0.22621.0\\um" "-IE:\Windows Kits\10\\include\10.0.22621.0\\shared" "-IE:\Windows Kits\10\\include\10.0.22621.0\\winrt" "-IE:\Windows Kits\10\\include\10.0.22621.0\\cppwinrt" -c C:\Users\qinglongshengzhe\AppData\Local\uv\cache\sdists-v8\.tmpRkTEpR\spas_sage_attn-0.1.0\csrc\qattn\qk_int_sv_f8_cuda.cu -o C:\Users\qinglongshengzhe\AppData\Local\uv\cache\sdists-v8\.tmpRkTEpR\spas_sage_attn-0.1.0\build\temp.win-amd64-cpython-310\Release\csrc/qattn/qk_int_sv_f8_cuda.obj -D__CUDA_NO_HALF_OPERATORS__ -D__CUDA_NO_HALF_CONVERSIONS__ -D__CUDA_NO_BFLOAT16_CONVERSIONS__ -D__CUDA_NO_HALF2_OPERATORS__ --expt-relaxed-constexpr -O3 -std=c++17 -U__CUDA_NO_HALF_OPERATORS__ -U__CUDA_NO_HALF_CONVERSIONS__ --use_fast_math --threads=8 -Xptxas=-v -diag-suppress=174 -D_GLIBCXX_USE_CXX11_ABI=0 -gencode arch=compute_89,code=sm_89 -DTORCH_API_INCLUDE_EXTENSION_H -DTORCH_EXTENSION_NAME=_qattn -D_GLIBCXX_USE_CXX11_ABI=0
FAILED: C:/Users/qinglongshengzhe/AppData/Local/uv/cache/sdists-v8/.tmpRkTEpR/spas_sage_attn-0.1.0/build/temp.win-amd64-cpython-310/Release/csrc/qattn/qk_int_sv_f8_cuda.obj
E:\cuda\12.6\bin\nvcc --generate-dependencies-with-compile --dependency-output C:\Users\qinglongshengzhe\AppData\Local\uv\cache\sdists-v8\.tmpRkTEpR\spas_sage_attn-0.1.0\build\temp.win-amd64-cpython-310\Release\csrc/qattn/qk_int_sv_f8_cuda.obj.d -std=c++17 --use-local-env -Xcompiler /MD -Xcompiler /wd4819 -Xcompiler /wd4251 -Xcompiler /wd4244 -Xcompiler /wd4267 -Xcompiler /wd4275 -Xcompiler /wd4018 -Xcompiler /wd4190 -Xcompiler /wd4624 -Xcompiler /wd4067 -Xcompiler /wd4068 -Xcompiler /EHsc -Xcudafe --diag_suppress=base_class_has_different_dll_interface -Xcudafe --diag_suppress=field_without_dll_interface -Xcudafe --diag_suppress=dll_interface_conflict_none_assumed -Xcudafe --diag_suppress=dll_interface_conflict_dllexport_assumed -IE:\Python310\lib\site-packages\torch\include -IE:\Python310\lib\site-packages\torch\include\torch\csrc\api\include -IE:\Python310\lib\site-packages\torch\include\TH -IE:\Python310\lib\site-packages\torch\include\THC -IE:\cuda\12.6\include -IE:\cudnn\v9.6\include -IE:\Python310\include -IE:\Python310\Include "-IE:\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.41.34120\include" "-IE:\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.41.34120\ATLMFC\include" "-IE:\Microsoft Visual Studio\2022\Community\VC\Auxiliary\VS\include" "-IE:\Windows Kits\10\include\10.0.22621.0\ucrt" "-IE:\Windows Kits\10\\include\10.0.22621.0\\um" "-IE:\Windows Kits\10\\include\10.0.22621.0\\shared" "-IE:\Windows Kits\10\\include\10.0.22621.0\\winrt" "-IE:\Windows Kits\10\\include\10.0.22621.0\\cppwinrt" -c C:\Users\qinglongshengzhe\AppData\Local\uv\cache\sdists-v8\.tmpRkTEpR\spas_sage_attn-0.1.0\csrc\qattn\qk_int_sv_f8_cuda.cu -o C:\Users\qinglongshengzhe\AppData\Local\uv\cache\sdists-v8\.tmpRkTEpR\spas_sage_attn-0.1.0\build\temp.win-amd64-cpython-310\Release\csrc/qattn/qk_int_sv_f8_cuda.obj -D__CUDA_NO_HALF_OPERATORS__ -D__CUDA_NO_HALF_CONVERSIONS__ -D__CUDA_NO_BFLOAT16_CONVERSIONS__ -D__CUDA_NO_HALF2_OPERATORS__ --expt-relaxed-constexpr -O3 -std=c++17 -U__CUDA_NO_HALF_OPERATORS__ -U__CUDA_NO_HALF_CONVERSIONS__ --use_fast_math --threads=8 -Xptxas=-v -diag-suppress=174 -D_GLIBCXX_USE_CXX11_ABI=0 -gencode arch=compute_89,code=sm_89 -DTORCH_API_INCLUDE_EXTENSION_H -DTORCH_EXTENSION_NAME=_qattn -D_GLIBCXX_USE_CXX11_ABI=0
qk_int_sv_f8_cuda.cu
cl: ������ warning D9025 :������д��/D__CUDA_NO_HALF_OPERATORS__��(�á�/U__CUDA_NO_HALF_OPERATORS__��)
cl: ������ warning D9025 :������д��/D__CUDA_NO_HALF_CONVERSIONS__��(�á�/U__CUDA_NO_HALF_CONVERSIONS__��)
qk_int_sv_f8_cuda.cu
C:\Users\qinglongshengzhe\AppData\Local\uv\cache\sdists-v8\.tmpRkTEpR\spas_sage_attn-0.1.0\csrc\qattn\qk_int_sv_f8_cuda.cu(17): fatal error C1083: �޷��򿪰����ļ�: ��../utils.cuh��: No such file or directory
cl: ������ warning D9025 :������д��/D__CUDA_NO_HALF_OPERATORS__��(�á�/U__CUDA_NO_HALF_OPERATORS__��)
cl: ������ warning D9025 :������д��/D__CUDA_NO_HALF_CONVERSIONS__��(�á�/U__CUDA_NO_HALF_CONVERSIONS__��)
qk_int_sv_f8_cuda.cu
C:\Users\qinglongshengzhe\AppData\Local\uv\cache\sdists-v8\.tmpRkTEpR\spas_sage_attn-0.1.0\csrc\qattn\qk_int_sv_f8_cuda.cu(17): fatal error C1083: �޷��򿪰����ļ�: ��../utils.cuh��: No such file or directory
[2/3] E:\cuda\12.6\bin\nvcc --generate-dependencies-with-compile --dependency-output C:\Users\qinglongshengzhe\AppData\Local\uv\cache\sdists-v8\.tmpRkTEpR\spas_sage_attn-0.1.0\build\temp.win-amd64-cpython-310\Release\csrc/qattn/qk_int_sv_f16_cuda.obj.d -std=c++17 --use-local-env -Xcompiler /MD -Xcompiler /wd4819 -Xcompiler /wd4251 -Xcompiler /wd4244 -Xcompiler /wd4267 -Xcompiler /wd4275 -Xcompiler /wd4018 -Xcompiler /wd4190 -Xcompiler /wd4624 -Xcompiler /wd4067 -Xcompiler /wd4068 -Xcompiler /EHsc -Xcudafe --diag_suppress=base_class_has_different_dll_interface -Xcudafe --diag_suppress=field_without_dll_interface -Xcudafe --diag_suppress=dll_interface_conflict_none_assumed -Xcudafe --diag_suppress=dll_interface_conflict_dllexport_assumed -IE:\Python310\lib\site-packages\torch\include -IE:\Python310\lib\site-packages\torch\include\torch\csrc\api\include -IE:\Python310\lib\site-packages\torch\include\TH -IE:\Python310\lib\site-packages\torch\include\THC -IE:\cuda\12.6\include -IE:\cudnn\v9.6\include -IE:\Python310\include -IE:\Python310\Include "-IE:\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.41.34120\include" "-IE:\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.41.34120\ATLMFC\include" "-IE:\Microsoft Visual Studio\2022\Community\VC\Auxiliary\VS\include" "-IE:\Windows Kits\10\include\10.0.22621.0\ucrt" "-IE:\Windows Kits\10\\include\10.0.22621.0\\um" "-IE:\Windows Kits\10\\include\10.0.22621.0\\shared" "-IE:\Windows Kits\10\\include\10.0.22621.0\\winrt" "-IE:\Windows Kits\10\\include\10.0.22621.0\\cppwinrt" -c C:\Users\qinglongshengzhe\AppData\Local\uv\cache\sdists-v8\.tmpRkTEpR\spas_sage_attn-0.1.0\csrc\qattn\qk_int_sv_f16_cuda.cu -o C:\Users\qinglongshengzhe\AppData\Local\uv\cache\sdists-v8\.tmpRkTEpR\spas_sage_attn-0.1.0\build\temp.win-amd64-cpython-310\Release\csrc/qattn/qk_int_sv_f16_cuda.obj -D__CUDA_NO_HALF_OPERATORS__ -D__CUDA_NO_HALF_CONVERSIONS__ -D__CUDA_NO_BFLOAT16_CONVERSIONS__ -D__CUDA_NO_HALF2_OPERATORS__ --expt-relaxed-constexpr -O3 -std=c++17 -U__CUDA_NO_HALF_OPERATORS__ -U__CUDA_NO_HALF_CONVERSIONS__ --use_fast_math --threads=8 -Xptxas=-v -diag-suppress=174 -D_GLIBCXX_USE_CXX11_ABI=0 -gencode arch=compute_89,code=sm_89 -DTORCH_API_INCLUDE_EXTENSION_H -DTORCH_EXTENSION_NAME=_qattn -D_GLIBCXX_USE_CXX11_ABI=0
FAILED: C:/Users/qinglongshengzhe/AppData/Local/uv/cache/sdists-v8/.tmpRkTEpR/spas_sage_attn-0.1.0/build/temp.win-amd64-cpython-310/Release/csrc/qattn/qk_int_sv_f16_cuda.obj
E:\cuda\12.6\bin\nvcc --generate-dependencies-with-compile --dependency-output C:\Users\qinglongshengzhe\AppData\Local\uv\cache\sdists-v8\.tmpRkTEpR\spas_sage_attn-0.1.0\build\temp.win-amd64-cpython-310\Release\csrc/qattn/qk_int_sv_f16_cuda.obj.d -std=c++17 --use-local-env -Xcompiler /MD -Xcompiler /wd4819 -Xcompiler /wd4251 -Xcompiler /wd4244 -Xcompiler /wd4267 -Xcompiler /wd4275 -Xcompiler /wd4018 -Xcompiler /wd4190 -Xcompiler /wd4624 -Xcompiler /wd4067 -Xcompiler /wd4068 -Xcompiler /EHsc -Xcudafe --diag_suppress=base_class_has_different_dll_interface -Xcudafe --diag_suppress=field_without_dll_interface -Xcudafe --diag_suppress=dll_interface_conflict_none_assumed -Xcudafe --diag_suppress=dll_interface_conflict_dllexport_assumed -IE:\Python310\lib\site-packages\torch\include -IE:\Python310\lib\site-packages\torch\include\torch\csrc\api\include -IE:\Python310\lib\site-packages\torch\include\TH -IE:\Python310\lib\site-packages\torch\include\THC -IE:\cuda\12.6\include -IE:\cudnn\v9.6\include -IE:\Python310\include -IE:\Python310\Include "-IE:\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.41.34120\include" "-IE:\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.41.34120\ATLMFC\include" "-IE:\Microsoft Visual Studio\2022\Community\VC\Auxiliary\VS\include" "-IE:\Windows Kits\10\include\10.0.22621.0\ucrt" "-IE:\Windows Kits\10\\include\10.0.22621.0\\um" "-IE:\Windows Kits\10\\include\10.0.22621.0\\shared" "-IE:\Windows Kits\10\\include\10.0.22621.0\\winrt" "-IE:\Windows Kits\10\\include\10.0.22621.0\\cppwinrt" -c C:\Users\qinglongshengzhe\AppData\Local\uv\cache\sdists-v8\.tmpRkTEpR\spas_sage_attn-0.1.0\csrc\qattn\qk_int_sv_f16_cuda.cu -o C:\Users\qinglongshengzhe\AppData\Local\uv\cache\sdists-v8\.tmpRkTEpR\spas_sage_attn-0.1.0\build\temp.win-amd64-cpython-310\Release\csrc/qattn/qk_int_sv_f16_cuda.obj -D__CUDA_NO_HALF_OPERATORS__ -D__CUDA_NO_HALF_CONVERSIONS__ -D__CUDA_NO_BFLOAT16_CONVERSIONS__ -D__CUDA_NO_HALF2_OPERATORS__ --expt-relaxed-constexpr -O3 -std=c++17 -U__CUDA_NO_HALF_OPERATORS__ -U__CUDA_NO_HALF_CONVERSIONS__ --use_fast_math --threads=8 -Xptxas=-v -diag-suppress=174 -D_GLIBCXX_USE_CXX11_ABI=0 -gencode arch=compute_89,code=sm_89 -DTORCH_API_INCLUDE_EXTENSION_H -DTORCH_EXTENSION_NAME=_qattn -D_GLIBCXX_USE_CXX11_ABI=0
qk_int_sv_f16_cuda.cu
cl: ������ warning D9025 :������д��/D__CUDA_NO_HALF_OPERATORS__��(�á�/U__CUDA_NO_HALF_OPERATORS__��)
cl: ������ warning D9025 :������д��/D__CUDA_NO_HALF_CONVERSIONS__��(�á�/U__CUDA_NO_HALF_CONVERSIONS__��)
qk_int_sv_f16_cuda.cu
C:\Users\qinglongshengzhe\AppData\Local\uv\cache\sdists-v8\.tmpRkTEpR\spas_sage_attn-0.1.0\csrc\qattn\qk_int_sv_f16_cuda.cu(17): fatal error C1083: �޷��򿪰����ļ�: ��../utils.cuh��: No such file or directory
cl: ������ warning D9025 :������д��/D__CUDA_NO_HALF_OPERATORS__��(�á�/U__CUDA_NO_HALF_OPERATORS__��)
cl: ������ warning D9025 :������д��/D__CUDA_NO_HALF_CONVERSIONS__��(�á�/U__CUDA_NO_HALF_CONVERSIONS__��)
qk_int_sv_f16_cuda.cu
C:\Users\qinglongshengzhe\AppData\Local\uv\cache\sdists-v8\.tmpRkTEpR\spas_sage_attn-0.1.0\csrc\qattn\qk_int_sv_f16_cuda.cu(17): fatal error C1083: �޷��򿪰����ļ�: ��../utils.cuh��: No such file or directory
[3/3] cl /showIncludes /nologo /O2 /W3 /GL /DNDEBUG /MD /MD /wd4819 /wd4251 /wd4244 /wd4267 /wd4275 /wd4018 /wd4190 /wd4624 /wd4067 /wd4068 /EHsc -IE:\Python310\lib\site-packages\torch\include -IE:\Python310\lib\site-packages\torch\include\torch\csrc\api\include -IE:\Python310\lib\site-packages\torch\include\TH -IE:\Python310\lib\site-packages\torch\include\THC -IE:\cuda\12.6\include -IE:\cudnn\v9.6\include -IE:\Python310\include -IE:\Python310\Include "-IE:\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.41.34120\include" "-IE:\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.41.34120\ATLMFC\include" "-IE:\Microsoft Visual Studio\2022\Community\VC\Auxiliary\VS\include" "-IE:\Windows Kits\10\include\10.0.22621.0\ucrt" "-IE:\Windows Kits\10\\include\10.0.22621.0\\um" "-IE:\Windows Kits\10\\include\10.0.22621.0\\shared" "-IE:\Windows Kits\10\\include\10.0.22621.0\\winrt" "-IE:\Windows Kits\10\\include\10.0.22621.0\\cppwinrt" -c C:\Users\qinglongshengzhe\AppData\Local\uv\cache\sdists-v8\.tmpRkTEpR\spas_sage_attn-0.1.0\csrc\qattn\pybind.cpp /FoC:\Users\qinglongshengzhe\AppData\Local\uv\cache\sdists-v8\.tmpRkTEpR\spas_sage_attn-0.1.0\build\temp.win-amd64-cpython-310\Release\csrc/qattn/pybind.obj -g -O3 -fopenmp -lgomp -std=c++17 -DENABLE_BF16 -D_GLIBCXX_USE_CXX11_ABI=0 -DTORCH_API_INCLUDE_EXTENSION_H -DTORCH_EXTENSION_NAME=_qattn -D_GLIBCXX_USE_CXX11_ABI=0 /std:c++17
FAILED: C:/Users/qinglongshengzhe/AppData/Local/uv/cache/sdists-v8/.tmpRkTEpR/spas_sage_attn-0.1.0/build/temp.win-amd64-cpython-310/Release/csrc/qattn/pybind.obj
cl /showIncludes /nologo /O2 /W3 /GL /DNDEBUG /MD /MD /wd4819 /wd4251 /wd4244 /wd4267 /wd4275 /wd4018 /wd4190 /wd4624 /wd4067 /wd4068 /EHsc -IE:\Python310\lib\site-packages\torch\include -IE:\Python310\lib\site-packages\torch\include\torch\csrc\api\include -IE:\Python310\lib\site-packages\torch\include\TH -IE:\Python310\lib\site-packages\torch\include\THC -IE:\cuda\12.6\include -IE:\cudnn\v9.6\include -IE:\Python310\include -IE:\Python310\Include "-IE:\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.41.34120\include" "-IE:\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.41.34120\ATLMFC\include" "-IE:\Microsoft Visual Studio\2022\Community\VC\Auxiliary\VS\include" "-IE:\Windows Kits\10\include\10.0.22621.0\ucrt" "-IE:\Windows Kits\10\\include\10.0.22621.0\\um" "-IE:\Windows Kits\10\\include\10.0.22621.0\\shared" "-IE:\Windows Kits\10\\include\10.0.22621.0\\winrt" "-IE:\Windows Kits\10\\include\10.0.22621.0\\cppwinrt" -c C:\Users\qinglongshengzhe\AppData\Local\uv\cache\sdists-v8\.tmpRkTEpR\spas_sage_attn-0.1.0\csrc\qattn\pybind.cpp /FoC:\Users\qinglongshengzhe\AppData\Local\uv\cache\sdists-v8\.tmpRkTEpR\spas_sage_attn-0.1.0\build\temp.win-amd64-cpython-310\Release\csrc/qattn/pybind.obj -g -O3 -fopenmp -lgomp -std=c++17 -DENABLE_BF16 -D_GLIBCXX_USE_CXX11_ABI=0 -DTORCH_API_INCLUDE_EXTENSION_H -DTORCH_EXTENSION_NAME=_qattn -D_GLIBCXX_USE_CXX11_ABI=0 /std:c++17
```

---

_Comment by @sdbds on 2025-03-24 11:37_

any new process？

---

_Assigned to @konstin by @zanieb on 2025-04-01 18:23_

---

_Unassigned @konstin by @konstin on 2025-04-15 12:36_

---

_Comment by @konstin on 2025-04-15 12:37_

I don't have a Windows CUDA machine I could test with. Can you try setting the env var `PYTHONUTF8` to `1`? uv assumes all subprocess output to be UTF8, which may not be the case when Python is not running in UTF8 mode.

---

_Comment by @sdbds on 2025-04-18 02:58_

> I don't have a Windows CUDA machine I could test with. Can you try setting the env var `PYTHONUTF8` to `1`? uv assumes all subprocess output to be UTF8, which may not be the case when Python is not running in UTF8 mode.

Thank you for your hint, I tested and still cannot recognize...

It seems to be a problem caused by cl.exe rather than Python, so the cl command must be specified to use UTF8.

UseUnicodeForAssemblerListing=True

However, it seems that it is not possible to affect the UV compiler initiated by the external environment

---

_Comment by @sdbds on 2025-04-22 10:54_

> I don't have a Windows CUDA machine I could test with. Can you try setting the env var `PYTHONUTF8` to `1`? uv assumes all subprocess output to be UTF8, which may not be the case when Python is not running in UTF8 mode.

Is there a way to specify the cl compiler to use UTF-8 environment?

---

_Comment by @konstin on 2025-04-22 11:27_

I don't know about the cl compiler setting unfortunately, I only know about the uv's build frontend side of it.

---
