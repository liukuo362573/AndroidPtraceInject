# Android Ptrace Inject

![](https://img.shields.io/badge/Android-Build-green)
![](https://img.shields.io/badge/Android%204~12-Support-green)
![](https://img.shields.io/badge/arm64--v8a-Support-green)
![](https://img.shields.io/badge/x86-Support-green)

> 中文可以参考我的注释内容进行理解 <br>
> 我写的注释相对来说比较全面了

## How to build

- Make sure you have `CMake` and `Ninja` in your PATH

- Edit CMakeLists.txt. Set [`ANDROID_NDK`](https://github.com/SsageParuders/AndroidPtraceInject/blob/dfcee37e0c302d70e81c323c326e21c7f9bfa08e/CMakeLists.txt#L12) and [`CMAKE_TOOLCHAIN_FILE`](https://github.com/SsageParuders/AndroidPtraceInject/blob/dfcee37e0c302d70e81c323c326e21c7f9bfa08e/CMakeLists.txt#L13) for yourself.

- If your OS is Windows, read `build.sh` and make it to be a `build.bat`.

  Also, don't forget to change [`CMAKE_RUNTIME_OUTPUT_DIRECTORY`](https://github.com/SsageParuders/AndroidPtraceInject/blob/dfcee37e0c302d70e81c323c326e21c7f9bfa08e/Inject/CMakeLists.txt#L4) and [`CMAKE_LIBRARY_OUTPUT_DIRECTORY`](https://github.com/SsageParuders/AndroidPtraceInject/blob/dfcee37e0c302d70e81c323c326e21c7f9bfa08e/Hook/CMakeLists.txt#L4) in Windows styles.

- If you want to build all ABIs, u can annotation [ANDROID_ABI on CMakeLists.txt](https://github.com/SsageParuders/AndroidPtraceInject/blob/dfcee37e0c302d70e81c323c326e21c7f9bfa08e/CMakeLists.txt#L7), and run build.sh
  ```shell
  git clone https://github.com/SsageParuders/AndroidPtraceInject.git
  cd AndroidPtraceInject
  mkdir build
  chmod +x build.sh
  ./build.sh
  ```
- Or you can don't run build.sh and don't annotation [ANDROID_ABI on CMakeLists.txt](https://github.com/SsageParuders/AndroidPtraceInject/blob/dfcee37e0c302d70e81c323c326e21c7f9bfa08e/CMakeLists.txt#L7).
  ```shell
  git clone https://github.com/SsageParuders/AndroidPtraceInject.git
  cd AndroidPtraceInject
  mkdir build
  cd build
  cmake .. -G Ninja
  cmake --build .
  ```

## How to use

```shell
# Here are the parameters of the Inject command line tool:
#   Some parameters are optional.
#   -p process 's pid <-- optional
#   -n process 's package name <-- optional
#   -f whether to start App <-- optional
#   ---- //  /data/local/tmp/Inject -f -n XXX <-- error
#   ---- //  cd /data/local/tmp && ./Inject -f -n XXX <-- right
#   -so so path for injection <-- mandatory
#   -symbols specify a symbol in so <-- optional
# For examples:
cd /data/local/tmp && ./Inject -f -n bin.mt.plus -so /data/local/tmp/libHook.so -symbols hello
```

## 运行结果
```
gemini:/ # cd /data/local/tmp && ./Inject -f -n com.friday.sotest -so /data/local/tmp/libHook.so -symbols hello
m.friday.sotest -so /data/local/tmp/libHook.so -symbols hello                 <
[+] libc_path is /system/lib64/libc.so
[+] linker_path is /system/bin/linker64
[+] libdl_path is /system/lib64/libdl.so
[+] system libs is OK
[+] handle_selinux_init is OK
[+] Start Inject
[+] app_start_activity is com.friday.sotest/.MainActivity
[+] am start com.friday.sotest/.MainActivity
Starting: Intent { act=android.intent.action.MAIN cat=[android.intent.category.LAUNCHER] cmp=com.friday.sotest/.MainActivity }
Warning: Activity not started, its current task has been brought to the front
[+] pkg_name is com.friday.sotest
[+] get_pid_by_name pid is 9215
[+] lib_path is /data/local/tmp/libHook.so
[+] symbols is hello
[+] handle_parameter is OK
[-] SELinux is Enforcing
[+] Selinux has been changed to Permissive
[+] attach porcess success, pid:9215
[+] [get_remote_func_addr] lmod=0x79A66E1000, rmod=0x773A8E1000, lfunc=0x79A674A798, rfunc=0x773A94A798
[+] mmap RemoteFuncAddr:0x773a94a798
[+] ptrace continue process success, pid:9215
[+] ptrace call ret status is 1151
[+] ptrace_call mmap success, return value=77399A3000, pc=773A8E1000
[+] Remote Process Map Memory Addr:0x77399a3000
[+] linker_path value:/system/bin/linker64
[+] [get_remote_func_addr] lmod=0x79A666B000, rmod=0x773AD5D000, lfunc=0x79A666C0C0, rfunc=0x773AD5E0C0
[+] dlopen RemoteFuncAddr:0x773ad5e0c0
[+] [get_remote_func_addr] lmod=0x79A666B000, rmod=0x773AD5D000, lfunc=0x79A666C0EC, rfunc=0x773AD5E0EC
[+] dlsym RemoteFuncAddr:0x773ad5e0ec
[+] [get_remote_func_addr] lmod=0x79A666B000, rmod=0x773AD5D000, lfunc=0x79A666C130, rfunc=0x773AD5E130
[+] dlclose RemoteFuncAddr:0x773ad5e130
[+] [get_remote_func_addr] lmod=0x79A666B000, rmod=0x773AD5D000, lfunc=0x79A666C0D8, rfunc=0x773AD5E0D8
[+] dlerror RemoteFuncAddr:0x773ad5e0d8
[+] Get imports: dlopen: 3ad5e0c0, dlsym: 3ad5e0ec, dlclose: 3ad5e130, dlerror: 3ad5e0d8
[+] LibPath = /data/local/tmp/libHook.so
[+] ptrace continue process success, pid:9215
[+] ptrace call ret status is 1151
[+] ptrace_call dlopen success, Remote Process load module Addr:0x62a5f9ee0727c1f
[+] func symbols is hello
[+] Have func !!
[+] ptrace continue process success, pid:9215
[+] ptrace call ret status is 1151
[+] ptrace_call dlsym success, Remote Process ModuleFunc Addr:0x771d08464c
[+] ptrace continue process success, pid:9215
[+] ptrace call ret status is 1151
[+] Recover Regs Success
[+] detach process success, pid:9215
[+] SELinux has been rec
[+] Finish Inject
```

# TODO LIST

## Finished

- [x] First Inject Succeeded

- [x] Handle Parameter

- [x] Handle SELinux

- [x] Handle Libs

- [x] Succeed Inject for arm64-v8a

- [x] Succeed Inject for Android 9 and Android 11

- [x] Adapt to all Android versions

- [x] Succeed Inject for x86

## Future

- [ ] Fix bugs for armeabi-v7a

- [ ] Adapt to the ABIs of each device, such as armeabi-v7a and x86_64

# Credits

[Adrill](https://github.com/mustime/Adrill) By mustime

[SharkInject](https://github.com/bigGreenPeople/SharkInject) By bigGreenPeople

[androidinject](https://github.com/mergerly/androidinject) By mergerly

[TinyInjector](https://github.com/shunix/TinyInjector) By shunix

[injectvm-binderjack](https://github.com/Chainfire/injectvm-binderjack) By Chainfire
