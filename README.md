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

- >Cannot open include file: 'wil/cppwinrt.h': No such file or directory

- Run `nuget` not found
```
winget install Microsoft.NuGet
cd d:\Projects\GitHub\ChrisTorng\MinimalCrop
nuget restore -SolutionDirectory .
```

- NuGet restored:
  - `packages\Microsoft.Windows.CppWinRT.2.0.240111.5`
  - `packages\Microsoft.Windows.ImplementationLibrary.1.0.231216.1`
  - `packages\robmikh.common.0.0.23-beta`

- Search for `..\` and `../`
`
- `CropAndLock.vcxproj`:
  - Replace `..\\..\\..\\..\\` to `.\\` in 
  - Remove reference projects:
    - common\logger\logger.vcxproj
    - common\SettingsAPI\SettingsAPI.vcxproj
    - common\Telemetry\EtwTrace\EtwTrace.vcxproj

- `CropAndLock.vcxproj.filters`
  - Comment out `<Natvis Include="$(MSBuildThisFileDirectory)..\..\natvis\wil.natvis" />`

- `.vscode\settings.json`:
    ```
    "C_Cpp_Runner.compilerArgs": [ "/std:c++17" ],
    "C_Cpp_Runner.includePaths": [
        ".\\packages\\Microsoft.Windows.ImplementationLibrary.1.0.231216.1\\include",
        ".\\packages\\robmikh.common.0.0.23-beta\\include"
    ],
    ```

- Comment out not used `trace.cpp` and `trace.h` files

- Comment out not used `logger`/`Settings`/`trace` lines

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
