# Building OpenBLAS with LLVM for Windows on ARM64

This guide provides instructions to build [OpenBLAS](https://github.com/OpenMathLib/OpenBLAS) for Windows on ARM64.

## Prerequisites

- **Windows on ARM64 device**
- **Scoop**: A command-line installer for Windows

## Steps

### 1. Install Scoop

If you don't have Scoop installed, open **PowerShell** as **Administrator** and run:

```powershell
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
iwr -useb get.scoop.sh | iex
```

This installs Scoop and set the execution policy to allow script execution.

### 2. Use Scoop to Install Git, CMake, and Ninja

Install the required tools via Scoop:

```powershell
scoop install git cmake ninja
```

These tools will be added to your system PATH automatically.

### 3. Install LLVM

Download and install the latest LLVM for Windows on ARM64:

- **LLVM-19.1.0-woa64.exe**: [Download Link](https://snapshots.linaro.org/llvm-toolchain/19/19.1.0/LLVM-19.1.0-woa64.exe)

During installation, select the option to add LLVM to your system PATH.

### 4. Set Up the Environment

Open a **Command Prompt** and run the following command to set up the ARM64 Visual Studio environment:

```cmd
"C:\Program Files\Microsoft Visual Studio\2022\Community\VC\Auxiliary\Build\vcvarsarm64.bat"
```

This initializes the environment variables required for ARM64 compilation.

**Note**: If you have a different edition of Visual Studio (e.g., Professional or Enterprise) or installed it in a custom location, adjust the path accordingly.

### 5. Clone OpenBLAS Repository

In the command prompt, run:

```bash
git clone https://github.com/OpenMathLib/OpenBLAS.git
```

### 6. Create Build Directory

Navigate to the OpenBLAS directory and create a build folder:

```bash
cd OpenBLAS
mkdir build
cd build
```

### 7. Configure CMake

Run CMake with the following command:

```bash
cmake .. -G Ninja ^
  -DCMAKE_BUILD_TYPE=Release ^
  -DTARGET=ARMV8 ^
  -DCMAKE_SYSTEM_NAME=Windows ^
  -DARCH=arm ^
  -DBINARY=64 ^
  -DCMAKE_SYSTEM_PROCESSOR=ARM64 ^
  -DCMAKE_C_COMPILER=clang-cl ^
  -DCMAKE_C_COMPILER_TARGET=arm64-pc-windows-msvc ^
  -DCMAKE_ASM_COMPILER_TARGET=arm64-pc-windows-msvc ^
  -DCMAKE_Fortran_COMPILER=flang-new
```

### 8. Build with Ninja

Start the build process:

```bash
ninja -j16
```

- Replace `16` with the number of CPU cores available for optimal performance.

### Note

- **Warnings**: You may encounter warnings during the build process. These can be safely ignored as long as the build completes successfully.

---

*Based on instructions from the [OpenBLAS Wiki](https://github.com/OpenMathLib/OpenBLAS/wiki/How-to-build-OpenBLAS-for-Windows-on-ARM64).*
