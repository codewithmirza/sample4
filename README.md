# Compiling libclang from the llvm repository to WASM.

## Prerequisites
- Install Emscripten: Ensure you have Emscripten installed and set up on your system. You can download and install it from the official Emscripten website.
- Clone the llvm-project from LLVM Repository.
- Install Ninja `sudo install Ninja`

## Steps

1. **Create Build Directory**:
   - Before configuring LLVM/Clang, create a build directory named `build.emscripten`:
     ```
     mkdir build.emscripten
     ```

2. **Configure LLVM/Clang with Emscripten**:
   - Run the following command to configure LLVM/Clang with Emscripten:
     ```
     emcmake cmake -S llvm -B build.emscripten -G Ninja -DCMAKE_BUILD_TYPE=Release -DLLVM_ENABLE_PROJECTS='llvm;clang;clang-tools-extra' -DCMAKE_EXPORT_COMPILE_COMMANDS=on -DCMAKE_LINKER="/mnt/d/emscripten/emsdk/upstream/bin/lld" -DLLVM_TARGET_TRIPLE="wasm32-unknown-emscripten"
     ```

3. **Build LLVM/Clang with Emscripten**:
   - Run the following command to build LLVM/Clang with Emscripten:
     ```
     cmake --build build.emscripten --target libclang
     ```

4. **Extract `libclang.a`**:
   - After the build process is complete, extract `libclang.a` from the LLVM/Clang build directory:
     ```
     ar x build.emscripten/lib/libclang.a
     ```

5. **Compile `libclang.a` to `libclang.wasm`**:
   - Change to the lib directory and use Emscripten's `emcc` compiler to compile `libclang.a` to WebAssembly:
     ```
     cd build.emscripten/lib
     emcc -o libclang.wasm *.o --no-entry
     ```

6. **Locate `libclang.wasm`**:
   - After compiling, you can find the resulting `libclang.wasm` file in the `build.emscripten/lib` directory.

## Additional Notes
- Be sure to check the Emscripten documentation for any specific configuration options or considerations when compiling C/C++ code to WebAssembly.
- Experiment with different build configurations and options to optimize performance and compatibility for your specific use case.

## Files in the `libclang` Folder Before Process
```
Directory: D:\llvm-project\clang\tools\libclang

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----                     ARCMigrate.cpp
-a----                     BuildSystem.cpp
-a----                   CIndex.cpp
-a----                  CIndexCodeCompletion.cpp
-a----                    CIndexCXX.cpp
...
...
-a----                     CXType.cpp
-a----                     CXType.h
-a----              FatalErrorHandler.cpp
-a----                    Indexing.cpp
-a----                    Index_Internal.h
-a----                    libclang.map
-a----          linker-script-to-export-list.py
-a----                     Rewrite.cpp
```
## Files in `build.emscripten/lib` folder after build/configuration process
```
Directory: D:\llvm-project\clang\tools\libclang

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----                     ARCMigrate.cpp.o
-a----                     BuildSystem.cpp.o
-a----                   CIndex.cpp.o
-a----                  CIndexCodeCompletion.cpp.o
-a----                    CIndexCXX.cpp.o
...
...
-a----                     CXType.cpp.o
-a----                     CXType.h.o
-a----              FatalErrorHandler.cpp.o
-a----                    Indexing.cpp.o
-a----                    Index_Internal.h.o
-a----                    libclang.map.o
-a----          linker-script-to-export-list.py.o
-a----                     Rewrite.cpp.o
```
