# Setting Up and Running SPEC CPU 2017 Benchmarks on Windows

## Step 1: Prepare Your System
1. **Run Windows Updates**:
   - Make sure your system is up-to-date before starting the benchmark setup to avoid unexpected restarts during the benchmark run.

2. **Install Required Software**:
   I used `scoop` package manager to install the following utilities:
   ```bash
   scoop install 7zip make git swig ccache llvm@18.1.8 cmake ninja python@3.11.9 perl
   ```

3. **Install Visual Studio**:
   Install Visual Studio 2022 with the following components:
   ```json
   {
     "version": "1.0",
     "components": [
       "Microsoft.VisualStudio.Component.CoreEditor",
       "Microsoft.VisualStudio.Workload.CoreEditor",
       "Microsoft.VisualStudio.Component.Roslyn.Compiler",
       "Microsoft.Component.MSBuild",
       "Microsoft.VisualStudio.Component.TextTemplating",
       "Microsoft.VisualStudio.Component.VC.CoreIde",
       "Microsoft.VisualStudio.Component.VC.Tools.x86.x64",
       "Microsoft.VisualStudio.Component.VC.ATL",
       "Microsoft.VisualStudio.Component.VC.Redist.14.Latest",
       "Microsoft.VisualStudio.ComponentGroup.NativeDesktop.Core",
       "Microsoft.VisualStudio.Component.Windows10SDK.19041",
       "Microsoft.VisualStudio.Component.VC.Tools.ARM64",
       "Microsoft.VisualStudio.Component.VC.Tools.ARM",
       "Microsoft.VisualStudio.Workload.NativeDesktop",
       "Microsoft.VisualStudio.Component.VC.ATL.ARM",
       "Microsoft.VisualStudio.Component.VC.ATL.ARM64"
     ]
   }
   ```

## Step 2: Install SPEC CPU 2017
1. **Obtain the SPEC CPU 2017 Benchmark Source**.
2. **Unzip the Sources** to a preferred location on your system.
3. **Run the Installation Script**:
   - Navigate to the SPEC source directory and run:
     ```
     install.bat
     ```
4. **Verify Successful Installation**:
   - After installation, you should see:
     ```
     Runcpu tests completed successfully!
     Installation completed!
     ```

5. **Add the `bin` Directory to the System PATH**:
   ```bash
   set PATH=%PATH%;<SPEC CPU 2017 Source Dir>\bin
   ```

## Step 3: Configure Visual Studio Environment
1. For Arm64 machines, run:
   ```bash
   "c:\Program Files\Microsoft Visual Studio\2022\Community\VC\Auxiliary\Build\vcvarsarm64.bat"
   ```

2. For x64 machines, run:
   ```bash
   "c:\Program Files\Microsoft Visual Studio\2022\Community\VC\Auxiliary\Build\vcvarsamd64.bat"
   ```

## Step 4: Set Up the SPEC Configuration File
1. **Copy a Sample Configuration** from the SPEC CPU Source `config` directory.
2. **Modify the Configuration, For Example**:
   - Set up configurations for compiler `clang-cl` and `cl`.
   - Add architecture flag `armv8.7` to each configuration.
   - Define optimization flags for `/Od`, `/Os`, and `/Ot`.
   - Set `copies` and `threads` to desired level.


## Step 5: Execute the Benchmarks
1. **Run Benchmarks**:
   Use the following command to run the benchmarks:
   ```bash
   runcpu --config=<your_config_file.cfg> --iterations=<no-of-iterations>
   ```

2. **Monitor and Validate Results**:
   Check the output directory for `raw` and `report` files to ensure everything ran successfully.

## Step 6: Analyze the Results
1. Review the generated reports for performance comparisons.
2. Collect data on execution time, throughput, and optimization effects for each compiler and configuration.
