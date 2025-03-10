# MinimalCrop

## VSCode C++

- Install [C/C++ Runner](https://marketplace.visualstudio.com/items?itemName=franneck94.c-cpp-runner) - Restart VS Code

- Open C++ folder, it will create `.vscode\settings.json`
  ```
  "C_Cpp_Runner.msvcBatchPath": "C:\\Program Files (x86)\\Microsoft Visual Studio\\2019\\BuildTools\\VC\\Auxiliary\\Build\\vcvarsall.bat",
  "C_Cpp_Runner.useMsvc": true,
  ```

- Bottom footer - Gear icon (Start Compilation)

## MinimalCrop

- [Download GitHub directory](https://download-directory.github.io/) - https://github.com/microsoft/PowerToys/tree/main/src/modules/CropAndLock

> Cannot open include file: 'wil/cppwinrt.h': No such file or directory

- Run `nuget` not found:
  ```cmd
  winget install Microsoft.NuGet
  cd MinimalCrop\
  nuget restore -SolutionDirectory .
  ```

- NuGet restored:
  - `packages\Microsoft.Windows.CppWinRT.2.0.240111.5`
  - `packages\Microsoft.Windows.ImplementationLibrary.1.0.231216.1`
  - `packages\robmikh.common.0.0.23-beta`

- Search for `..\` and `../` yields:
  `
  - `CropAndLock.vcxproj`:
    - Replace `..\\..\\..\\..\\` to `.\\` in 
    - Remove referenced projects:
      - `common\logger\logger.vcxproj`
      - `common\SettingsAPI\SettingsAPI.vcxproj`
      - `common\Telemetry\EtwTrace\EtwTrace.vcxproj`
    - Replace `10.0.22621.0` into `10.0.19041.0` to match local Windows 10 SDK version. The minimal version is `10.0.19041.0`. Check the `C:\Program Files (x86)\Windows Kits\10\bin` to see which version is installed.
      ```xml
      <WindowsTargetPlatformVersion Condition=" '$(WindowsTargetPlatformVersion)' == '' ">10.0.19041.0</WindowsTargetPlatformVersion>
      <WindowsTargetPlatformMinVersion>10.0.19041.0</WindowsTargetPlatformMinVersion>
      ```

  - `CropAndLock.vcxproj.filters`:
    - Comment out `<Natvis Include="$(MSBuildThisFileDirectory)..\..\natvis\wil.natvis" />`

- `.vscode\settings.json`:
  ```
  "C_Cpp_Runner.compilerArgs": [
    "/std:c++17",
    "/DUNICODE",
    "/D_UNICODE"
  ],
  "C_Cpp_Runner.includePaths": [
      ".\\packages\\Microsoft.Windows.ImplementationLibrary.1.0.231216.1\\include",
      ".\\packages\\robmikh.common.0.0.23-beta\\include"
  ],
  ```

- Comment out all content of not used `trace.cpp` and `trace.h` files.

- Comment out not used `logger`/`Settings`/`trace` related lines.

- Download referneced `.h` files:
  - [PowerToys/src/common/interop](https://github.com/microsoft/PowerToys/tree/v0.89.0/src/common/interop)
    - `shared_constants.h`
  - [PowerToys/src/common/utils](https://github.com/microsoft/PowerToys/tree/v0.89.0/src/common/utils)
    - `gpo.h`
    - `ProcessWaiter.h`
    - `UnhandledExceptionHandler.h`
    - `winapi_error.h`
  - [PowerToys/src/common/version](https://github.com/microsoft/PowerToys/blob/main/src/common/version)
    - `version.h`

# Current errors left

> `main.cpp` L10:<br>
  fatal error C1083: Cannot open include file: 'common/interop/shared_constants.h': No such file or directory

> `C:\Program Files (x86)\Windows Kits\10\include\10.0.19041.0\cppwinrt\winrt/impl/Windows.Foundation.0.h` L983/1004/1038/1057:<br>
  error C2039: 'wait_for': is not a member of 'winrt::impl'
